---
title : "Triển khai"
date : 2026-07-10
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

Hướng dẫn này trình bày quy trình triển khai end-to-end hệ thống KTS Smart Agriculture bằng cách tự tay tạo từng dịch vụ AWS qua Console và CLI. Tất cả tài nguyên sử dụng region `ap-southeast-1` (Singapore) trừ khi có ghi chú khác.

**Thứ tự triển khai rất quan trọng** — thực hiện theo đúng trình tự vì các tài nguyên sau phụ thuộc vào tài nguyên trước.

---

## Bước 1 — Tạo WAF WebACL (us-east-1)

WAF WebACL phải được tạo tại **us-east-1** vì CloudFront chỉ chấp nhận WebACL có scope `CLOUDFRONT` được tạo tại region này.

Chuyển region AWS Console sang **US East (N. Virginia) us-east-1**, sau đó điều hướng đến **WAF & Shield** → **Web ACLs** → **Create web ACL**.

Cấu hình WebACL như sau:

- **Name:** `kts-smart-agri-waf-prod`
- **Resource type:** Amazon CloudFront distributions
- **Region:** US East (N. Virginia)

> `[SCREENSHOT — Trang tạo WAF Web ACL với tên và loại tài nguyên đã điền]`

Thêm các rule theo thứ tự sau:

**Rule 1 — Giới hạn tốc độ:** Nhấn **Add rules** → **Add my own rules and rule groups** → **Rule builder**. Đặt tên `RateLimitRule`, loại **Rate-based rule**, giới hạn **1000**, kiểm tra theo **Source IP**. Đặt action là **Block**.

**Rule 2 — AWS Managed Common Rule Set:** Nhấn **Add rules** → **Add managed rule groups** → tìm `AWSManagedRulesCommonRuleSet` → Add to web ACL. Giữ override action là **None (use rule defaults)**.

**Rule 3 — AWS Managed Known Bad Inputs:** Lặp lại bước trên, tìm `AWSManagedRulesKnownBadInputsRuleSet` → Add to web ACL.

Đặt default action là **Allow**, sau đó nhấn **Create web ACL**.

> `[SCREENSHOT — WAF console hiển thị kts-smart-agri-waf-prod với trạng thái Active và 3 rule]`

Sau khi tạo xong, nhấn vào tên WebACL, vào tab **Overview** và sao chép **Web ACL ARN** — cần dùng ở Bước 13 (CloudFront).

> `[SCREENSHOT — Tab Overview của WAF WebACL hiển thị giá trị ARN]`

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

> `[SCREENSHOT — Trang tạo Cognito User Pool với tên và đăng nhập bằng email đã chọn]`

**App client:** Ở bước **Integrate your app**, cấu hình:
- **App type:** Public client
- **App client name:** `kts-smart-agri-client-prod`
- **Client secret:** Không tạo client secret
- **Auth flows:** Bật `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH`, `ALLOW_USER_SRP_AUTH`

Nhấn **Create user pool**.

> `[SCREENSHOT — Cognito console hiển thị user pool mới với trạng thái Active]`

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
- **Message retention period:** 14 ngày (1209600 giây)
- Giữ nguyên các cài đặt còn lại

Nhấn **Create queue** và sao chép **ARN** của DLQ.

> `[SCREENSHOT — Trang tạo SQS queue cho DLQ]`

**Queue 2 — Main inference queue:**
- **Type:** Standard
- **Name:** `kts-smart-agri-inference-prod`
- **Visibility timeout:** 120 giây
- **Message retention period:** 1 ngày (86400 giây)
- **Dead-letter queue:** Bật → chọn `kts-smart-agri-inference-dlq-prod` → Maximum receives: **3**

Nhấn **Create queue**.

> `[SCREENSHOT — SQS console hiển thị cả hai queue đã tạo]`

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

> `[SCREENSHOT — Editor access policy của SQS với policy cho phép S3 gửi message]`

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

> `[SCREENSHOT — Editor cấu hình CORS của S3 với rule PUT/HEAD]`

Tiếp theo vào tab **Properties** → **Event notifications** → **Create event notification**:
- **Event name:** `SendToSQS`
- **Event types:** All object create events
- **Prefix filter:** `uploads/`
- **Destination:** SQS queue → chọn `kts-smart-agri-inference-prod`

> `[SCREENSHOT — Cấu hình event notification S3 trỏ đến SQS queue]`

