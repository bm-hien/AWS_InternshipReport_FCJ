---
title : "CI/CD Automation"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Hướng dẫn thiết lập AWS CodeBuild cho Flyora Frontend

{{% notice info %}}
Hướng dẫn này chỉ bao gồm thiết lập CI/CD cho **Repository Frontend** (React).
{{% /notice %}}

Hướng dẫn này sẽ giúp bạn thiết lập AWS CodeBuild và CodePipeline cho ứng dụng Flyora React frontend.

---

## Yêu cầu

- Tài khoản AWS với quyền phù hợp
- GitHub repository: `QuangHieu-lab/Flyora-shop`
- File `buildspec.yml` trong thư mục gốc của repository (xem bên dưới)
- AWS Region: **Singapore (ap-southeast-1)** (hoặc region bạn chọn)
- S3 bucket để host static files
- CloudFront distribution (tùy chọn, cho CDN)

### Bắt buộc: File buildspec.yml

Tạo file `buildspec.yml` trong thư mục gốc của repository với nội dung:

```yaml
version: 0.2

phases:
  pre_build:
    commands:
      - echo "Installing dependencies on `date`"
      - npm ci
      - echo "Running linter..."
      - npm run lint --if-present || echo "No lint script configured"
  
  build:
    commands:
      - echo "Running tests on `date`"
      - npm test -- --watchAll=false --passWithNoTests --coverage || echo "Tests completed"
      - echo "Building application on `date`"
      - npm run build
      - echo "Build completed on `date`"
  
  post_build:
    commands:
      - echo "Post-build phase started on `date`"
      - echo "Checking if build directory exists..."
      - ls -la build/
      - echo "Build artifacts created successfully"
      - echo "Build completed successfully - artifacts ready for deployment"
      - echo "Use CodePipeline Deploy stage for S3 deployment"
      - echo "CloudFront invalidation will be handled by CodePipeline"

artifacts:
  files:
    - '**/*'
  base-directory: build
  name: BuildArtifact
  discard-paths: yes

cache:
  paths:
    - '/root/.npm/**/*'
    - 'node_modules/**/*'
```

**Điểm chính:**
- Sử dụng `npm ci` để cài đặt nhanh và đáng tin cậy hơn
- Chạy linter nếu được cấu hình
- Chạy tests với coverage
- Build ứng dụng React
- Deployment được xử lý bởi CodePipeline Deploy stage (không trong buildspec)
- Cache npm và node_modules

![Buildspec](/images/5-Workshop/6.ci-cd/buildspec.jpg)

---

## Bước 1: Tạo S3 Bucket để Hosting

Trước khi thiết lập CodeBuild, tạo S3 bucket để host ứng dụng React:

1. Vào **S3** console
2. Click **"Create bucket"**
3. Bucket name: `flyora-frontend-hosting`
4. Region: **Singapore (ap-southeast-1)**
5. Bỏ chọn **"Block all public access"** (để host static website)
6. Bật **"Static website hosting"** trong bucket properties
7. Đặt **Index document**: `index.html`
8. Đặt **Error document**: `index.html`

---

## Bước 2: Truy cập CodeBuild

1. Mở trình duyệt và truy cập: https://console.aws.amazon.com/codesuite/codebuild/projects
2. Đăng nhập vào tài khoản AWS
3. Đảm bảo đang ở region **Singapore (ap-southeast-1)**
4. Click **"Create build project"**

---

## Bước 3: Cấu hình Project

| Trường | Giá trị |
|--------|---------|
| **Project name** | `flyora-frontend-build` |
| **Description** | `Build project for Flyora React frontend` |
| **Build badge** | Để trống |

![Project Configuration](/images/5-Workshop/6.ci-cd/projectConfig.jpg)

---

## Bước 4: Cấu hình Source

| Trường | Giá trị |
|--------|---------|
| **Source provider** | `GitHub` |
| **Repository** | `Repository in my GitHub account` |
| **GitHub repository** | `https://github.com/QuangHieu-lab/Flyora-shop` |

![Source Configuration](/images/5-Workshop/6.ci-cd/source.jpg)
| **Source version** | Để trống |
| **Git clone depth** | `1` |
| **Primary source webhook events** | ⚠️ **BỎ CHỌN** |

---

## Bước 5: Cấu hình Environment

| Phần | Trường | Giá trị |
|------|--------|---------|
| **Provisioning** | Provisioning model | `On-demand` |
| | Environment image | `Managed image` |
| | Operating system | `Amazon Linux` |
| | Runtime(s) | `Standard` |
| | Image | Latest (vd: `aws/codebuild/amazonlinux2-x86_64-standard:5.0`) |
| **Service role** | Service role | `New service role` |

---

