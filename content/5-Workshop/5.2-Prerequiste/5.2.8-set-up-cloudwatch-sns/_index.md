---
title: "Set up CloudWatch and Amazon SNS"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.2.8. </b> "
---

## Objectives

Set up monitoring and alerting system for GlobalMart infrastructure using Amazon CloudWatch and Amazon SNS, helping track the operational status of services, collect logs, visualize operational metrics, and send notifications when incidents are detected.

---

## 1. Configure CloudWatch Logs

Logs from Frontend and Backend containers are automatically pushed to CloudWatch Logs
through the **awslogs log driver** configuration in the ECS Task Definition.
Each service has a separate Log Group for easier tracing.

**Create 2 Log Groups:**

Go to **CloudWatch → Log groups → Create log group**

| Log Group | Retention | Service |
|---|---|---|
| `/ecs/globalmart-frontend-task` | 30 days | ECS Frontend container |
| `/ecs/globalmart-backend-task` | 30 days | ECS Backend container |

![Overview](/images/5-Workshop/Workshop-img/cloudwatch-1.jpg)
Click **Create**
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-2.jpg)

---

## 2. Collect Metrics and create Dashboard

Create a comprehensive CloudWatch Dashboard to monitor the entire system from a single screen.

Go to **CloudWatch → Dashboards → Create dashboard**

**Dashboard name:** `globalmart-ops-dashboard`
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-3.jpg)

Go to **Newly created Dashboard** → Click the **add widget icon** in the top-right corner → 6 widgets are configured:

| Widget Title | Widget Type | Metrics Path & Dimensions | Monitoring Purpose |
| :--- | :--- | :--- | :--- |
| **ECS CPU Utilization** | Line chart | ECS → ClusterName + ServiceName:<br>• `globalmart-frontend-task`: CPUUtilization <br>• `globalmart-backend-task`: CPUUtilization | Detect containers with overloaded CPU processing capacity. |
| **ECS Memory Utilization** | Line chart | ECS → ClusterName + ServiceName:<br>• `globalmart-frontend-task`: MemoryUtilization <br>• `globalmart-backend-task`: MemoryUtilization | Early detection of memory leak errors in the application. |
| **ALB Request Count** | Line chart | ApplicationELB → Select Load Balancer `globalmart-alb-public` → RequestCount | Track total user traffic volume. |
| **ALB HTTP Errors** | Line chart | ApplicationELB → Select Load Balancer `globalmart-alb-public`:<br>• HTTPCode_ELB_4XX_Count <br>• HTTPCode_ELB_5XX_Count | Detect error rates from client-side (4xx) and server-side system errors (5xx). |
| **RDS Performance** | Line chart | RDS → Select DB Instance `globalmart-db`:<br>• CPUUtilization<br>• DatabaseConnections | Monitor load capacity and number of concurrent database connections. |
| **Alarm Overview** | Alarm status | Select to display status integration of **all 6 Alarms** just created. | Provide the quickest overview of the entire infrastructure's safety. |


**Step 1**: Widget Type
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-5.jpg)
**Step 2**: Metrics Path & Dimensions

![Overview](/images/5-Workshop/Workshop-img/cloudwatch-6.jpg)

**Step 2**: Monitoring Purpose
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-7.jpg)


**Step 4**: Save Dashboard
![Overview](/images/5-Workshop/Workshop-img/cloudwatch-4.jpg)

---

## 3. Create Alarms and SNS Notifications

### 3.1 Create SNS Topic

Go to **SNS** → **Topics** → **Create topic**:
* **Type**: Standard
* **Name**: `globalmart-alerts`
![Overview](/images/5-Workshop/Workshop-img/sns-1.jpg)

Go to **Newly created Topic** → **Add subscription**:
* **Type**: Email
* **Endpoint**: `your-email@example.com`
![Overview](/images/5-Workshop/Workshop-img/sns-3.jpg)
> After creating, go to your email inbox and click **Confirm subscription**
> then it will change Status to **Confirmed**.
![Overview](/images/5-Workshop/Workshop-img/sns-2.jpg)


### 3.2 Configure 6 CloudWatch Alarms
Go to **CloudWatch** → **Alarms** → Select **Create alarm** and repeat the configuration steps for the 6 alarms below:

#### 1. Alarm 1 — ECS Frontend High CPU
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

> Repeat the same for other Alarms:
#### 2. Alarm 2 — ECS Backend High CPU
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

#### 3. Alarm 3 — ALB 5xx Errors
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

#### 4. Alarm 4 — RDS Low Storage
* **Select metric**: `RDS` → `Per-Database Metrics`
  * **DBInstanceIdentifier**: `globalmart-db`
  * **Metric**: `FreeStorageSpace`
* **Conditions**:
  * **Threshold**: Less than `5368709120` *(5GB converted to bytes)*
  * **Period**: `5 minutes`
  * **Datapoints to alarm**: `1 out of 1`
* **Actions**: Add notification → **Alarm state trigger**: `In alarm` → **SNS Topic**: `globalmart-alerts`
* **Name**: `globalmart-rds-storage-low`

#### 5. Alarm 5 — RDS High CPU
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
 *Image: Complete list of 6 CloudWatch Alarms created in green (OK) status.*

---
 ![Overview](/images/5-Workshop/Workshop-img/alarm-4.jpg)

## Expected results

After completing this lab, GlobalMart system has been fully supplemented with monitoring and operational alerting capabilities.

| Component | Status |
|---|---|
| CloudWatch Log Groups | Created for Frontend and Backend |
| ECS Containers Logs | Automatically sent to CloudWatch Logs |
| CloudWatch Dashboard | Created with system-wide monitoring widgets |
| ECS Metrics | Track CPU and Memory Utilization |
| Application Load Balancer Metrics | Track Request Count and HTTP Errors |
| Amazon RDS Metrics | Track CPU, Database Connections and Free Storage |
| CloudWatch Alarms | 6 alarms configured for ECS, ALB, RDS and API Gateway |
| Amazon SNS Topic | Created and linked to CloudWatch Alarms |
| Email Notification | Confirmed and ready to receive alerts |
