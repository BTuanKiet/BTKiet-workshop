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

> `[SCREENSHOT — WAF Create web ACL page with name and resource type filled in]`

Add the following rules in order:

**Rule 1 — Rate limiting:** Click **Add rules** → **Add my own rules and rule groups** → **Rule builder**. Set name to `RateLimitRule`, type **Rate-based rule**, rate limit **1000**, inspect **Source IP**. Set action to **Block**.

**Rule 2 — AWS Managed Common Rule Set:** Click **Add rules** → **Add managed rule groups** → search `AWSManagedRulesCommonRuleSet` → Add to web ACL. Keep override action as **None (use rule defaults)**.

**Rule 3 — AWS Managed Known Bad Inputs:** Repeat the above, search `AWSManagedRulesKnownBadInputsRuleSet` → Add to web ACL.

Set default action to **Allow**, then click **Create web ACL**.

> `[SCREENSHOT — WAF console showing kts-smart-agri-waf-prod with status Active and 3 rules listed]`

After creation, click on the WebACL name, go to the **Overview** tab and copy the **Web ACL ARN** — it will be needed in Step 7 (CloudFront).

> `[SCREENSHOT — WAF WebACL overview tab showing the ARN value]`

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

> `[SCREENSHOT — Cognito User Pool creation page with name and email sign-in selected]`

**App client:** On the **Integrate your app** step, set:
- **App type:** Public client
- **App client name:** `kts-smart-agri-client-prod`
- **Client secret:** Don't generate a client secret
- **Auth flows:** Enable `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH`, `ALLOW_USER_SRP_AUTH`

Click **Create user pool**.

> `[SCREENSHOT — Cognito console showing the new user pool with status Active]`

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

> `[SCREENSHOT — SQS queue creation page for the DLQ]`

**Queue 2 — Main inference queue:**
- **Type:** Standard
- **Name:** `kts-smart-agri-inference-prod`
- **Visibility timeout:** 120 seconds
- **Message retention period:** 1 day (86400 seconds)
- **Dead-letter queue:** Enable → select `kts-smart-agri-inference-dlq-prod` → Maximum receives: **3**

Click **Create queue**.

> `[SCREENSHOT — SQS console showing both queues created]`

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

> `[SCREENSHOT — SQS access policy editor with the S3 permission policy entered]`

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

> `[SCREENSHOT — S3 CORS configuration editor with the PUT/HEAD rule]`

Then go to **Properties** tab → **Event notifications** → **Create event notification**:
- **Event name:** `SendToSQS`
- **Event types:** All object create events
- **Prefix filter:** `uploads/`
- **Destination:** SQS queue → select `kts-smart-agri-inference-prod`

> `[SCREENSHOT — S3 event notification configuration pointing to the SQS queue]`

