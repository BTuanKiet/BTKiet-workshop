---
title : "Triển khai"
date : 2026-07-10
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

Hướng dẫn này trình bày quy trình triển khai end-to-end hệ thống KTS Smart Agriculture bằng cách tự tay tạo từng dịch vụ AWS qua Console và CLI. Tất cả tài nguyên sử dụng region **ap-southeast-1** (Singapore) trừ khi có ghi chú khác.

**Thứ tự triển khai rất quan trọng** — thực hiện theo đúng trình tự vì các tài nguyên sau phụ thuộc vào tài nguyên trước.

---

## Bước 1 — Tạo WAF WebACL (us-east-1)

WAF WebACL phải được tạo tại **us-east-1** vì CloudFront chỉ chấp nhận WebACL có scope CLOUDFRONT được tạo tại region này.

Chuyển region AWS Console sang **US East (N. Virginia) us-east-1**, sau đó điều hướng đến **WAF & Shield** → **Web ACLs** → **Create web ACL**.

Cấu hình WebACL như sau:

- **Name:** `kts-smart-agri-waf-prod`
- **Resource type:** Amazon CloudFront distributions
- **Region:** US East (N. Virginia)

Thêm các rule theo thứ tự sau:

**Rule 1 — Giới hạn tốc độ:** Nhấn **Add rules** → **Add my own rules and rule groups** → **Rule builder**. Đặt tên `RateLimitRule`, loại **Rate-based rule**, giới hạn **1000**, kiểm tra theo **Source IP**. Đặt action là **Block**.

**Rule 2 — AWS Managed Common Rule Set:** Nhấn **Add rules** → **Add managed rule groups** → tìm `AWSManagedRulesCommonRuleSet` → Add to web ACL. Giữ override action là **None (use rule defaults)**.

**Rule 3 — AWS Managed Known Bad Inputs:** Lặp lại bước trên, tìm `AWSManagedRulesKnownBadInputsRuleSet` → Add to web ACL.

Đặt default action là **Allow**, sau đó nhấn **Create web ACL**.

Sau khi tạo xong, nhấn vào tên WebACL, vào tab **Overview** và sao chép **Web ACL ARN** — cần dùng ở Bước 13 (CloudFront).

![create waf](/images/5-Workshop/5.3-Implementation/waf.png)

---

## Bước 2 — Tạo Cognito User Pool

Chuyển region về **ap-southeast-1**. Điều hướng đến **Cognito** → **User pools** → **Create user pool**.

**Authentication providers:** Chọn **Email** làm phương thức đăng nhập.

**Password policy:**
- Độ dài tối thiểu: **8**
- Yêu cầu chữ hoa: ✅
- Yêu cầu chữ thường: ✅
- Yêu cầu chữ số: ✅
- Yêu cầu ký tự đặc biệt: ☐ (bỏ chọn)

**Multi-factor authentication:** No MFA.

**User account recovery:** Bật tự khôi phục tài khoản qua email.

**Email delivery:** Dùng email tích hợp sẵn của Cognito để gửi mã xác nhận.

**User pool name:** `kts-smart-agri-user-pool-prod`

**App client:** Ở bước **Integrate your app**, cấu hình:
- **App type:** Public client
- **App client name:** `kts-smart-agri-client-prod`
- **Client secret:** Không tạo client secret
- **Auth flows:** Bật `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH`, `ALLOW_USER_SRP_AUTH`

Nhấn **Create user pool**.

![create cognito](/images/5-Workshop/5.3-Implementation/cognito.png)

Sau khi tạo xong, ghi lại:
- **User Pool ID** (định dạng: `ap-southeast-1_XXXXXXXXX`)
- **App client ID** (trong tab App clients)

Hai giá trị này sẽ được điền vào `frontend-app/.env` ở bước sau.

---

## Bước 3 — Tạo SQS Queues

Điều hướng đến **SQS** → **Create queue**.

**Queue 1 — Dead Letter Queue (tạo trước):**
- **Type:** Standard
- **Name:** `kts-smart-agri-inference-dlq-prod`
- **Message retention period:** 14 ngày (1.209.600 giây)
- Giữ nguyên các cài đặt còn lại

Nhấn **Create queue** và sao chép ARN của DLQ.

![create sqs](/images/5-Workshop/5.3-Implementation/create_sqs.png)

**Queue 2 — Main inference queue:**
- **Type:** Standard
- **Name:** `kts-smart-agri-inference-prod`
- **Visibility timeout:** 120 giây
- **Message retention period:** 1 ngày (86.400 giây)
- **Dead-letter queue:** Bật → chọn `kts-smart-agri-inference-dlq-prod` → Maximum receives: **3**

