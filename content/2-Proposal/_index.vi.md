---
title: "Bản đề xuất"
date: 2026-07-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Hạ tầng Điện toán Đám mây GlobalMart
## Giải pháp Thiết lập Hạ tầng DevOps/Platform Engineering Tiêu chuẩn Production: Tự động hóa CI/CD, Giám sát Đa tầng và Đảm bảo Tính sẵn sàng cao (High Availability)

### 1. Tóm tắt Dự án 
GlobalMart là dự án tập trung hoàn toàn vào việc nghiên cứu, thiết kế và triển khai hệ thống hạ tầng đám mây đám mây tiêu chuẩn Production trên nền tảng AWS. Mục tiêu cốt lõi của dự án không nằm ở việc phát triển logic mã nguồn ứng dụng (code phần mềm), mà tập trung vào việc tự động hóa chu trình vận hành phần mềm (DevOps). Dự án áp dụng các kỹ thuật thiết lập hạ tầng Multi-AZ, xây dựng pipeline CI/CD tự động 3 giai đoạn bảo mật, quản lý kết nối dữ liệu thông minh qua Proxy, cấu hình lưu vết nhật ký hệ thống nâng cao (VPC Flow Logs, Container Logs) và giả lập các kịch bản diễn tập khôi phục thảm họa trong thời gian dưới 60 giây.

### 2. Phát biểu Bài toán
### Vấn đề là gì?
Trong các dự án phần mềm thực tế, việc thiếu một hạ tầng mạng chuẩn hóa, phân lớp bảo mật lỏng lẻo hoặc phân bổ tài nguyên trên một vùng sẵn sàng đơn lẻ (Single-AZ) luôn là nguyên nhân hàng đầu dẫn đến rủi ro sập hệ thống. Quy trình build và deploy container thủ công bằng tay gây tốn thời gian, dễ rò rỉ mã nguồn và không thể kiểm soát lỗ hổng bảo mật. Thêm vào đó, việc thiếu hụt một hệ thống giám sát tập trung thu thập cả log mạng (VPC Flow Logs) lẫn log ứng dụng làm tê liệt khả năng ứng phó sự cố của đội ngũ kỹ sư khi hệ thống gặp lỗi hoặc bị tấn công.

### Giải pháp
Dự án GlobalMart tập trung thiết lập một hạ tầng mạng VPC Multi-AZ (4 Subnets, 2 NAT Gateways) để cô lập hoàn toàn môi trường chạy và lưu trữ dữ liệu. Quy trình phân quyền được siết chặt ngay từ đầu bằng hệ thống IAM Roles chi tiết cho từng tác vụ (Pipeline, Build, ECS, Deploy, RDS Monitoring). Chuỗi tích hợp và triển khai liên tục (CI/CD) được xây dựng tự động qua AWS CodePipeline kết hợp CodeBuild (bật Privileged mode để đóng gói Docker) và CodeDeploy (sử dụng chiến lược Blue/Green bảo mật). Toàn bộ trạng thái dữ liệu được bảo vệ bằng cụm RDS MySQL Multi-AZ đi kèm AWS RDS Proxy giúp tối ưu connection pooling. Hệ thống được giám sát đa lớp bằng CloudWatch Dashboard tập trung kết hợp bộ cảnh báo tự động thông qua Amazon SNS.

### Lợi ích và Hiệu quả Đầu tư
Dự án mang lại giá trị thực tế cao thông qua việc tối ưu hóa quy trình vận hành và chuẩn hóa bảo mật hạ tầng mà không phụ thuộc vào mã nguồn ứng dụng. Việc tự động hóa quy trình quét bảo mật khi push image (Scan on push tại ECR) giúp phát hiện sớm các lỗ hổng mã độc. Việc áp dụng cơ chế kết nối qua RDS Proxy bảo vệ database khỏi nguy cơ quá tải kết nối. Quan trọng nhất, các kịch bản diễn tập lỗigiúp chứng minh hạ tầng có khả năng tự động phục hồi tự động và chuyển vùng an toàn trong thời gian cực ngắn (<60 giây), giảm thiểu tối đa thời gian gián đoạn hệ thống trên môi trường Production.

### 3. Kiến trúc Giải pháp 
Kiến trúc dự án được thiết lập đồng bộ trên 2 vùng sẵn sàng Availability Zone A và Availability Zone B nhằm đạt tiêu chí High Availability. Các tầng mạng, điều phối container, định tuyến và lưu trữ dữ liệu đều được phân tách rõ ràng qua ma trận phân quyền Security Groups bảo mật. Chi tiết sơ đồ hạ tầng được mô tả dưới đây:

