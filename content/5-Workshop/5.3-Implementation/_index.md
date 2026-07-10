---
title : "Implementation"
date : 2026-07-10
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

This guide walks through the complete end-to-end deployment of the KTS Smart Agriculture system by manually creating each AWS service through the Console and CLI. All resources use the `ap-southeast-1` (Singapore) region unless otherwise specified.

**Deployment order matters** — follow the steps in sequence as later resources depend on earlier ones.

---

## Step 1 — Create WAF WebACL (us-east-1)

The WAF WebACL must be created in **us-east-1** because CloudFront only accepts WebACLs with `CLOUDFRONT` scope from that region.

Switch your AWS Console region to **US East (N. Virginia) us-east-1**, then navigate to **WAF & Shield** → **Web ACLs** → **Create web ACL**.

Configure the WebACL as follows:

- **Name:** `kts-smart-agri-waf-prod`
- **Resource type:** Amazon CloudFront distributions
- **Region:** US East (N. Virginia)

Add the following rules in order:

**Rule 1 — Rate limiting:** Click **Add rules** → **Add my own rules and rule groups** → **Rule builder**. Set name to `RateLimitRule`, type **Rate-based rule**, rate limit **1000**, inspect **Source IP**. Set action to **Block**.

**Rule 2 — AWS Managed Common Rule Set:** Click **Add rules** → **Add managed rule groups** → search `AWSManagedRulesCommonRuleSet` → Add to web ACL. Keep override action as **None (use rule defaults)**.

**Rule 3 — AWS Managed Known Bad Inputs:** Repeat the above, search `AWSManagedRulesKnownBadInputsRuleSet` → Add to web ACL.

Set default action to **Allow**, then click **Create web ACL**.

After creation, click on the WebACL name, go to the **Overview** tab and copy the **Web ACL ARN** — it will be needed in Step 13 (CloudFront).

![create waf](/images/5-Workshop/5.3-Implementation/waf.png)

---

## Step 2 — Create Cognito User Pool

Switch region back to **ap-southeast-1**. Navigate to **Cognito** → **User pools** → **Create user pool**.

**Authentication providers:** Select **Email** as the sign-in option.

**Password policy:**
- Minimum length: **8**
- Require uppercase: ✅
- Require lowercase: ✅
- Require numbers: ✅
- Require symbols: ☐ (unchecked)

**Multi-factor authentication:** No MFA.

**User account recovery:** Enable self-service account recovery via email.

**Email delivery:** Use Cognito's built-in email for sending verification codes.

**User pool name:** `kts-smart-agri-user-pool-prod`

**App client:** On the **Integrate your app** step, set:
- **App type:** Public client
- **App client name:** `kts-smart-agri-client-prod`
- **Client secret:** Don't generate a client secret
- **Auth flows:** Enable `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH`, `ALLOW_USER_SRP_AUTH`

Click **Create user pool**.

![create cognito](/images/5-Workshop/5.3-Implementation/cognito.png)

After creation, note down:
- **User Pool ID** (format: `ap-southeast-1_XXXXXXXXX`)
- **App client ID** (from the App clients tab)

These values go into `frontend-app/.env` later.

---

## Step 3 — Create SQS Queues

Navigate to **SQS** → **Create queue**.

**Queue 1 — Dead Letter Queue (create this first):**
- **Type:** Standard
- **Name:** `kts-smart-agri-inference-dlq-prod`
- **Message retention period:** 14 days (1209600 seconds)
- Leave all other settings as default

Click **Create queue** and copy the DLQ **ARN**.

![create sqs](/images/5-Workshop/5.3-Implementation/create_sqs.png)

**Queue 2 — Main inference queue:**
- **Type:** Standard
- **Name:** `kts-smart-agri-inference-prod`
- **Visibility timeout:** 120 seconds
- **Message retention period:** 1 day (86400 seconds)
- **Dead-letter queue:** Enable → select `kts-smart-agri-inference-dlq-prod` → Maximum receives: **3**

Click **Create queue**.

![all sqs](/images/5-Workshop/5.3-Implementation/sqs.png)

