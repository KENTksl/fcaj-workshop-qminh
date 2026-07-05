---
title : "Thiết lập CloudWatch và Amazon SNS"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.2.8. </b> "
---

## Mục tiêu

Thiết lập hệ thống giám sát và cảnh báo cho hạ tầng GlobalMart bằng Amazon CloudWatch và Amazon SNS, giúp theo dõi trạng thái hoạt động của các dịch vụ, thu thập nhật ký, trực quan hóa các chỉ số vận hành và gửi thông báo khi phát hiện sự cố.

---

## 1. Cấu hình CloudWatch Logs

Log từ Frontend và Backend containers được đẩy tự động lên CloudWatch Logs
thông qua cấu hình **awslogs log driver** trong ECS Task Definition.
Mỗi service có một Log Group riêng biệt để dễ truy vết.

**Tạo 2 Log Groups:**

Vào **CloudWatch → Log groups → Create log group**

| Log Group | Retention | Service |
|---|---|---|
| `/ecs/globalmart-frontend-task` | 30 days | ECS Frontend container |
| `/ecs/globalmart-backend-task` | 30 days | ECS Backend container |

![Overview](/images/5-Workshop/Workshop-img/cloudwatch-1.jpg)
Click vào **Create**
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-2.jpg)

---

## 2. Thu thập Metrics và tạo Dashboard

Tạo CloudWatch Dashboard tổng hợp toàn bộ hệ thống để theo dõi một màn hình.

Vào **CloudWatch → Dashboards → Create dashboard**

**Tên dashboard:** `globalmart-ops-dashboard`
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-3.jpg)

Vào **Dashboards mới tạo** → Click vào **icon add widget** góc phải trên → 6 widgets được cấu hình:

| Tên Giao Diện (Widget Title) | Loại Đồ Thị (Widget Type) | Đường Dẫn Cấu Hình Chỉ Số (Metrics Path & Dimensions) | Mục Đích Theo Dõi |
| :--- | :--- | :--- | :--- |
| **ECS CPU Utilization** | Line chart (Đồ thị đường) | ECS → ClusterName + ServiceName:<br>• `globalmart-frontend-task`: CPUUtilization <br>• `globalmart-backend-task`: CPUUtilization | Phát hiện container bị quá tải năng lực xử lý CPU. |
| **ECS Memory Utilization** | Line chart (Đồ thị đường) | ECS → ClusterName + ServiceName:<br>• `globalmart-frontend-task`: MemoryUtilization <br>• `globalmart-backend-task`: MemoryUtilization | Phát hiện sớm các lỗi tràn bộ nhớ (Memory leak) của ứng dụng. |
| **ALB Request Count** | Line chart (Đồ thị đường) | ApplicationELB → Chọn Load Balancer `globalmart-alb-public` → RequestCount | Theo dõi tổng số lượng lưu lượng truy cập từ người dùng. |
| **ALB HTTP Errors** | Line chart (Đồ thị đường) | ApplicationELB → Chọn Load Balancer `globalmart-alb-public`:<br>• HTTPCode_ELB_4XX_Count <br>• HTTPCode_ELB_5XX_Count | Phát hiện tỷ lệ lỗi phản hồi từ phía client (4xx) và lỗi hệ thống sập nguồn phía server (5xx). |
| **RDS Performance** | Line chart (Đồ thị đường) | RDS → Chọn DB Instance `globalmart-db`:<br>• CPUUtilization<br>• DatabaseConnections | Theo dõi sức chịu tải và số lượng phiên kết nối đồng thời vào Database. |
| **Alarm Overview** | Alarm status (Trạng thái) | Chọn cấu hình hiển thị trạng thái tích hợp dữ liệu của **tất cả 6 Alarms** vừa khởi tạo. | Cung cấp cái nhìn tổng quan nhanh nhất về độ an toàn của toàn bộ hạ tầng. |


**Bước 1**: Loại Đồ Thị
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-5.jpg)
**Bước 2**: Đường Dẫn Cấu Hình Chỉ Số

![Overview](/images/5-Workshop/Workshop-img/cloudwatch-6.jpg)

**Bước 2**: Mục đích Theo Dõi
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-7.jpg)


**Bước 4**: Save Dashboard
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-4.jpg)

---

## 3. Tạo Alarms và SNS Notifications

### 3.1 Tạo SNS Topic

Vào **SNS** → **Topics** → **Create topic**:
* **Type**: Standard
* **Name**: `globalmart-alerts`
![Overview](/images/5-Workshop/Workshop-img/sns-1.jpg)

Vào **Topic vừa tạo** → **Add subscription**:
* **Type**: Email
* **Endpoint**: `your-email@example.com`
![Overview](/images/5-Workshop/Workshop-img/sns-3.jpg)
> Sau khi tạo, vào hòm thư email và click **Confirm subscription**
> thì sẽ chuyển sang Status là **Confirmed**.
![Overview](/images/5-Workshop/Workshop-img/sns-2.jpg)


