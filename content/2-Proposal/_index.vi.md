---
title: "Bản đề xuất"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hạ tầng Điện toán Đám mây GlobalMart
## Giải pháp Thiết lập Hạ tầng DevOps/Platform Engineering Tiêu chuẩn: Tự động hóa CI/CD qua GitHub Actions, Mạng Cô lập Đa phân vùng và Giám sát Đa tầng CloudWatch

### 1. Tóm tắt Dự án 
GlobalMart là dự án tập trung hoàn toàn vào việc nghiên cứu, thiết kế và triển khai hệ thống hạ tầng đám mây tiêu chuẩn hiện đại trên nền tảng AWS. Dự án tập trung tối ưu hóa chu trình vận hành phần mềm (DevOps) bằng cách loại bỏ các dịch vụ quản lý pipeline cồng kềnh truyền thống trên AWS, thay thế bằng **GitHub Actions** kết hợp xác thực bảo mật **IAM OIDC Role**. Hệ thống áp dụng mô hình phân tách mạng nghiêm ngặt (VPC Public/Private Subnets), định tuyến API tập trung qua **Amazon API Gateway / VPC Link**, cấu hình lưu vết nhật ký hệ thống nâng cao (Container Logs) và thiết lập kịch bản sao lưu an toàn tự động về S3.

### 2. Phát biểu Bài toán
### Vấn đề là gì?
Trong các dự án phần mềm thực tế, việc thiết lập hạ tầng mạng lỏng lẻo, phơi bày database ra internet hoặc deploy code thủ công bằng tay luôn là nguyên nhân hàng đầu dẫn đến rủi ro rò rỉ dữ liệu và gián đoạn hệ thống. Việc lưu trữ Access Key/Secret Key của AWS trực tiếp trên các nền tảng CI/CD bên ngoài tiềm ẩn nguy cơ lộ lọt tài khoản đặc quyền. Thêm vào đó, việc thiếu hụt một hệ thống giám sát tập trung làm tê liệt khả năng ứng phó sự cố của đội ngũ kỹ sư khi container ứng dụng gặp lỗi ngầm hoặc bị crash đột ngột.

### Giải pháp
Dự án GlobalMart thiết lập một hạ tầng mạng VPC chuẩn hóa gồm Public Subnet A (chứa Internet ALB, NAT Gateway) và hệ thống Private Subnet để cô lập hoàn toàn môi trường chạy ứng dụng (ECS Fargate) và lưu trữ dữ liệu (RDS). Chuỗi tích hợp và triển khai liên tục (CI/CD) được xây dựng tự động hoàn toàn qua GitHub Actions, ứng dụng cơ chế **IAM OIDC Identity Provider** để cấp quyền bảo mật không cần dùng Key cứng. Toàn bộ traffic đi vào hệ thống được kiểm soát tập trung qua **API Gateway**, đi qua mạng nội bộ nhờ **VPC Link / ALB Internal**. Hệ thống được giám sát đa lớp bằng CloudWatch Dashboard tập trung kết hợp bộ cảnh báo tự động thông qua Amazon SNS gửi thẳng về Email/SMS của quản trị viên.

### Lợi ích và Hiệu quả Đầu tư
Dự án mang lại giá trị thực tế lớn thông qua việc tinh gọn bộ máy CI/CD giúp cắt giảm chi phí vận hành (không phải trả tiền cho CodeBuild/CodePipeline của AWS). Việc áp dụng cơ chế Scan on push tại ECR giúp kiểm soát mã độc container sớm. Hệ thống đạt trạng thái tự động hóa hoàn chỉnh: Kỹ sư chỉ cần thực hiện `git push` lên GitHub, hệ thống sẽ tự động build image, cập nhật Task Definition Revision mới và kích hoạt **ECS Rolling Update** cập nhật website mà không gây gián đoạn dịch vụ (Zero-downtime).

### 3. Kiến trúc Giải pháp 
Kiến trúc dự án được thiết lập đồng bộ trên nền tảng AWS Fargate (Serverless Container) và mô hình mạng phân lớp bảo mật qua ma trận Security Groups. Chi tiết sơ đồ hạ tầng được mô tả dưới đây:

![Kiến trúc Triển khai GlobalMart](/images/2-Proposal/globalmart.png)