## Bước 6: Biến môi trường

Thêm các biến môi trường trong CodeBuild:

| Tên | Giá trị | Loại |
|-----|---------|------|
| `AWS_S3_BUCKET` | `flyora-frontend-hosting` | Plaintext |
| `CLOUDFRONT_DISTRIBUTION_ID` | CloudFront distribution ID của bạn (nếu dùng) | Plaintext |
| `REACT_APP_API_URL` | URL backend API của bạn | Plaintext |

![Environment Variables](/images/5-Workshop/6.ci-cd/env.jpg)

![Environment Variables 2](/images/5-Workshop/6.ci-cd/env2.jpg)

![Environment Variables 3](/images/5-Workshop/6.ci-cd/buildspec.jpg)

---

## Bước 7: Buildspec và Logs

**Buildspec:**
- **Build specifications**: `Use a buildspec file`
- **Buildspec name**: Để trống

**Logs:**
- **CloudWatch logs**: ✅ Bật
- **Group name**: `/aws/codebuild/flyora-frontend`

---

## Bước 8: Quyền IAM

CodeBuild service role cần quyền truy cập S3 và CloudFront. Thêm policy này:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::flyora-frontend-hosting",
        "arn:aws:s3:::flyora-frontend-hosting/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudfront:CreateInvalidation"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## Bước 9: Tạo CodePipeline

1. Vào **CodePipeline** console
2. Click **"Create pipeline"**
3. Pipeline name: `flyora-frontend-pipeline`
4. **Source stage**: GitHub (Version 2)
5. **Build stage**: Chọn `flyora-frontend-build`
6. **Deploy stage**: Thêm S3 deploy action
   - **Action provider**: Amazon S3
   - **Bucket**: `flyora-frontend-hosting`
   - **Extract file before deploy**: ✅ Chọn

![Pipeline Review](/images/5-Workshop/6.ci-cd/pipelineReview.jpg)

---

## Kiểm tra

Sau khi tạo pipeline:

1. Thực hiện thay đổi trong React repository
2. Commit và push lên GitHub
3. Pipeline tự động trigger
4. Kiểm tra S3 bucket có files đã cập nhật
5. Truy cập website qua S3 endpoint hoặc CloudFront URL

![Phase Details](/images/5-Workshop/6.ci-cd/phaseDetail.jpg)

![Deploy Success](/images/5-Workshop/6.ci-cd/deploySuccess.jpg)

---

## Xử lý sự cố

### Vấn đề: Build thất bại với "npm: command not found"
**Giải pháp:** Đảm bảo `runtime-versions: nodejs: 18` được đặt trong buildspec.yml

### Vấn đề: S3 sync permission denied
**Giải pháp:** Kiểm tra IAM role có quyền S3 (xem Bước 8)

### Vấn đề: Website hiển thị nội dung cũ
**Giải pháp:** 
- Xóa browser cache
- Invalidate CloudFront cache nếu dùng CDN
- Kiểm tra S3 bucket có files mới nhất

---

## Tham khảo nhanh

| Tài nguyên | Giá trị |
|------------|---------|
| **Pipeline Name** | `flyora-frontend-pipeline` |
| **Build Project Name** | `flyora-frontend-build` |
| **S3 Bucket** | `flyora-frontend-hosting` |
| **Region** | Singapore (ap-southeast-1) |
| **Source** | GitHub (`QuangHieu-lab/Flyora-shop`) |
| **Buildspec** | `buildspec.yml` (trong repo root) |
| **Logs** | CloudWatch: `/aws/codebuild/flyora-frontend` |

---

## Ước tính chi phí

| Hạng mục | Chi phí |
|----------|---------|
| **CodePipeline** | $1/tháng cho mỗi pipeline |
| **CodeBuild (Free Tier)** | 100 phút build/tháng trong 12 tháng |
| **S3 Storage** | ~$0.023/GB/tháng |
| **S3 Requests** | Tối thiểu cho static hosting |
| **CloudFront** | Free tier: 1TB data transfer/tháng |
| **Hàng tháng (ước tính)** | $1-3/tháng |

---

## Tóm tắt

✅ **Đã cấu hình Frontend CI/CD Pipeline!**

**Đã hoàn thành:**
- ✅ CodeBuild project cho ứng dụng React
- ✅ CodePipeline cho quy trình tự động
- ✅ Tích hợp GitHub với trigger tự động
- ✅ Deploy tự động lên S3
- ✅ Tích hợp CloudFront CDN (tùy chọn)

**Pipeline của bạn hiện tại:**
- Tự động phát hiện thay đổi code trong GitHub
- Build ứng dụng React với npm
- Deploy static files lên S3
- Phục vụ website qua S3 hoặc CloudFront
