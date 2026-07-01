---
title : "Chuẩn bị GitHub, IAM User Access Key, ECR và Thông báo"
date : 2026-06-30 
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

## Mục tiêu

Hoàn thành việc cấu hình toàn bộ tài nguyên đầu vào cho chu trình tự động hóa: thiết lập kho lưu trữ mã nguồn, khởi tạo tài khoản đặc quyền IAM User, khởi tạo và trích xuất cặp khóa Access Key, chuẩn bị các kho chứa Docker Image tập trung và thiết lập các biến môi trường bảo mật trên GitHub Actions.

## Các bước thực hiện chi tiết

### 1. Chuẩn bị cấu trúc GitHub Repository
Đảm bảo mã nguồn của bạn đã được tổ chức phân tách rõ ràng trong Repository để GitHub Actions có thể định vị chính xác vị trí build:
- Thư mục ứng dụng **Frontend** (chứa `Dockerfile` phục vụ giao diện).
```dockerfile
  # Stage 1: Build the application
  FROM node:20-alpine AS builder

  WORKDIR /app

  # Copy package files first to cache dependencies
  COPY package*.json ./

  # Install dependencies
  RUN npm install

  # Copy the rest of the project
  COPY . .

  # Build the application
  RUN npm run build

  # Stage 2: Serve with Nginx
  FROM nginx:alpine

  # Copy built files from builder stage
  COPY --from=builder /app/dist /usr/share/nginx/html

  # Copy Nginx configuration
  COPY nginx.conf /etc/nginx/conf.d/default.conf

  # Expose port 80
  EXPOSE 80

  # Start Nginx
  CMD ["nginx", "-g", "daemon off;"]
```

- Thư mục ứng dụng Backend (chứa `Dockerfile` phục vụ ứng dụng Spring Boot).
```dockerfile
# Stage 1: Build the application
# Sử dụng đúng Java 17 như cấu hình pom.xml của bạn
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app

# Copy Maven wrapper and pom.xml first to cache dependencies
COPY mvnw ./
COPY .mvn ./.mvn
COPY pom.xml ./

# Make mvnw executable
RUN chmod +x mvnw

# Download dependencies (this layer will be cached)
RUN ./mvnw dependency:go-offline -B

# Copy the rest of the project
COPY src ./src

# Build the application
RUN ./mvnw clean package -DskipTests

# Stage 2: Run the application
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copy the built jar from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose the default Spring Boot port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 2. Khởi tạo IAM User và Trích xuất Access Key trên AWS
Để GitHub Actions có quyền kết nối và tương tác với các tài nguyên trên AWS thông qua CLI, chúng ta cần tạo một tài khoản programmatic user chuyên biệt:

1. Truy cập **AWS Management Console** -> Tìm kiếm dịch vụ **IAM** -> Tại menu bên trái chọn **Users** -> Chọn **Create user**.
   - **User name**: Điền chính xác `github-actions-deployer`.
   - Nhấn **Next**
![Overview](/images/5-Workshop/Workshop-img/IAM-1.jpg)
2. **Gán chính sách phân quyền (Set permissions)**: 
   - Chọn mục **Attach policies directly**.
   - Tìm và chọn attach policy **AmazonECRFullAccess**.
   - Nhấn **Next** -> Chọn **Create user**.
   ![Overview](/images/5-Workshop/Workshop-img/IAM-2.jpg)
3. **Add thêm permission**: 
   - Chọn IAM User **`github-actions-deployer`** mới tạo.
      ![Overview](/images/5-Workshop/Workshop-img/IAM-3.jpg)
   - Click vào **Add permissions** -> **Create inline policy** -> **JSON**.
   - Dán vào nội dung JSON sau:
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowECRAuth",
			"Effect": "Allow",
			"Action": [
				"ecr:GetAuthorizationToken"
			],
			"Resource": "*"
		},
		{
			"Sid": "AllowECRProjectActions",
			"Effect": "Allow",
			"Action": [
				"ecr:BatchCheckLayerAvailability",
				"ecr:GetDownloadUrlForLayer",
				"ecr:BatchGetImage",
				"ecr:InitiateLayerUpload",
				"ecr:UploadLayerPart",
				"ecr:CompleteLayerUpload",
				"ecr:PutImage"
			],
			"Resource": "arn:aws:ecr:*:307257806722:repository/*"
		},
		{
			"Sid": "AllowECSAdvancedDeployment",
			"Effect": "Allow",
			"Action": [
				"ecs:DescribeTaskDefinition",
				"ecs:RegisterTaskDefinition",
				"ecs:UpdateService",
				"ecs:DescribeServices"
			],
			"Resource": "*"
		},
		{
			"Sid": "AllowPassRole",
			"Effect": "Allow",
			"Action": [
				"iam:PassRole"
			],
			"Resource": [
				"arn:aws:iam::307257806722:role/ecsTaskExecutionRole"
			]
		}
	]
}
```
   - Nhấn **Next** -> Chọn **Create**.
   ![Overview](/images/5-Workshop/Workshop-img/IAM-4.jpg)