### Các Dịch vụ AWS Được Sử Dụng
- **AWS VPC & IAM OIDC**: Thiết lập mạng lõi gồm Public Subnet (định tuyến Internet) và các Private Subnet cô lập, kết hợp cấu hình Identity Provider cho phép GitHub Actions thiết lập liên kết tin cậy bảo mật.
- **GitHub Actions (Build & Update Service)**: Đóng vai trò là công cụ CI/CD cốt lõi, thay thế hoàn toàn cho CodePipeline, CodeBuild và CodeDeploy của AWS để tự động hóa toàn bộ chu trình.
- **Amazon ECR & S3 Artifact Bucket**: Kho lưu trữ Docker Image tích hợp quét bảo mật (Scan on push) và lưu trữ trạng thái build hệ thống.
- **Amazon ECS Cluster & AWS Fargate**: Điều phối container không máy chủ (Serverless) quản lý độc lập hai dịch vụ `globalmart-frontend-task` và `globalmart-backend-task`.
- **AWS Application Load Balancer (ALB)**: Hệ thống 2 bộ ALB độc lập (ALB-Internet-Facing tiếp nhận traffic public vào Frontend; ALB Internal tiếp nhận traffic nội bộ điều hướng vào Backend).
- **Amazon API Gateway & VPC Link**: Cổng kiểm soát API tập trung, kết hợp VPC Link để chuyển tiếp traffic an toàn từ môi trường Public vào Load Balancer nội bộ nằm trong vùng Private.
- **Amazon RDS Single AZ**: Hệ thống cơ sở dữ liệu MySQL/PostgreSQL được đặt ẩn hoàn toàn trong vùng Private Subnet B, chặn tuyệt đối mọi hành vi truy cập trực tiếp từ bên ngoài Internet.
- **Amazon CloudWatch & Amazon SNS**: Bộ đôi thu thập tập trung logs/metrics đa lớp (ECS, ALB, RDS) và phân phối cảnh báo tự động qua Email/SMS khi hệ thống quá tải.

### Thiết kế Thành phần 
- **Hạ tầng Mạng & Bảo mật Phân lớp**: Hệ thống bao gồm 3 Security Groups riêng biệt để cô lập dòng dữ liệu. Ứng dụng Frontend và Backend được xếp vào phân vùng mạng Private Subnet A nhằm che giấu IP gốc, Database nằm riêng biệt tại Private Subnet B.
- **Chuỗi Tự động hóa CI/CD qua GitHub Actions**: Cấu hình tệp workflow `.yml` xử lý tuần tự: Checkout code -> Giả lập AWS Credentials qua OIDC Role -> Build và tag Docker Image bằng Git Commit SHA -> Push lên ECR -> Tự động sinh file JSON Task Definition mới -> Kích hoạt lệnh `update-service` trên ECS.
- **Kết nối & Vận hành Dữ liệu**: Người dùng gọi API qua API Gateway -> Đi qua VPC Link -> Đến ALB Internal -> Đến ECS Backend -> Truy vấn trực tiếp vào Database RDS Single AZ thông qua kết nối mạng nội bộ bảo mật.
- **Giám sát & Nhật ký Tập trung**: CloudWatch Logs thu thập toàn bộ dữ liệu nhật ký dòng lệnh (Console logs) từ các container Frontend/Backend, vết định tuyến tại ALB và trạng thái Performance của RDS để hiển thị trực quan lên CloudWatch Dashboard tập trung.