After creating the main queue, navigate to it, go to the **Access policy** tab and click **Edit**. Replace the policy with the following (substitute your Account ID and region):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3SendMessage",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "sqs:SendMessage",
      "Resource": "arn:aws:sqs:ap-southeast-1:<ACCOUNT_ID>:kts-smart-agri-inference-prod",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:::kts-smart-agri-images-<ACCOUNT_ID>-prod"
        }
      }
    }
  ]
}
```

![access policy sqs](/images/5-Workshop/5.3-Implementation/access_policy_sqs.png)

---

## Step 4 — Create S3 Buckets

Navigate to **S3** → **Create bucket**.

**Bucket 1 — Images bucket:**
- **Bucket name:** `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Enable all four options ✅
- **Bucket versioning:** Disabled

After creation, go to the bucket → **Permissions** tab → **Cross-origin resource sharing (CORS)** → **Edit**, and paste:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["PUT", "HEAD"],
    "AllowedOrigins": ["*"],
    "MaxAgeSeconds": 3000
  }
]
```

![edit cors S3](/images/5-Workshop/5.3-Implementation/edit_cors_S3.png)

Then go to **Properties** tab → **Event notifications** → **Create event notification**:
- **Event name:** `SendToSQS`
- **Event types:** All object create events
- **Prefix filter:** `uploads/`
- **Destination:** SQS queue → select `kts-smart-agri-inference-prod`

![event notification S3](/images/5-Workshop/5.3-Implementation/event_notification_S3.png)

**Bucket 2 — Archive bucket:**
- **Bucket name:** `kts-smart-agri-archive-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Enable all four options ✅

After creation, go to **Management** tab → **Lifecycle rules** → **Create lifecycle rule**:
- **Rule name:** `ExpireProcessedImages`
- **Scope:** Apply to all objects
- **Lifecycle rule actions:** Expire current versions of objects
- **Days after object creation:** **90**

![lifecycle rule S3](/images/5-Workshop/5.3-Implementation/lifecycle_rule_S3.png)

**Bucket 3 — CloudTrail bucket:**
- **Bucket name:** `kts-smart-agri-cloudtrail-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Enable all four options ✅

After creation, go to **Permissions** → **Bucket policy** → **Edit**, and paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AWSCloudTrailAclCheck",
      "Effect": "Allow",
      "Principal": { "Service": "cloudtrail.amazonaws.com" },
      "Action": "s3:GetBucketAcl",
      "Resource": "arn:aws:s3:::kts-smart-agri-cloudtrail-<ACCOUNT_ID>-prod"
    },
    {
      "Sid": "AWSCloudTrailWrite",
      "Effect": "Allow",
      "Principal": { "Service": "cloudtrail.amazonaws.com" },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::kts-smart-agri-cloudtrail-<ACCOUNT_ID>-prod/AWSLogs/<ACCOUNT_ID>/*",
      "Condition": {
        "StringEquals": { "s3:x-amz-acl": "bucket-owner-full-control" }
      }
    }
  ]
}
```

![bucket policy S3](/images/5-Workshop/5.3-Implementation/bucket_policy_S3.png)

---

## Step 5 — Create DynamoDB Table

Navigate to **DynamoDB** → **Tables** → **Create table**.

- **Table name:** `kts-smart-agri-diagnosis-prod`
- **Partition key:** `userId` (String)
- **Sort key:** `imageId` (String)
- **Table settings:** Customize → **On-demand** capacity mode

![create dynamodb table](/images/5-Workshop/5.3-Implementation/create_dynamodb_table.png)

After the table is created, go to **Indexes** tab → **Create index** (Global Secondary Index):

- **Partition key:** `userId` (String)
- **Sort key:** `createdAt` (String)
- **Index name:** `userId-createdAt-index`
- **Attribute projections:** All

![create dynamodb gsi](/images/5-Workshop/5.3-Implementation/create_dynamodb_gsi.png)

---

## Step 6 — Create ECR Repository

Navigate to **ECR** (Elastic Container Registry) → **Repositories** → **Create repository**.