4. **Khởi tạo cặp khóa credentials**:
   - Nhấp chọn vào User **`github-actions-deployer`** vừa được tạo -> Chuyển sang tab **Security credentials**.
   - Cuộn xuống mục **Access keys** -> Nhấn nút **Create access key**.
   - Tại trang chọn mục đích sử dụng, tích chọn **Command Line Interface (CLI)** -> Nhấn **Next**.
   - **Description**: Điền `GitHub Actions Key`.
   - Nhấn **Create access key**.
   - **QUAN TRỌNG**: Hệ thống sẽ hiển thị một lần duy nhất cặp khóa bao gồm **Access Key ID** và **Secret Access Key**. Hãy nhấn nút **Download .csv file** để lưu trữ lại cẩn thận, tuyệt đối không làm lộ cặp khóa này.
![Overview](/images/5-Workshop/Workshop-img/IAM-5.jpg)

### 3. Khởi tạo các kho chứa Amazon ECR Repositories
1. Trên thanh tìm kiếm AWS, nhập **Elastic Container Registry** hoặc **ECR** -> Chọn **Repositories** -> Chọn **Create repository**.
2. Tiến hành khởi tạo lần lượt 2 kho chứa với cấu hình bảo mật:
   - Kho chứa thứ nhất mang tên: `globalmart-frontend`.
   - Kho chứa thứ hai mang tên: `globalmart-backend`.
   ![Overview](/images/5-Workshop/Workshop-img/ECR-1.jpg)
3. **Mục nâng cao bắt buộc**: Tại phần **Image scan settings**, hãy gạt bật tính năng **Scan on push** sang trạng thái màu xanh (`Enabled`) để kích hoạt tính năng tự động quét mã độc và lỗ hổng bảo mật ngay khi GitHub đẩy Image lên.
![Overview](/images/5-Workshop/Workshop-img/ECR-2.jpg)


### 4. Cấu hình Secrets và Variables bảo mật trên GitHub
1. Truy cập vào Repository của bạn trên **GitHub** -> Chọn tab **Settings** -> Menu trái chọn **Secrets and variables** -> Chọn **Actions**.
2. Tại tab **Secrets** -> Chọn **New repository secret** để cấu hình các thông tin bảo mật:
   - `AWS_ACCESS_KEY_ID`: Dán chuỗi mã Access Key ID lấy từ file CSV ở Bước 2.
   - `AWS_SECRET_ACCESS_KEY`: Dán chuỗi mã mã hóa Secret Access Key lấy từ file CSV ở Bước 2.
   - `MAIL_PASSWORD`: Điền mã **App Password** gồm 16 ký tự được cấp từ tài khoản Google.
   - `AWS_REGION`: Điền `ap-southeast-1`.
![Overview](/images/5-Workshop/Workshop-img/ECR-3.jpg)


## Kết quả mong đợi

Sau khi hoàn thành bài thực hành này, bạn phải đạt được các kết quả sau:
- Tài khoản IAM User `github-actions-deployer` hoạt động ổn định và giữ file CSV chứa credential an toàn.
- Hai kho ECR (`globalmart-frontend` và `globalmart-backend`) sẵn sàng nhận lệnh push.
- Toàn bộ Access Keys, Secrets và tài khoản Mail đã được mã hóa an toàn trên hệ thống GitHub.