### 4. Triển khai Kỹ thuật
**Các Giai đoạn Triển khai**
Tiến trình thực hiện dự án được lên kế hoạch chi tiết theo thời gian 1 tháng, tập trung hoàn toàn vào các bước cấu hình, tích hợp hạ tầng hệ thống:
- **Tuần 1 - Cốt lõi Mạng & Thiết lập IAM OIDC Role**: Khởi tạo mạng lõi VPC gồm hệ thống Public Subnet (chứa ALB Internet-Facing, NAT Gateway) và các Private Subnet cô lập. Cấu hình Identity Provider (IdP) trên IAM gắn với OIDC Provider của GitHub, tạo IAM Role cho phép tài khoản GitHub Actions có quyền tương tác với ECR và ECS mà không cần Access Key cứng.
- **Tuần 2 - Đóng gói & Xây dựng Workflow GitHub Actions**: Khởi tạo các kho lưu trữ ECR Frontend và Backend có bật tính năng scan on push. Viết tệp workflow cấu hình `.github/workflows/deploy.yml` trên GitHub để tự động hóa luồng chạy: sử dụng lệnh docker build, tag image theo mã định danh `${{ github.sha }}` và thực hiện đẩy lên Amazon ECR.
- **Tuần 3 - Khởi tạo Runtime Container, API Gateway & Định tuyến**: Khởi tạo cụm ECS Cluster Fargate cùng hệ thống Task Definitions chi tiết. Thiết lập ALB Internet-Facing (trỏ vào Target Group Frontend) và ALB Internal (trỏ vào Target Group Backend). Cấu hình API Gateway kết hợp VPC Link kết nối trực tiếp với ALB Internal để mở đường dẫn API bảo mật cho ứng dụng.
- **Tuần 4 - Tầng Dữ liệu, Hệ thống Giám sát & Báo động SNS**: Khởi tạo cấu hình RDS Single AZ nằm trong Private Subnet B, truyền biến môi trường kết nối an toàn từ Backend. Cấu hình hệ thống lưu vết nhật ký CloudWatch Logs, Dashboard tập trung hiển thị trực quan các chỉ số CPU/Memory Utilization của Container và hiệu năng RDS. Thiết lập bộ 6 CloudWatch Alarms cảnh báo tự động thông qua Amazon SNS gửi mail về điện thoại khi có sự cố quá tải hệ thống.

**Yêu cầu Kỹ thuật**
- **Kỹ năng Tự động hóa với GitHub Actions**: Hiểu rõ cách viết workflow YAML, quản lý GitHub Secrets bảo mật, khai báo biến môi trường và sử dụng AWS CLI kết hợp Python để xào nấu Task Definition (`register-task-definition`).
- **Tư duy Mạng và Định tuyến Nâng cao**: Hiểu rõ cơ chế định tuyến qua API Gateway, VPC Link, cách phân quyền Security Group chéo (cho phép ALB truy cập Container, cho phép Backend kết nối vào đúng Port của Database RDS).

### 5. Lộ trình & Cột mốc 
**Tiến độ Dự án**
Lộ trình thực hiện dự án được chia nhỏ và bám sát theo tiến độ triển khai hạ tầng thực tế trong vòng 1 tháng:
- Cột mốc 1 (Cuối Tuần 1): Hoàn thành thiết lập toàn bộ lõi mạng bảo mật VPC, cấu hình thông suốt liên kết bảo mật IAM OIDC Role giữa GitHub và AWS.
- Cột mốc 2 (Cuối Tuần 2): Hoàn thành cấu hình workflow GitHub Actions, thực hiện đóng gói và push thành công các phiên bản Docker Image Frontend/Backend lên Amazon ECR.
- Cột mốc 3 (Cuối Tuần 3): Triển khai thành công cụm ECS Fargate, cấu hình xong hệ thống API Gateway, VPC Link và các bộ cân bằng tải ALB đa tầng, đảm bảo định tuyến web thông suốt.
- Cột mốc 4 (Cuối Tuần 4): Đưa tầng dữ liệu RDS Single AZ vào hoạt động, kích hoạt hệ thống giám sát đa lớp CloudWatch Dashboard và bộ cảnh báo tự động qua SNS. Thực hiện thành công kịch bản kiểm tra kích hoạt báo động gửi mail thực tế và nghiệm thu dự án.