**Bucket 2 — Archive bucket:**
- **Bucket name:** `kts-smart-agri-archive-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Enable all four options ✅

After creation, go to **Management** tab → **Lifecycle rules** → **Create lifecycle rule**:
- **Rule name:** `ExpireProcessedImages`
- **Scope:** Apply to all objects
- **Lifecycle rule actions:** Expire current versions of objects
- **Days after object creation:** **90**

> `[SCREENSHOT — S3 lifecycle rule set to expire objects after 90 days]`

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

> `[SCREENSHOT — S3 bucket policy editor for the CloudTrail bucket]`

---

## Step 5 — Create DynamoDB Table

Navigate to **DynamoDB** → **Tables** → **Create table**.

- **Table name:** `kts-smart-agri-diagnosis-prod`
- **Partition key:** `userId` (String)
- **Sort key:** `imageId` (String)
- **Table settings:** Customize → **On-demand** capacity mode

> `[SCREENSHOT — DynamoDB table creation with partition key userId and sort key imageId]`

After the table is created, go to **Indexes** tab → **Create index** (Global Secondary Index):

- **Partition key:** `userId` (String)
- **Sort key:** `createdAt` (String)
- **Index name:** `userId-createdAt-index`
- **Attribute projections:** All

> `[SCREENSHOT — DynamoDB GSI creation form with userId and createdAt keys]`

---

## Step 6 — Create ECR Repository

Navigate to **ECR** (Elastic Container Registry) → **Repositories** → **Create repository**.

- **Visibility:** Private
- **Repository name:** `kts-smart-agri-ai-inference-prod`
- **Image scan settings:** Enable **Scan on push**

Click **Create repository** and copy the **Repository URI** — needed when pushing the Docker image.

> `[SCREENSHOT — ECR repository created showing the URI in format <account>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod]`

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

> `[SCREENSHOT — terminal showing Docker build progress with "Successfully built" at the end]`

**Step 7.4 — Tag and push to ECR**

```bash
docker tag kts-smart-agri-ai-inference-prod:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
```

> `[SCREENSHOT — ECR console showing the pushed image with tag "latest" and size displayed]`

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

> `[SCREENSHOT — IAM Roles console showing all four kts-agri roles created]`

---

## Step 9 — Create Lambda Functions

Navigate to **Lambda** → **Functions** → **Create function**.

**Function 1 — presign-url:**
- **Author from scratch**
- **Function name:** `kts-smart-agri-presign-url-prod`
- **Runtime:** Python 3.13
- **Architecture:** x86_64
- **Execution role:** Use existing role → `kts-agri-presign-url-role-prod`

After creation, upload `functions/presign-url/app.py` via **Code source** → **Upload from** → **.zip file** (zip the entire `presign-url/` folder including dependencies).

Set **Environment variables**:
- `IMAGES_BUCKET_NAME` = `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- `ENVIRONMENT` = `prod`
- `POWERTOOLS_SERVICE_NAME` = `kts-smart-agri`

Set **General configuration** → Timeout: **30 seconds**.

> `[SCREENSHOT — Lambda function presign-url with code uploaded and environment variables set]`

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

> `[SCREENSHOT — Lambda ai-inference showing container image URI, 1536 MB memory, 120s timeout, and SQS trigger]`

**Function 3 — get-result:**
- Same as Function 1 but name `kts-smart-agri-get-result-prod`, role `kts-agri-get-result-role-prod`, upload `functions/get-result/`.
- Environment variable: `DYNAMODB_TABLE_NAME` = `kts-smart-agri-diagnosis-prod`

**Function 4 — data-maintenance:**
- Name `kts-smart-agri-data-maintenance-prod`, role `kts-agri-data-maintenance-role-prod`, upload `functions/data-maintenance/`.
- Timeout: **300 seconds**
- Environment variables: `IMAGES_BUCKET_NAME`, `DYNAMODB_TABLE_NAME`, `RETENTION_DAYS=30`
- Add **Trigger** → **EventBridge (CloudWatch Events)** → Create new rule → Schedule expression: `cron(0 0 ? * SUN *)`

> `[SCREENSHOT — Lambda console showing all four kts-smart-agri functions listed]`

---

## Step 10 — Create API Gateway

Navigate to **API Gateway** → **Create API** → **REST API** → **Build**.

- **API name:** `kts-smart-agri-api-prod`
- **API endpoint type:** Regional

> `[SCREENSHOT — API Gateway creation page with REST API and Regional endpoint selected]`

**Step 10.1 — Create Cognito Authorizer**

Go to **Authorizers** → **Create authorizer**:
- **Name:** `CognitoAuthorizer`
- **Type:** Cognito
- **Cognito user pool:** select `kts-smart-agri-user-pool-prod`
- **Token source:** `Authorization`

> `[SCREENSHOT — API Gateway authorizer created with Cognito user pool linked]`

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

> `[SCREENSHOT — API Gateway resource tree showing /images, /images/presign, and /images/{imageId}/result]`

**Step 10.3 — Deploy the API**