![Kiến trúc Triển khai GlobalMart](/images/2-Proposal/GlobalMart.png)

### Các Dịch vụ AWS Được Sử Dụng
- **AWS VPC & IAM**: Thiết lập mạng lõi 4 Subnets, 2 NAT Gateways tránh SPOF, 2 Route Tables Private riêng biệt và ma trận phân quyền IAM Roles chi tiết.
- **AWS CodePipeline, CodeBuild & CodeDeploy**: Bộ ba dịch vụ xây dựng pipeline tự động 3 giai đoạn (Source --> Build --> Deploy) và triển khai Blue/Green an toàn.
- **Amazon ECR & S3 Artifact Bucket**: Kho lưu trữ Docker Image tích hợp quét bảo mật (Scan on push) và lưu trữ trạng thái build có bật vòng đời (Lifecycle policy 30 ngày) kèm kiểm soát phiên bản (Versioning).
- **Amazon ECS Cluster & AWS Fargate**: Điều phối container không máy chủ (Serverless) tích hợp tính năng theo dõi chuyên sâu Container Insights.
- **AWS Application Load Balancer (ALB)**: Hệ thống 2 bộ ALB độc lập (Public ALB span 2 AZ với 2 Target Groups cho Blue/Green; Internal ALB span 2 AZ để kết nối Frontend và Backend nội bộ).
- **Amazon RDS for MySQL, Secrets Manager & RDS Proxy**: Cụm database Multi-AZ lưu trữ thông tin bảo mật qua Secrets Manager, giao tiếp gián tiếp bắt buộc qua lớp RDS Proxy để tối ưu failover transparent.
- **Amazon CloudWatch & Amazon SNS**: Bộ đôi thu thập tập trung logs/metrics đa lớp và phân phối cảnh báo tự động qua Email/SMS.

### Thiết kế Thành phần 
- **Hạ tầng Mạng & Bảo mật**: Thiết lập mạng VPC gồm 2 Public Subnet và 2 Private Subnet. Sử dụng 6 Security Groups riêng biệt để cô lập từng thành phần (bao gồm sg-rds-proxy). Ứng dụng Frontend được xếp vào phân vùng mạng Private Subnet A (AZ-A) và Backend nằm tại Private Subnet B (AZ-B).
- **Chuỗi Tự động hóa CI/CD**: Cấu hình các file khai báo `buildspec.yml`, `appspec.yml`, `taskdef.json` trực tiếp trong GitHub repo để CodePipeline điều phối. Quá trình build trong CodeBuild chạy ở Privileged mode phục vụ đóng gói Docker Image Frontend và Backend.
- **Kết nối & Vận hành Dữ liệu**: Khởi tạo DB Subnet Group phủ qua cả 2 AZ. Tầng Backend chỉ kết nối an toàn với database qua Endpoint của RDS Proxy, chặn tuyệt đối mọi hành vi kết nối trực tiếp vào RDS MySQL.
- **Giám sát & Lưu vết Nhật ký**: CloudWatch Logs thu thập toàn bộ dữ liệu từ Container, dữ liệu luồng mạng qua VPC Flow Logs và vết định tuyến tại ALB. Cấu hình CloudWatch Dashboard Multi-AZ tập trung hiển thị trực quan qua 9 widgets chỉ số.
- **Dự phòng & Diễn tập Khôi phục**: Hệ thống tự động thiết lập chính sách sao lưu RDS Automated Backup trong vòng 7 ngày và chuyển dịch an toàn về S3 Backup Bucket độc lập.