**Bucket 2 — Archive bucket:**
- **Bucket name:** `kts-smart-agri-archive-<ACCOUNT_ID>-prod`
- **Region:** ap-southeast-1
- **Block all public access:** Bật tất cả bốn tùy chọn ✅

Sau khi tạo xong, vào tab **Management** → **Lifecycle rules** → **Create lifecycle rule**:
- **Rule name:** `ExpireProcessedImages`
- **Scope:** Apply to all objects
- **Lifecycle rule actions:** Expire current versions of objects
- **Days after object creation:** **90**

> `[SCREENSHOT — Lifecycle rule S3 đặt xóa object sau 90 ngày]`

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

> `[SCREENSHOT — Editor bucket policy S3 cho CloudTrail bucket]`

---

## Bước 5 — Tạo DynamoDB Table

Điều hướng đến **DynamoDB** → **Tables** → **Create table**.

- **Table name:** `kts-smart-agri-diagnosis-prod`
- **Partition key:** `userId` (String)
- **Sort key:** `imageId` (String)
- **Table settings:** Customize → chế độ **On-demand**

> `[SCREENSHOT — Trang tạo DynamoDB table với partition key userId và sort key imageId]`

Sau khi table được tạo, vào tab **Indexes** → **Create index** (Global Secondary Index):

- **Partition key:** `userId` (String)
- **Sort key:** `createdAt` (String)
- **Index name:** `userId-createdAt-index`
- **Attribute projections:** All

> `[SCREENSHOT — Form tạo DynamoDB GSI với key userId và createdAt]`

---

## Bước 6 — Tạo ECR Repository

Điều hướng đến **ECR** (Elastic Container Registry) → **Repositories** → **Create repository**.

- **Visibility:** Private
- **Repository name:** `kts-smart-agri-ai-inference-prod`
- **Image scan settings:** Bật **Scan on push**

Nhấn **Create repository** và sao chép **Repository URI** — cần dùng khi push Docker image.

> `[SCREENSHOT — ECR repository đã tạo hiển thị URI dạng <account>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod]`

---

## Bước 7 — Build và Push Docker Image lên ECR

Thực hiện trên máy tính cục bộ.

**Bước 7.1 — Sao chép file model**

```bash
copy ai-service\best_resnet_model.pth cloud-backend\functions\ai-inference\
copy ai-service\best_lenet_model.pth  cloud-backend\functions\ai-inference\
```