- **Visibility:** Private
- **Repository name:** `kts-smart-agri-ai-inference-prod`
- **Image scan settings:** Enable **Scan on push**

Click **Create repository** and copy the **Repository URI** — needed when pushing the Docker image.

![create ecr repository](/images/5-Workshop/5.3-Implementation/create_ecr_repository.png)

---

## Step 7 — Build and Push Docker Image to ECR

On your local machine, copy the model files and build the Docker image.

**Step 7.1 — Copy model files**

```bash
copy ai-service\best_resnet_model.pth cloud-backend\functions\ai-inference\
copy ai-service\best_lenet_model.pth  cloud-backend\functions\ai-inference\
```

**Step 7.2 — Authenticate Docker to ECR**

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
```

**Step 7.3 — Build the Docker image**

```bash
cd cloud-backend/functions/ai-inference
docker build -t kts-smart-agri-ai-inference-prod .
```

**Step 7.4 — Tag and push to ECR**

```bash
docker tag kts-smart-agri-ai-inference-prod:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
```

![push image ecr](/images/5-Workshop/5.3-Implementation/push_image_ecr.png)

---

## Step 8 — Create IAM Roles for Lambda

Navigate to **IAM** → **Roles** → **Create role**. Select **AWS service** → **Lambda** as the use case.

Create one role for each Lambda function listed below. For each role, attach the stated policies.

**Role 1: `kts-agri-presign-url-role-prod`**
Attach the AWS managed policy `AWSLambdaBasicExecutionRole`, then add an inline policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:PutObject", "s3:PutObjectAcl"],
    "Resource": "arn:aws:s3:::kts-smart-agri-images-<ACCOUNT_ID>-prod/*"
  }]
}
```