Click **Deploy API** → Create a new stage named `prod`. Note down the **Invoke URL** — this is the `ApiEndpoint` used in the frontend `.env`.

> `[SCREENSHOT — API Gateway stage editor showing the Invoke URL for the prod stage]`

---

## Step 11 — Create SNS Topic and CloudWatch Alarms

Navigate to **SNS** → **Topics** → **Create topic**:
- **Type:** Standard
- **Name:** `kts-smart-agri-alerts-prod`

After creation, click **Create subscription**:
- **Protocol:** Email
- **Endpoint:** your admin email address

Check your inbox and click the confirmation link in the SNS subscription email.

> `[SCREENSHOT — SNS topic with one email subscription showing status Confirmed]`

Navigate to **CloudWatch** → **Alarms** → **Create alarm**. Create the following three alarms, all with **SNS action** pointing to `kts-smart-agri-alerts-prod`:

**Alarm 1:** Metric: `AWS/Lambda` → `Errors` → Function `kts-smart-agri-ai-inference-prod` → Period 5 min → Threshold >= 1 → Name `kts-agri-ai-inference-errors-prod`

**Alarm 2:** Same as above but for function `kts-smart-agri-presign-url-prod` → Name `kts-agri-presign-url-errors-prod`

**Alarm 3:** Metric: `AWS/DynamoDB` → `SystemErrors` → Table `kts-smart-agri-diagnosis-prod` → Period 5 min → Threshold >= 1 → Name `kts-agri-dynamodb-system-errors-prod`

> `[SCREENSHOT — CloudWatch Alarms console showing all three alarms with status OK]`

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

> `[SCREENSHOT — CloudTrail trail creation page with the images S3 bucket selected as data event source]`

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

> `[SCREENSHOT — CloudFront distribution showing status Enabled with the domain name displayed]`

Copy the **Distribution domain name** (format: `xxxxxxxxxxxx.cloudfront.net`).

---

## Step 14 — Configure and Run the Frontend

On your local machine, navigate to `frontend-app/` and create a `.env` file:

```env
VITE_COGNITO_USER_POOL_ID=<User Pool ID from Step 2>
VITE_COGNITO_USER_POOL_CLIENT_ID=<App Client ID from Step 2>
VITE_API_BASE_URL=https://<API Gateway Invoke URL from Step 10>
```

Install dependencies and start the development server:

```bash
npm install
npm run dev
```

Open `http://localhost:5173` in your browser.

> `[SCREENSHOT — browser showing the KTS Smart Agriculture login page]`

---

## Step 15 — End-to-End Test

**15.1 — Register:** Click **Create account**, enter your email and a password meeting the requirements (min 8 chars, uppercase, lowercase, number). Submit the form.

> `[SCREENSHOT — registration form filled out]`

**15.2 — Verify email:** Check your inbox for the Cognito verification code and enter it on the confirmation screen.

> `[SCREENSHOT — email verification code input screen]`

**15.3 — Log in:** Enter your credentials on the login page.

> `[SCREENSHOT — dashboard after successful login]`

**15.4 — Upload a plant image:** Navigate to the Upload page, select a JPG/PNG photo of a plant leaf, and click Upload. The progress bar indicates the direct S3 upload.

> `[SCREENSHOT — upload page showing progress bar at 100%]`

**15.5 — View result:** After upload, the page redirects to the result view. The image passes through S3 → SQS → AI Inference Lambda → DynamoDB. On first cold start, loading the ResNet50 + LeNet models takes approximately 20–30 seconds. Click **Refresh** if the status shows "Processing".

> `[SCREENSHOT — result page showing disease class name and confidence percentage]`

**15.6 — Verify in DynamoDB:** Navigate to **DynamoDB → Tables → kts-smart-agri-diagnosis-prod → Explore items** to confirm the record has `status: COMPLETED` with disease and confidence values written.

> `[SCREENSHOT — DynamoDB item showing userId, imageId, status COMPLETED, disease name, and confidence]`