**Bước 7.2 — Xác thực Docker với ECR**

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com
```

**Bước 7.3 — Build Docker image**

```bash
cd cloud-backend/functions/ai-inference
docker build -t kts-smart-agri-ai-inference-prod .
```

> `[SCREENSHOT — Terminal hiển thị tiến trình Docker build với dòng "Successfully built" ở cuối]`

**Bước 7.4 — Tag và push lên ECR**

```bash
docker tag kts-smart-agri-ai-inference-prod:latest <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
docker push <ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/kts-smart-agri-ai-inference-prod:latest
```

> `[SCREENSHOT — ECR console hiển thị image đã push với tag "latest" và dung lượng]`

---

## Bước 8 — Tạo IAM Roles cho Lambda

Điều hướng đến **IAM** → **Roles** → **Create role**. Chọn **AWS service** → **Lambda** làm use case.

Tạo một role cho mỗi Lambda function theo danh sách dưới đây.

**Role 1: `kts-agri-presign-url-role-prod`**
Đính kèm managed policy `AWSLambdaBasicExecutionRole`, sau đó thêm inline policy:
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
Đính kèm `AWSLambdaBasicExecutionRole` và `AWSLambdaSQSQueueExecutionRole`, sau đó thêm inline policy:
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
Đính kèm `AWSLambdaBasicExecutionRole`, sau đó thêm inline policy:
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
Đính kèm `AWSLambdaBasicExecutionRole`, sau đó thêm inline policy cho phép `s3:*` trên images bucket và `dynamodb:*` trên diagnosis table.

> `[SCREENSHOT — IAM Roles console hiển thị cả bốn role kts-agri đã tạo]`

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

> `[SCREENSHOT — Lambda function presign-url với code đã upload và environment variables đã điền]`

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

> `[SCREENSHOT — Lambda ai-inference hiển thị container image URI, 1536 MB memory, timeout 120s và SQS trigger]`

**Function 3 — get-result:**
Tương tự Function 1, tên `kts-smart-agri-get-result-prod`, role `kts-agri-get-result-role-prod`, upload thư mục `functions/get-result/`.
Environment variable: `DYNAMODB_TABLE_NAME` = `kts-smart-agri-diagnosis-prod`

**Function 4 — data-maintenance:**
Tên `kts-smart-agri-data-maintenance-prod`, role `kts-agri-data-maintenance-role-prod`, upload thư mục `functions/data-maintenance/`.
- Timeout: **300 giây**
- Environment variables: `IMAGES_BUCKET_NAME`, `DYNAMODB_TABLE_NAME`, `RETENTION_DAYS=30`
- Thêm **Trigger** → **EventBridge (CloudWatch Events)** → Create new rule → Schedule expression: `cron(0 0 ? * SUN *)`

> `[SCREENSHOT — Lambda console hiển thị danh sách bốn function kts-smart-agri]`

---

## Bước 10 — Tạo API Gateway

Điều hướng đến **API Gateway** → **Create API** → **REST API** → **Build**.

- **API name:** `kts-smart-agri-api-prod`
- **API endpoint type:** Regional

> `[SCREENSHOT — Trang tạo API Gateway với REST API và Regional endpoint đã chọn]`

**Bước 10.1 — Tạo Cognito Authorizer**

Vào **Authorizers** → **Create authorizer**:
- **Name:** `CognitoAuthorizer`
- **Type:** Cognito
- **Cognito user pool:** chọn `kts-smart-agri-user-pool-prod`
- **Token source:** `Authorization`

> `[SCREENSHOT — API Gateway authorizer đã tạo với Cognito user pool được liên kết]`

**Bước 10.2 — Tạo resources và methods**

Tạo các resource và method sau. Với mỗi method, đặt **Integration type** là **Lambda Function** và bật **Lambda Proxy Integration**.

| Resource | Method | Lambda | Xác thực |
|----------|--------|--------|----------|
| `/images/presign` | POST | `kts-smart-agri-presign-url-prod` | CognitoAuthorizer |
| `/images` | GET | `kts-smart-agri-get-result-prod` | CognitoAuthorizer |
| `/images/{imageId}/result` | GET | `kts-smart-agri-get-result-prod` | CognitoAuthorizer |

Với mỗi resource, tạo thêm method **OPTIONS** với **Mock integration** và thêm các response headers để hỗ trợ CORS:
- `Access-Control-Allow-Origin: '*'`
- `Access-Control-Allow-Headers: 'Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Requested-With'`
- `Access-Control-Allow-Methods: 'GET,POST,PUT,DELETE,OPTIONS'`

> `[SCREENSHOT — Cây resource API Gateway hiển thị /images, /images/presign và /images/{imageId}/result]`

**Bước 10.3 — Deploy API**

Nhấn **Deploy API** → Tạo stage mới tên `prod`. Ghi lại **Invoke URL** — đây là `ApiEndpoint` dùng trong file `.env` của frontend.

> `[SCREENSHOT — API Gateway stage editor hiển thị Invoke URL của stage prod]`

---

## Bước 11 — Tạo SNS Topic và CloudWatch Alarms

Điều hướng đến **SNS** → **Topics** → **Create topic**:
- **Type:** Standard
- **Name:** `kts-smart-agri-alerts-prod`

Sau khi tạo xong, nhấn **Create subscription**:
- **Protocol:** Email
- **Endpoint:** địa chỉ email quản trị viên của bạn

Kiểm tra hộp thư và nhấn link xác nhận trong email từ SNS.

> `[SCREENSHOT — SNS topic với một email subscription hiển thị trạng thái Confirmed]`

Điều hướng đến **CloudWatch** → **Alarms** → **Create alarm**. Tạo ba alarm sau, tất cả đều có **SNS action** trỏ đến `kts-smart-agri-alerts-prod`:

**Alarm 1:** Metric `AWS/Lambda` → `Errors` → Function `kts-smart-agri-ai-inference-prod` → Period 5 phút → Threshold >= 1 → Tên `kts-agri-ai-inference-errors-prod`

**Alarm 2:** Tương tự nhưng cho function `kts-smart-agri-presign-url-prod` → Tên `kts-agri-presign-url-errors-prod`

**Alarm 3:** Metric `AWS/DynamoDB` → `SystemErrors` → Table `kts-smart-agri-diagnosis-prod` → Period 5 phút → Threshold >= 1 → Tên `kts-agri-dynamodb-system-errors-prod`

> `[SCREENSHOT — CloudWatch Alarms console hiển thị ba alarm với trạng thái OK]`

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

> `[SCREENSHOT — Trang tạo CloudTrail trail với S3 images bucket được chọn làm data event source]`

---

## Bước 13 — Tạo CloudFront Distribution

Điều hướng đến **CloudFront** → **Distributions** → **Create distribution**.

**Origin:**
- **Origin domain:** `<API_ID>.execute-api.ap-southeast-1.amazonaws.com`
- **Origin path:** `/prod`
- **Protocol:** HTTPS only
- **Minimum origin SSL protocol:** TLSv1.2

**Default cache behavior:**
- **Viewer protocol policy:** Redirect HTTP to HTTPS
- **Allowed HTTP methods:** GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- **Cache policy:** `CachingDisabled` (dùng AWS managed policy)
- **Origin request policy:** `AllViewerExceptHostHeader`

**WAF:** Chọn Web ACL đã tạo ở Bước 1 (`kts-smart-agri-waf-prod`)

**Price class:** Use all edge locations

**Description:** `KTS Smart Agriculture - prod`

Nhấn **Create distribution** và chờ trạng thái chuyển từ **Deploying** sang **Enabled** (mất 5–10 phút).

> `[SCREENSHOT — CloudFront distribution hiển thị trạng thái Enabled với domain name]`

Sao chép **Distribution domain name** (định dạng: `xxxxxxxxxxxx.cloudfront.net`).

---

## Bước 14 — Cấu hình và Chạy Frontend

Trên máy tính cục bộ, điều hướng đến thư mục `frontend-app/` và tạo file `.env`:

```env
VITE_COGNITO_USER_POOL_ID=<User Pool ID từ Bước 2>
VITE_COGNITO_USER_POOL_CLIENT_ID=<App Client ID từ Bước 2>
VITE_API_BASE_URL=https://<API Gateway Invoke URL từ Bước 10>
```

Cài đặt dependencies và khởi động development server:

```bash
npm install
npm run dev
```

Mở `http://localhost:5173` trên trình duyệt.