Nhấn **Create queue**.

![all sqs](/images/5-Workshop/5.3-Implementation/sqs.png)

Sau khi tạo xong main queue, vào queue đó, chuyển sang tab **Access policy** và nhấn **Edit**. Thay thế policy bằng nội dung sau (thay `<ACCOUNT_ID>` bằng Account ID của bạn):

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

## Bước 4 — Tạo S3 Buckets

Điều hướng đến **S3** → **Create bucket**.

**Bucket 1 — Images bucket:**
- **Bucket name:** `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Bật tất cả bốn tùy chọn ✅
- **Bucket versioning:** Disabled

Sau khi tạo xong, vào bucket → tab **Permissions** → **Cross-origin resource sharing (CORS)** → **Edit**, dán nội dung sau:

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

Tiếp theo vào tab **Properties** → **Event notifications** → **Create event notification**:
- **Event name:** `SendToSQS`
- **Event types:** All object create events
- **Prefix filter:** `uploads/`
- **Destination:** SQS queue → chọn `kts-smart-agri-inference-prod`

![event notification S3](/images/5-Workshop/5.3-Implementation/event_notification_S3.png)

**Bucket 2 — Archive bucket:**
- **Bucket name:** `kts-smart-agri-archive-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Bật tất cả bốn tùy chọn ✅

Sau khi tạo xong, vào tab **Management** → **Lifecycle rules** → **Create lifecycle rule**:
- **Rule name:** `ExpireProcessedImages`
- **Scope:** Apply to all objects
- **Lifecycle rule actions:** Expire current versions of objects
- **Days after object creation:** **90**

![lifecycle rule S3](/images/5-Workshop/5.3-Implementation/lifecycle_rule_S3.png)

**Bucket 3 — CloudTrail bucket:**
- **Bucket name:** `kts-smart-agri-cloudtrail-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Bật tất cả bốn tùy chọn ✅

Sau khi tạo xong, vào **Permissions** → **Bucket policy** → **Edit**, dán nội dung sau:

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

## Bước 5 — Tạo DynamoDB Table

Điều hướng đến **DynamoDB** → **Tables** → **Create table**.

- **Table name:** `kts-smart-agri-diagnosis-prod`
- **Partition key:** `userId` (String)
- **Sort key:** `imageId` (String)
- **Table settings:** Customize → chế độ **On-demand**

![create dynamodb table](/images/5-Workshop/5.3-Implementation/create_dynamodb_table.png)

Sau khi table được tạo, vào tab **Indexes** → **Create index** (Global Secondary Index):

- **Partition key:** `userId` (String)
- **Sort key:** `createdAt` (String)
- **Index name:** `userId-createdAt-index`
- **Attribute projections:** All

![create dynamodb gsi](/images/5-Workshop/5.3-Implementation/create_dynamodb_gsi.png)

---

## Bước 6 — Tạo ECR Repository

Điều hướng đến **ECR** (Elastic Container Registry) → **Repositories** → **Create repository**.

- **Visibility:** Private
- **Repository name:** `kts-smart-agri-ai-inference-prod`
- **Image scan settings:** Bật **Scan on push**

Nhấn **Create repository** và sao chép Repository URI — cần dùng khi push Docker image.

![create ecr repository](/images/5-Workshop/5.3-Implementation/create_ecr_repository.png)

---

## Bước 7 — Build và Push Docker Image lên ECR

Thực hiện trên máy tính cục bộ.

**Bước 7.1 — Xác thực Docker với ECR**

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
```

**Bước 7.2 — Build Docker image**

```bash
cd cloud-backend/functions/ai-inference
docker build -t kts-smart-agri-ai-inference-prod .
```

**Bước 7.3 — Tag và push lên ECR**

```bash
docker tag kts-smart-agri-ai-inference-prod:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
```

![push image ecr](/images/5-Workshop/5.3-Implementation/push_image_ecr.png)

---

## Bước 8 — Tạo IAM Roles cho Lambda

Điều hướng đến **IAM** → **Roles** → **Create role**. Chọn **AWS service** → **Lambda** làm use case.

Tạo một role cho mỗi Lambda function theo danh sách dưới đây.

**Role 1: `kts-agri-presign-url-role-prod`**
Đính kèm managed policy **AWSLambdaBasicExecutionRole**, sau đó thêm inline policy:
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
Đính kèm **AWSLambdaBasicExecutionRole** và **AWSLambdaSQSQueueExecutionRole**, sau đó thêm inline policy:
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
Đính kèm **AWSLambdaBasicExecutionRole**, sau đó thêm inline policy:
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
Đính kèm **AWSLambdaBasicExecutionRole**, sau đó thêm inline policy cho phép `s3:*` trên images bucket và `dynamodb:*` trên diagnosis table.