### 4. Triển khai Kỹ thuật
**Các Giai đoạn Triển khai**
Tiến trình thực hiện dự án được lên kế hoạch chi tiết theo thời gian 1 tháng, tập trung hoàn toàn vào các bước cấu hình, tích hợp hạ tầng hệ thống:
- **Tuần 1 - Cốt lõi Mạng & Bảo mật Phân lớp**: Nghiên cứu yêu cầu, thiết lập hệ thống IAM Roles phân quyền chi tiết. Khởi tạo mạng lõi VPC Multi-AZ gồm 4 Subnets, cấu hình 2 NAT Gateways độc lập để tránh điểm lỗi đơn lẻ (SPOF) và gán 2 Route Tables Private riêng cho từng AZ. Xây dựng ma trận bảo mật đầu quy trình với 6 Security Groups (bao gồm sg-rds-proxy).
- **Tuần 2 - Đóng gói & Chuẩn bị Tự động hóa CI/CD**: Khởi tạo các kho lưu trữ ECR Frontend và Backend có bật tính năng scan on push. Viết các file cấu hình và định nghĩa pipeline (`buildspec.yml`, `appspec.yml`, `taskdef.json`). Khởi tạo S3 Artifact Bucket có bật tính năng versioning cùng quy trình vòng đời (lifecycle 30 ngày). Cấu hình CodeBuild Project bật Privileged mode để đóng gói Docker.
- **Tuần 3 - Khởi tạo Runtime Container & Định tuyến Lưu lượng**: Cấu hình chuỗi CodePipeline hoàn chỉnh 3 giai đoạn. Khởi tạo cụm ECS Cluster Fargate tích hợp Container Insights cùng hệ thống Task Definitions chi tiết. Thiết lập cặp đôi ALB Public (gắn 2 Target Groups phục vụ Blue/Green) và ALB Internal chạy song song trên 2 AZ. Triển khai cấu hình CodeDeploy nâng cao.
- **Tuần 4 - Thiết lập Tầng Dữ liệu HA, Hệ thống Giám sát & Diễn tập Thảm họa**: Tạo DB Subnet Group, thiết lập hệ thống RDS MySQL Multi-AZ (Primary AZ-A + Standby AZ-B) bảo mật thông tin bằng Secrets Manager. Tích hợp cấu hình RDS Proxy quản lý connection pooling. Đồng thời, cấu hình CloudWatch Logs, Dashboard tập trung (9 widgets) cùng bộ 8 Alarms cảnh báo tự động thông qua SNS. Tiến hành chạy kịch bản diễn tập DR Drill (RDS Failover dưới 60 giây) và thực hiện End-to-End Test toàn hệ thống trước khi nghiệm thu.

**Yêu cầu Kỹ thuật**
- **Kỹ năng Cấu hình Hạ tầng & IaC**: Hiểu rõ cách liên kết mạng, viết và tối ưu hóa các tệp cấu hình trung gian cho hệ thống container (`taskdef.json`) và điều phối pipeline (`appspec.yml`).
- **Tư duy Bảo mật & Giám sát**: Năng lực phân tách quyền hạn (Least Privilege), hiểu rõ cơ chế hoạt động của VPC Flow Logs và thiết lập logic ma trận lọc điều kiện cảnh báo (CloudWatch Alarms) tương ứng với các lỗi thảm họa hệ thống đám mây.

### 5. Lộ trình & Cột mốc 
**Tiến độ Dự án**
Lộ trình thực hiện dự án được chia nhỏ và bám sát theo tiến độ triển khai hạ tầng thực tế trong vòng 1 tháng:
- Cột mốc 1 (Cuối Tuần 1): Hoàn thành thiết lập toàn bộ lõi mạng bảo mật VPC Multi-AZ, ma trận Security Groups nhằm loại bỏ điểm lỗi đơn lẻ ở tầng mạng biên.
- Cột mốc 2 (Cuối Tuần 2): Hoàn thành cấu hình và tích hợp thành công các kho lưu trữ hình ảnh container (ECR), chuẩn bị xong các file cấu hình định nghĩa để tự động hóa luồng đóng gói.
- Cột mốc 3 (Cuối Tuần 3): Chuỗi CodePipeline tự động hóa hoàn toàn luồng CI/CD đi vào hoạt động ổn định, triển khai thành công cụm ECS Fargate và cặp bộ cân bằng tải ALB đa vùng.
- Cột mốc 4 (Cuối Tuần 4): Đưa tầng dữ liệu an toàn Multi-AZ, hệ thống RDS Proxy và bộ giám sát đa tầng CloudWatch vào hoạt động. Thực hiện thực tế thành công các kịch bản khôi phục thảm họa (DR Drill) dưới 60 giây và nghiệm thu dự án.