> `[SCREENSHOT — Trình duyệt hiển thị trang đăng nhập KTS Smart Agriculture]`

---

## Bước 15 — Kiểm tra End-to-End

**15.1 — Đăng ký:** Nhấn **Create account**, nhập email và mật khẩu đáp ứng yêu cầu (tối thiểu 8 ký tự, có chữ hoa, chữ thường, chữ số). Gửi form.

> `[SCREENSHOT — Form đăng ký đã điền đầy đủ thông tin]`

**15.2 — Xác nhận email:** Kiểm tra hộp thư để lấy mã xác nhận từ Cognito và nhập vào màn hình xác nhận.

> `[SCREENSHOT — Màn hình nhập mã xác nhận email]`

**15.3 — Đăng nhập:** Nhập thông tin đăng nhập trên trang login.

> `[SCREENSHOT — Dashboard sau khi đăng nhập thành công]`

**15.4 — Upload ảnh cây trồng:** Điều hướng đến trang Upload, chọn ảnh JPG/PNG của lá cây và nhấn Upload. Thanh tiến trình hiển thị quá trình upload trực tiếp lên S3.

> `[SCREENSHOT — Trang upload hiển thị thanh tiến trình đạt 100%]`

**15.5 — Xem kết quả chẩn đoán:** Sau khi upload, trang chuyển đến màn hình kết quả. Ảnh được xử lý qua pipeline S3 → SQS → AI Inference Lambda → DynamoDB. Lần đầu khởi động (cold start), việc load mô hình ResNet50 + LeNet mất khoảng 20–30 giây. Nhấn **Refresh** nếu trạng thái hiển thị "Đang xử lý".

> `[SCREENSHOT — Trang kết quả hiển thị tên bệnh và phần trăm độ tin cậy]`

**15.6 — Xác minh trong DynamoDB:** Điều hướng đến **DynamoDB → Tables → kts-smart-agri-diagnosis-prod → Explore items** để xác nhận record có `status: COMPLETED` với tên bệnh và giá trị confidence đã được ghi vào.

> `[SCREENSHOT — DynamoDB item hiển thị userId, imageId, status COMPLETED, tên bệnh và confidence]`