### 6. Ước tính Ngân sách (Budget Estimation)
### Chi phí hạ tầng
- Dịch vụ AWS:
    - Amazon ECS Fargate (Frontend): $4.25/tháng (1 task, 0.25 vCPU, 512 MB, chạy 24/7).
    - Amazon ECS Fargate (Backend): $10.36/tháng (1 task, 0.5 vCPU, 1 GB, chạy 24/7).
    - Public IPv4 (ALB + NAT): $7.30/tháng (2 địa chỉ IP công khai × $0.005/giờ).
    - NAT Gateway: $43.37/tháng (730 giờ, ~5 GB dữ liệu xử lý).
    - ALB Internet-facing: $24.24/tháng (730 giờ, ~1 LCU/giờ).
    - ALB Internal: $19.40/tháng (730 giờ, LCU tối thiểu).
    - Amazon API Gateway (HTTP API): $1.00/tháng (~1 triệu requests/tháng).
    - Data Transfer Out: $0.45/tháng (~5 GB truyền ra Internet).
    - Amazon RDS Single AZ (db.t3.micro): $17.42/tháng (Chạy 24/7 môi trường Dev/Test).
    - RDS Storage: $2.76/tháng (20 GB gp2).
    - Amazon ECR (Frontend + Backend): $0.58/tháng (4 GB lưu trữ, ~100 lần pull).
    - Backup Bucket (S3): $0.58/tháng (20 GB lưu trữ bản chụp snapshot/export).
    - CloudWatch Logs: $3.95/tháng (5 GB ingestion, logs container + ALB).
    - CloudWatch Metrics & Alarms: $4.95/tháng (15 metrics, 6 alarms).
    - CloudWatch Dashboard: $3.00/tháng (1 dashboard, 5 widgets chính).
    - Amazon SNS: $0.00/tháng (dưới 1.000 email thông báo, nằm trong AWS Free Tier).
    - GitHub Actions Runner: $0.00/tháng (Sử dụng gói 2.000 phút miễn phí/tháng của GitHub cho repository private).

Tổng cộng: **$187.01 / tháng** 

### 7. Đánh giá Rủi ro
#### Ma trận Rủi ro
- Lỗi vỡ ký tự hoặc sai cấu trúc JSON khi tự động tạo Task Definition qua CLI: Tác động trung bình, xác suất xảy ra trung bình.
- Lỗi lộ lọt dữ liệu do mở nhầm cổng Database ra Internet: Tác động cao, xác suất xảy ra thấp.
- Lỗi nghẽn hoặc treo luồng deploy do Target Group Health Check cấu hình sai mã trạng thái (Success codes): Tác động trung bình, xác suất xảy ra trung bình.

#### Chiến lược Giảm thiểu
- Lỗi tạo Task Definition: Được xử lý triệt để nhờ việc xuất chuỗi cấu hình JSON từ script Python ra file tạm `*.json` trước khi nạp vào lệnh `--cli-input-json` của AWS CLI trong workflow.
- Bảo mật Database: RDS được đặt hoàn toàn trong Private Subnet B không có internet gateway, đồng thời Security Group của RDS chỉ mở cổng duy nhất cho phép IP nội bộ đi từ Security Group của ECS Backend Task.
- Lỗi nghẽn Health Check: Cấu hình nới lỏng mã thành công thành `200-499` trong Target Group (`tg-backend` và `tg-frontend`) để chấp nhận mọi phản hồi khởi động ban đầu từ Spring Boot/Node.js.

#### Kế hoạch Dự phòng 
- Kích hoạt lịch chụp RDS Snapshot thủ công trước mỗi đợt push code lớn, đảm bảo khả năng quay lui dữ liệu (Rollback) về trạng thái ổn định gần nhất trong vòng dưới 5 phút nếu code mới làm lỗi cấu trúc DB.
- Thiết lập kịch bản xuất bản sao lưu tự động (Backup Export) sang hệ thống S3 Backup Bucket độc lập có bật tính năng lưu trữ vòng đời để tối ưu chi phí lưu trữ lâu dài.

### 8. Kết quả Mong đợi
#### Cải tiến Kỹ thuật:
Xây dựng thành công một hạ tầng DevOps tự động hóa, tinh gọn và đạt tiêu chuẩn bảo mật hiện đại: làm chủ luồng xác thực IAM OIDC Role không dùng Key, kiểm soát traffic an toàn qua API Gateway/ALB, và thiết lập thành công hệ thống giám sát CloudWatch Dashboard kết hợp cảnh báo chủ động qua Email khi xảy ra sự cố quá tải container.
#### Giá trị Lâu dài
Mang lại một tài liệu mẫu kiến trúc (Blueprint) chuẩn chỉnh về giải pháp xây dựng chu trình CI/CD tích hợp trực tiếp giữa GitHub Actions và AWS ECS Fargate; tạo tiền đề hạ tầng vững chắc, dễ bảo trì và tối ưu chi phí để doanh nghiệp sẵn sàng triển khai các ứng dụng Microservices full-stack sau này.