**Role 5: `kts-agri-data-maintenance-scheduler-role-prod`**
Role này được sử dụng bởi **Amazon EventBridge Scheduler** để gọi Lambda theo lịch định kỳ, không phải execution role của Lambda.
Khi tạo role, chọn **AWS service** → **Scheduler** làm use case (hoặc sử dụng Custom trust policy với principal `scheduler.amazonaws.com`), sau đó thêm inline policy:
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
Role này sẽ được sử dụng làm **Execution role** khi tạo **EventBridge Schedule** cho Lambda `kts-smart-agri-data-maintenance-prod` ở Bước 9.

![iam role](/images/5-Workshop/5.3-Implementation/iam_role.png)

---

## Bước 9 — Tạo Lambda Functions

Điều hướng đến **Lambda** → **Functions** → **Create function**.

**Function 1 — presign-url:**
- **Author from scratch**
- **Function name:** `kts-smart-agri-presign-url-prod`
- **Runtime:** Python 3.13
- **Architecture:** x86_64
- **Execution role:** Use existing role → `kts-agri-presign-url-role-prod`

Sau khi tạo xong, upload code qua **Code source** → **Upload from** → **.zip file** (nén toàn bộ thư mục `presign-url/` bao gồm cả dependencies).

Đặt **Environment variables**:
- `IMAGES_BUCKET_NAME` = `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- `ENVIRONMENT` = `prod`
- `POWERTOOLS_SERVICE_NAME` = `kts-smart-agri`

Vào **General configuration** → đặt Timeout: **30 giây**.

![Lambda function presign-url](/images/5-Workshop/5.3-Implementation/Lambda_function_presign-url.png)

**Function 2 — ai-inference:**
- **Container image**
- **Function name:** `kts-smart-agri-ai-inference-prod`
- **Container image URI:** Browse ECR → chọn `kts-smart-agri-ai-inference-prod:latest`
- **Execution role:** Use existing role → `kts-agri-ai-inference-role-prod`

Vào **General configuration**:
- Timeout: **120 giây**
- Memory: **1536 MB**

Đặt **Environment variables**:
- `DYNAMODB_TABLE_NAME` = `kts-smart-agri-diagnosis-prod`
- `ARCHIVE_BUCKET_NAME` = `kts-smart-agri-archive-<ACCOUNT_ID>-prod`
- `ENVIRONMENT` = `prod`

Thêm **Trigger** → **SQS** → chọn `kts-smart-agri-inference-prod` → Batch size: **1** → Bật **Report batch item failures**.

![lambda ai-inference](/images/5-Workshop/5.3-Implementation/lambda_ai-inference.png)

**Function 3 — get-result:**
Tương tự Function 1, tên `kts-smart-agri-get-result-prod`, role `kts-agri-get-result-role-prod`, upload thư mục `functions/get-result/`.
Environment variable: `DYNAMODB_TABLE_NAME` = `kts-smart-agri-diagnosis-prod`

**Function 4 — data-maintenance:**
Tên `kts-smart-agri-data-maintenance-prod`, role `kts-agri-data-maintenance-role-prod`, upload thư mục `functions/data-maintenance/`.
- Timeout: **300 giây**
- Environment variables: `IMAGES_BUCKET_NAME`, `DYNAMODB_TABLE_NAME`, `RETENTION_DAYS` = `30`
- Thêm **Trigger** → **EventBridge (CloudWatch Events)** → Create new rule → Schedule expression: `cron(0 0 ? * SUN *)`

![four function kts-smart-agri](/images/5-Workshop/5.3-Implementation/four_function_kts-smart-agri.png)

---

## Bước 10 — Tạo API Gateway

Điều hướng đến **API Gateway** → **Create API** → **REST API** → **Build**.

- **API name:** `kts-smart-agri-api-prod`
- **API endpoint type:** Regional

![create api gateway](/images/5-Workshop/5.3-Implementation/create_api_gateway.png)

**Bước 10.1 — Tạo Cognito Authorizer**

Vào **Authorizers** → **Create authorizer**:
- **Name:** `CognitoAuthorizer`
- **Type:** Cognito
- **Cognito user pool:** chọn `kts-smart-agri-user-pool-prod`
- **Token source:** `Authorization`

![create cognito authorizer api gateway](/images/5-Workshop/5.3-Implementation/create_cognito_authorizer_api_gateway.png)

**Bước 10.2 — Tạo resources và methods**

Tạo các resource và method sau. Với mỗi method, đặt **Integration type** là **Lambda Function** và bật **Lambda Proxy Integration**.

| Resource | Method | Lambda | Xác thực |
|----------|--------|--------|----------|
| `/images/presign` | POST | `kts-smart-agri-presign-url-prod` | CognitoAuthorizer |
| `/images` | GET | `kts-smart-agri-get-result-prod` | CognitoAuthorizer |
| `/images/{imageId}/result` | GET | `kts-smart-agri-get-result-prod` | CognitoAuthorizer |

Với mỗi resource, tạo thêm method **OPTIONS** với **Mock integration** và thêm các response headers để hỗ trợ CORS:
- `Access-Control-Allow-Origin`: `'*'`
- `Access-Control-Allow-Headers`: `'Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Requested-With'`
- `Access-Control-Allow-Methods`: `'GET,POST,PUT,DELETE,OPTIONS'`

![create resources vs methods api gateway](/images/5-Workshop/5.3-Implementation/create_resources_vs_methods_api_gateway.png)

**Bước 10.3 — Deploy API**

Nhấn **Deploy API** → Tạo stage mới tên `prod`. Ghi lại **Invoke URL** — đây là API endpoint dùng trong file `.env` của frontend.

![deploy api](/images/5-Workshop/5.3-Implementation/deploy_api.png)

---

## Bước 11 — Tạo SNS Topic và CloudWatch Alarms

Điều hướng đến **SNS** → **Topics** → **Create topic**:
- **Type:** Standard
- **Name:** `kts-smart-agri-alerts-prod`

Sau khi tạo xong, nhấn **Create subscription**:
- **Protocol:** Email
- **Endpoint:** địa chỉ email quản trị viên của bạn

Kiểm tra hộp thư và nhấn link xác nhận trong email từ SNS.

![sns topic](/images/5-Workshop/5.3-Implementation/sns_topic.png)

Điều hướng đến **CloudWatch** → **Alarms** → **Create alarm**. Tạo ba alarm sau, tất cả đều có SNS action trỏ đến `kts-smart-agri-alerts-prod`:

**Alarm 1:** Metric **AWS/Lambda** → **Errors** → Function `kts-smart-agri-ai-inference-prod` → Period 5 phút → Threshold ≥ 1 → Tên `kts-agri-ai-inference-errors-prod`

**Alarm 2:** Tương tự nhưng cho function `kts-smart-agri-presign-url-prod` → Tên `kts-agri-presign-url-errors-prod`

**Alarm 3:** Metric **AWS/DynamoDB** → **SystemErrors** → Table `kts-smart-agri-diagnosis-prod` → Period 5 phút → Threshold ≥ 1 → Tên `kts-agri-dynamodb-system-errors-prod`

![cloudwatch alarms](/images/5-Workshop/5.3-Implementation/cloudwatch_alarms.png)

---

## Bước 12 — Tạo CloudTrail Trail

Điều hướng đến **CloudTrail** → **Trails** → **Create trail**:
- **Trail name:** `kts-smart-agri-trail-prod`
- **Storage location:** Use existing S3 bucket → `kts-smart-agri-cloudtrail-<ACCOUNT_ID>-prod`
- **Log file SSE-KMS encryption:** Disabled
- **CloudWatch Logs:** Disabled
- **Include global service events:** ✅ Bật
- **Multi-region trail:** Disabled

Trong phần **Data events**, thêm S3 data event:
- **S3 bucket:** `kts-smart-agri-images-<ACCOUNT_ID>-prod`
- **Read/Write:** All

![create cloudtrail](/images/5-Workshop/5.3-Implementation/create_cloudtrail.png)

---

## Bước 13 — Tạo CloudFront Distribution

Điều hướng đến **CloudFront** → **Distributions** → **Create distribution**.

**Origin:**
- **Origin domain:** endpoint API Gateway của stage `prod`
- **Origin path:** `/prod`
- **Protocol:** HTTPS only
- **Minimum origin SSL protocol:** TLSv1.2

**Default cache behavior:**
- **Viewer protocol policy:** Redirect HTTP to HTTPS
- **Allowed HTTP methods:** GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- **Cache policy:** CachingDisabled (dùng AWS managed policy)
- **Origin request policy:** AllViewerExceptHostHeader

**WAF:** Chọn Web ACL đã tạo ở Bước 1 (`kts-smart-agri-waf-prod`)

**Price class:** Use all edge locations

**Description:** `KTS Smart Agriculture - prod`

Nhấn **Create distribution** và chờ trạng thái chuyển từ **Deploying** sang **Enabled** (mất 5–10 phút).

![create_cloudFront_distribution](/images/5-Workshop/5.3-Implementation/create_cloudFront_distribution.png)

Sao chép **Distribution domain name** (định dạng: `xxxxxxxxxxxx.cloudfront.net`).