### 3.2 Cấu hình 6 CloudWatch Alarms
Vào **CloudWatch** → **Alarms** → Chọn **Create alarm** và tiến hành lặp lại các bước cấu hình tương ứng cho 6 bộ cảnh báo dưới đây:

#### 1. Alarm 1 — ECS Frontend CPU cao
* **Select metric**: `ECS` → `ClusterName, ServiceName`
  * **ClusterName**: `globalmart-cluster`
  * **ServiceName**: `globalmart-frontend-task`
  * **Metric**: `CPUUtilization`
  ![Overview](/images/5-Workshop/Workshop-img/alarm-1.jpg)
* **Conditions**:
  * **Threshold**: Greater than `80`
  * **Period**: `5 minutes`
  * **Datapoints to alarm**: `2 out of 2`
   ![Overview](/images/5-Workshop/Workshop-img/alarm-2.jpg)
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
 ![Overview](/images/5-Workshop/Workshop-img/alarm-3.jpg)
* **Name**: `globalmart-frontend-cpu-high`

> Thực hiện tương tự cho các Alarm khác:
#### 2. Alarm 2 — ECS Backend CPU cao
* **Select metric**: `ECS` → `ClusterName, ServiceName`
  * **ClusterName**: `globalmart-cluster`
  * **ServiceName**: `globalmart-backend-task`
  * **Metric**: `CPUUtilization`
* **Conditions**:
  * **Threshold**: Greater than `80`
  * **Period**: `5 minutes`
  * **Datapoints to alarm**: `2 out of 2`
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
* **Name**: `globalmart-backend-cpu-high`

#### 3. Alarm 3 — ALB 5xx Errors (Lỗi hệ thống cân bằng tải)
* **Select metric**: `ApplicationELB` → `Per AppELB Metrics`
  * **Load Balancer**: `globalmart-alb-public`
  * **Metric**: `HTTPCode_ELB_5XX_Count`
* **Conditions**:
  * **Threshold**: Greater than `10`
  * **Period**: `1 minute`
  * **Datapoints to alarm**: `1 out of 1`
  * **Missing data treatment**: `Treat missing data as good (not breaching)`
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
* **Name**: `globalmart-alb-5xx-errors`

#### 4. Alarm 4 — RDS Storage thấp (Sắp hết dung lượng Database)
* **Select metric**: `RDS` → `Per-Database Metrics`
  * **DBInstanceIdentifier**: `globalmart-db`
  * **Metric**: `FreeStorageSpace`
* **Conditions**:
  * **Threshold**: Less than `5368709120` *(5GB quy đổi thành bytes)*
  * **Period**: `5 minutes`
  * **Datapoints to alarm**: `1 out of 1`
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
* **Name**: `globalmart-rds-storage-low`

#### 5. Alarm 5 — RDS CPU cao
* **Select metric**: `RDS` → `Per-Database Metrics`
  * **DBInstanceIdentifier**: `globalmart-db`
  * **Metric**: `CPUUtilization`
* **Conditions**:
  * **Threshold**: Greater than `80`
  * **Period**: `5 minutes`
  * **Datapoints to alarm**: `3 out of 3`
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
* **Name**: `globalmart-rds-cpu-high`

#### 6. Alarm 6 — API Gateway 5xx Error
* **Select metric**: `ApiGateway` → `By Api Name`
  * **ApiName**: `globalmart-api`
  * **Metric**: `5XXError`
* **Conditions**:
  * **Threshold**: Greater than `10`
  * **Period**: `1 minute`
  * **Missing data treatment**: `Treat missing data as good (not breaching)`
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
* **Name**: `globalmart-apigateway-5xx`

---
 *Hình: Danh sách hệ thống 6 CloudWatch Alarms đã khởi tạo hoàn chỉnh ở trạng thái màu xanh lá cây (OK).*

---
 ![Overview](/images/5-Workshop/Workshop-img/alarm-4.jpg)

## Kết quả đạt được

Sau khi hoàn thành bài thực hành này, hệ thống GlobalMart đã được bổ sung đầy đủ khả năng giám sát và cảnh báo vận hành.

| Thành phần | Trạng thái |
|---|---|
| CloudWatch Log Groups | Đã tạo cho Frontend và Backend |
| ECS Containers Logs | Được gửi tự động lên CloudWatch Logs |
| CloudWatch Dashboard | Đã tạo với các widget theo dõi toàn bộ hệ thống |
| ECS Metrics | Theo dõi CPU và Memory Utilization |
| Application Load Balancer Metrics | Theo dõi Request Count và HTTP Errors |
| Amazon RDS Metrics | Theo dõi CPU, Database Connections và Free Storage |
| CloudWatch Alarms | Đã cấu hình 6 cảnh báo cho ECS, ALB, RDS và API Gateway |
| Amazon SNS Topic | Đã tạo và liên kết với CloudWatch Alarms |
| Email Notification | Đã xác nhận (Confirmed) và sẵn sàng nhận cảnh báo |