**Role 2: `kts-agri-ai-inference-role-prod`**
Attach `AWSLambdaBasicExecutionRole` and `AWSLambdaSQSQueueExecutionRole`, then add an inline policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["dynamodb:PutItem", "dynamodb:UpdateItem", "dynamodb:GetItem"],
      "Resource": "arn:aws:dynamodb:ap-southeast-1:<ACCOUNT_ID>:table/kts-smart-agri-diagnosis-prod"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::kts-smart-agri-images-<ACCOUNT_ID>-prod/*"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject"],
      "Resource": "arn:aws:s3:::kts-smart-agri-archive-<ACCOUNT_ID>-prod/*"
    }
  ]
}
```

**Role 3: `kts-agri-get-result-role-prod`**
Attach `AWSLambdaBasicExecutionRole`, then add an inline policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["dynamodb:GetItem", "dynamodb:Query"],
    "Resource": [
      "arn:aws:dynamodb:ap-southeast-1:<ACCOUNT_ID>:table/kts-smart-agri-diagnosis-prod",
      "arn:aws:dynamodb:ap-southeast-1:<ACCOUNT_ID>:table/kts-smart-agri-diagnosis-prod/index/*"
    ]
  }]
}
```

**Role 4: `kts-agri-data-maintenance-role-prod`**
Attach `AWSLambdaBasicExecutionRole`, then add an inline policy allowing `s3:*` on the images bucket and `dynamodb:*` on the diagnosis table.

**Role 5: `kts-agri-data-maintenance-scheduler-role-prod`**
This role is used by **Amazon EventBridge Scheduler** to invoke Lambda on a schedule, not as a Lambda execution role.
When creating the role, select **AWS service** → **Scheduler** as the use case (or use a Custom trust policy with principal `scheduler.amazonaws.com`), then add an inline policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "lambda:InvokeFunction",
    "Resource": "arn:aws:lambda:ap-southeast-1:<ACCOUNT_ID>:function:kts-smart-agri-data-maintenance-prod"
  }]
}
```
This role will be used as the **Execution role** when creating the **EventBridge Schedule** for the `kts-smart-agri-data-maintenance-prod` Lambda in Step 9.

![iam role](/images/5-Workshop/5.3-Implementation/iam_role.png)

---

## Step 9 — Create Lambda Functions

Navigate to **Lambda** → **Functions** → **Create function**.

**Function 1 — presign-url:**
- **Author from scratch**
- **Function name:** `kts-smart-agri-presign-url-prod`
- **Runtime:** Python 3.13
- **Architecture:** x86_64
- **Execution role:** Use existing role → `kts-agri-presign-url-role-prod`

After creation, upload code via **Code source** → **Upload from** → **.zip file** (zip the entire `presign-url/` folder including dependencies).

Set **Environment variables**:
- `IMAGES_BUCKET_NAME` = `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- `ENVIRONMENT` = `prod`
- `POWERTOOLS_SERVICE_NAME` = `kts-smart-agri`

Set **General configuration** → Timeout: **30 seconds**.

![Lambda function presign-url](/images/5-Workshop/5.3-Implementation/Lambda_function_presign-url.png)

**Function 2 — ai-inference:**
- **Container image**
- **Function name:** `kts-smart-agri-ai-inference-prod`
- **Container image URI:** Browse ECR → select `kts-smart-agri-ai-inference-prod:latest`
- **Execution role:** Use existing role → `kts-agri-ai-inference-role-prod`

Set **General configuration**:
- Timeout: **120 seconds**
- Memory: **1536 MB**

Set **Environment variables**:
- `DYNAMODB_TABLE_NAME` = `kts-smart-agri-diagnosis-prod`
- `ARCHIVE_BUCKET_NAME` = `kts-smart-agri-archive-<ACCOUNT_ID>-prod`
- `ENVIRONMENT` = `prod`

Add **Trigger** → **SQS** → select `kts-smart-agri-inference-prod` → Batch size: **1** → Enable **Report batch item failures**.

![lambda ai-inference](/images/5-Workshop/5.3-Implementation/lambda_ai-inference.png)

**Function 3 — get-result:**
Same as Function 1 but name `kts-smart-agri-get-result-prod`, role `kts-agri-get-result-role-prod`, upload `functions/get-result/`.
Environment variable: `DYNAMODB_TABLE_NAME` = `kts-smart-agri-diagnosis-prod`

**Function 4 — data-maintenance:**
Name `kts-smart-agri-data-maintenance-prod`, role `kts-agri-data-maintenance-role-prod`, upload `functions/data-maintenance/`.
- Timeout: **300 seconds**
- Environment variables: `IMAGES_BUCKET_NAME`, `DYNAMODB_TABLE_NAME`, `RETENTION_DAYS=30`
- Add **Trigger** → **EventBridge (CloudWatch Events)** → Create new rule → Schedule expression: `cron(0 0 ? * SUN *)`

![four function kts-smart-agri](/images/5-Workshop/5.3-Implementation/four_function_kts-smart-agri.png)

---

## Step 10 — Create API Gateway

Navigate to **API Gateway** → **Create API** → **REST API** → **Build**.

- **API name:** `kts-smart-agri-api-prod`
- **API endpoint type:** Regional

![create api gateway](/images/5-Workshop/5.3-Implementation/create_api_gateway.png)

**Step 10.1 — Create Cognito Authorizer**

Go to **Authorizers** → **Create authorizer**:
- **Name:** `CognitoAuthorizer`
- **Type:** Cognito
- **Cognito user pool:** select `kts-smart-agri-user-pool-prod`
- **Token source:** `Authorization`

![create cognito authorizer api gateway](/images/5-Workshop/5.3-Implementation/create_cognito_authorizer_api_gateway.png)

**Step 10.2 — Create resources and methods**

Create the following resources and methods. For each method, set **Integration type** to **Lambda Function** and enable **Lambda Proxy Integration**.

| Resource | Method | Lambda | Auth |
|----------|--------|--------|------|
| `/images/presign` | POST | `kts-smart-agri-presign-url-prod` | CognitoAuthorizer |
| `/images` | GET | `kts-smart-agri-get-result-prod` | CognitoAuthorizer |
| `/images/{imageId}/result` | GET | `kts-smart-agri-get-result-prod` | CognitoAuthorizer |

For each resource, also create an **OPTIONS** method with **Mock integration** and add the following response headers to support CORS:
- `Access-Control-Allow-Origin: '*'`
- `Access-Control-Allow-Headers: 'Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Requested-With'`
- `Access-Control-Allow-Methods: 'GET,POST,PUT,DELETE,OPTIONS'`

![create resources vs methods api gateway](/images/5-Workshop/5.3-Implementation/create_resources_vs_methods_api_gateway.png)

**Step 10.3 — Deploy the API**

Click **Deploy API** → Create a new stage named `prod`. Note down the **Invoke URL** — this is the `ApiEndpoint` used in the frontend `.env`.

![deploy api](/images/5-Workshop/5.3-Implementation/deploy_api.png)

---

## Step 11 — Create SNS Topic and CloudWatch Alarms

Navigate to **SNS** → **Topics** → **Create topic**:
- **Type:** Standard
- **Name:** `kts-smart-agri-alerts-prod`

After creation, click **Create subscription**:
- **Protocol:** Email
- **Endpoint:** your admin email address

Check your inbox and click the confirmation link in the SNS subscription email.

![sns topic](/images/5-Workshop/5.3-Implementation/sns_topic.png)

Navigate to **CloudWatch** → **Alarms** → **Create alarm**. Create the following three alarms, all with **SNS action** pointing to `kts-smart-agri-alerts-prod`:

**Alarm 1:** Metric: `AWS/Lambda` → `Errors` → Function `kts-smart-agri-ai-inference-prod` → Period 5 min → Threshold >= 1 → Name `kts-agri-ai-inference-errors-prod`

**Alarm 2:** Same as above but for function `kts-smart-agri-presign-url-prod` → Name `kts-agri-presign-url-errors-prod`

**Alarm 3:** Metric: `AWS/DynamoDB` → `SystemErrors` → Table `kts-smart-agri-diagnosis-prod` → Period 5 min → Threshold >= 1 → Name `kts-agri-dynamodb-system-errors-prod`

![cloudwatch alarms](/images/5-Workshop/5.3-Implementation/cloudwatch_alarms.png)

---

## Step 12 — Create CloudTrail Trail

Navigate to **CloudTrail** → **Trails** → **Create trail**:
- **Trail name:** `kts-smart-agri-trail-prod`
- **Storage location:** Use existing S3 bucket → `kts-smart-agri-cloudtrail-<ACCOUNT_ID>-prod`
- **Log file SSE-KMS encryption:** Disabled (for simplicity)
- **CloudWatch Logs:** Disabled
- **Include global service events:** ✅ Enabled
- **Multi-region trail:** Disabled

Under **Data events**, add an S3 data event:
- **S3 bucket:** `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- **Read/Write:** All

![create cloudtrail](/images/5-Workshop/5.3-Implementation/create_cloudtrail.png)

---

## Step 13 — Create CloudFront Distribution

Navigate to **CloudFront** → **Distributions** → **Create distribution**.

**Origin:**
- **Origin domain:** `<API_ID>.execute-api.ap-southeast-1.amazonaws.com`
- **Origin path:** `/prod`
- **Protocol:** HTTPS only
- **Minimum origin SSL protocol:** TLSv1.2

**Default cache behavior:**
- **Viewer protocol policy:** Redirect HTTP to HTTPS
- **Allowed HTTP methods:** GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- **Cache policy:** `CachingDisabled` (use the AWS managed policy)
- **Origin request policy:** `AllViewerExceptHostHeader`

**WAF:** Select the WAF Web ACL created in Step 1 (`kts-smart-agri-waf-prod`)

**Price class:** Use all edge locations

**Description:** `KTS Smart Agriculture - prod`

Click **Create distribution** and wait for the status to change from **Deploying** to **Enabled** (takes 5–10 minutes).

![create_cloudFront_distribution](/images/5-Workshop/5.3-Implementation/create_cloudFront_distribution.png)

Copy the **Distribution domain name** (format: `xxxxxxxxxxxx.cloudfront.net`).