### 6. Ước tính Ngân sách (Budget Estimation)
### Chi phí hạ tầng
- Dịch vụ AWS:
    - Amazon ECS Fargate (Frontend): $8.50/tháng (2 tasks, 0.25 vCPU, 512 MB, chạy 24/7).
    - Amazon ECS Fargate (Backend): $20.73/tháng (2 tasks, 0.5 vCPU, 1 GB, chạy 24/7).
    - Public IPv4 (ECS + ALB + NAT): $29.20/tháng (8 địa chỉ IP công khai × $0.005/giờ).
    - NAT Gateway (AZ-A): $43.37/tháng (730 giờ, ~5 GB dữ liệu xử lý).
    - NAT Gateway (AZ-B): $43.37/tháng (730 giờ, ~5 GB dữ liệu xử lý).
    - ALB Internet-facing: $24.24/tháng (730 giờ, ~1 LCU/giờ).
    - ALB Internal: $19.40/tháng (730 giờ, LCU tối thiểu).
    - Data Transfer Out: $0.45/tháng (~5 GB truyền ra Internet).
    - Data Transfer Cross-AZ: $0.10/tháng (~10 GB từ Frontend sang Backend khác AZ).
    - RDS MySQL Multi-AZ (db.t3.micro): $34.84/tháng (Primary + Standby, chạy 24/7).
    - RDS Storage: $5.52/tháng (20 GB gp2 × 2 AZ).
    - RDS Proxy: $21.90/tháng (tối thiểu 2 vCPU, 730 giờ).
    - Secrets Manager: $0.41/tháng (1 secret, lời gọi API).
    - RDS Snapshot Export to S3: $0.24/tháng (~20 GB xuất ra).
    - CodePipeline: $1.00/tháng (1 pipeline đang hoạt động).
    - CodeBuild: $1.25/tháng (~50 lần build × 5 phút, general1.small).
    - Amazon ECR (Frontend + Backend): $0.58/tháng (4 GB lưu trữ, ~100 lần pull).
    - S3 Artifact Bucket: $0.04/tháng (1 GB, 1.000 requests).
    - S3 Backup Bucket: $0.58/tháng (20 GB, chuyển sang Glacier sau 30 ngày).
    - CloudWatch Logs: $3.95/tháng (5 GB ingestion, logs container + ALB + VPC Flow).
    - CloudWatch Metrics & Alarms: $6.60/tháng (20 metrics, 8 alarms).
    - CloudWatch Dashboard: $3.00/tháng (1 dashboard, 9 widgets).
    - AWS Backup: $1.95/tháng (lịch backup hàng ngày + hàng tuần, ~30 GB vault).
    - Amazon SNS: $0.00/tháng (dưới 1.000 email thông báo, free tier).
    - Amazon EventBridge: $0.00/tháng (dưới 1 triệu sự kiện/tháng, free tier).

Tổng: $271.22/tháng

### 7. Đánh giá Rủi ro
#### Ma trận Rủi ro
- Lỗ hổng bảo mật trong Docker Image: Tác động trung bình, xác suất xảy ra trung bình.
- Quá tải hoặc cạn kiệt kết nối database: Tác động cao, xác suất xảy ra trung bình.
- Lỗi phân quyền chéo giữa các dịch vụ (IAM Misconfiguration): Tác động cao, xác suất xảy ra thấp.

#### Chiến lược Giảm thiểu
- Lỗ hổng Image: Được xử lý triệt để nhờ cơ chế Scan on push tự động của ECR để phát hiện mã độc ngay khi kết thúc bước Build.
- Quá tải kết nối: Được kiểm soát hoàn toàn thông qua lớp quản lý tập trung AWS RDS Proxy (connection pooling) giúp gom cụm và tái sử dụng kết nối thông minh.
- Lỗi phân quyền: Áp dụng nguyên tắc đặc quyền tối thiểu (Principle of Least Privilege), kiểm tra ma trận Security Groups và cấu hình chặt chẽ IAM Roles chuyên biệt cho từng hạng mục dịch vụ đám mây từ ngày đầu tiên.

#### Kế hoạch Dự phòng 
- Thực hiện DR Drill định kỳ: Giả lập lỗi sập database chính để kích hoạt tính năng chuyển vùng tự động Failover của RDS Multi-AZ sang vùng Standby với thời gian gián đoạn mục tiêu dưới 60 giây.
- Kích hoạt phục hồi trạng thái từ các bản sao lưu tự động S3 Backup Bucket trong trường hợp gặp sự cố toàn vẹn dữ liệu nghiêm trọng.

### 8. Kết quả Mong đợi
#### Cải tiến Kỹ thuật:
Xây dựng thành công một hạ tầng đám mây tự động hoàn toàn đạt tiêu chuẩn Production thực tế: tự động hóa chuỗi bàn giao mã nguồn 3 giai đoạn bảo mật, thiết lập mạng lưới Multi-AZ High Availability chống điểm lỗi đơn lẻ, tối ưu hóa giao tiếp dữ liệu qua Proxy và làm chủ hệ thống giám sát, cảnh báo sự cố chủ động trước khi lỗi ảnh hưởng tới trải nghiệm của người dùng cuối.
#### Giá trị Lâu dài
Mang lại một tài liệu mẫu chuẩn chỉnh (Blueprint) về mặt kiến trúc hạ tầng, quy trình DevOps và Platform Engineering thực tế cho doanh nghiệp; phục vụ như một nền tảng hạ tầng bảo mật vững chắc để triển khai nhanh chóng các hệ thống dịch vụ full-stack hoặc microservices phức tạp sau này.