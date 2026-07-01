---
title : "Prepare GitHub, IAM User Access Key, ECR and Notifications"
date : 2026-06-30 
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

## Objectives

Complete the configuration of all input resources for the automation pipeline: set up source code repository, initialize a privileged IAM User account, create and extract Access Key pair, prepare centralized Docker Image repositories, and set up secure environment variables on GitHub Actions.

## Detailed implementation steps

### 1. Prepare GitHub Repository structure
Ensure your source code is organized clearly separated in the Repository so GitHub Actions can accurately locate the build position:
- **Frontend** application directory (contains `Dockerfile` for serving the interface).
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

- Backend application directory (contains `Dockerfile` for serving Spring Boot application).
```dockerfile
# Stage 1: Build the application
# Use the correct Java 17 as configured in your pom.xml
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

### 2. Create IAM User and Extract Access Key on AWS
For GitHub Actions to have permissions to connect and interact with AWS resources via CLI, we need to create a dedicated programmatic user account:

1. Access **AWS Management Console** -> Search for **IAM** service -> In left menu select **Users** -> Select **Create user**.
   - **User name**: Enter exactly `github-actions-deployer`.
   - Click **Next**
![Overview](/images/5-Workshop/Workshop-img/IAM-1.jpg)
2. **Assign permission policies (Set permissions)**: 
   - Select **Attach policies directly**.
   - Search and attach policy **AmazonECRFullAccess**.
   - Click **Next** -> Select **Create user**.
   ![Overview](/images/5-Workshop/Workshop-img/IAM-2.jpg)
3. **Add additional permissions**: 
   - Select the newly created IAM User **`github-actions-deployer`**.
      ![Overview](/images/5-Workshop/Workshop-img/IAM-3.jpg)
   - Click on **Add permissions** -> **Create inline policy** -> **JSON**.
   - Paste the following JSON content:
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
   - Click **Next** -> Select **Create**.
   ![Overview](/images/5-Workshop/Workshop-img/IAM-4.jpg)
4. **Create credentials key pair**:
   - Click on the User **`github-actions-deployer`** just created -> Switch to **Security credentials** tab.
   - Scroll down to **Access keys** section -> Click **Create access key**.
   - On the usage purpose page, select **Command Line Interface (CLI)** -> Click **Next**.
   - **Description**: Enter `GitHub Actions Key`.
   - Click **Create access key**.
   - **IMPORTANT**: The system will display the key pair only once including **Access Key ID** and **Secret Access Key**. Click **Download .csv file** to store them carefully, never expose this key pair.
![Overview](/images/5-Workshop/Workshop-img/IAM-5.jpg)

### 3. Create Amazon ECR Repositories
1. On AWS search bar, enter **Elastic Container Registry** or **ECR** -> Select **Repositories** -> Select **Create repository**.
2. Proceed to create 2 repositories with security configuration:
   - First repository name: `globalmart-frontend`.
   - Second repository name: `globalmart-backend`.
   ![Overview](/images/5-Workshop/Workshop-img/ECR-1.jpg)
3. **Mandatory advanced item**: In **Image scan settings**, toggle **Scan on push** to green (`Enabled`) to activate automatic malware and vulnerability scanning immediately when GitHub pushes Images.
![Overview](/images/5-Workshop/Workshop-img/ECR-2.jpg)


### 4. Configure Secrets and Variables on GitHub
1. Access your Repository on **GitHub** -> Select **Settings** tab -> Left menu select **Secrets and variables** -> Select **Actions**.
2. On **Secrets** tab -> Select **New repository secret** to configure secure information:
   - `AWS_ACCESS_KEY_ID`: Paste the Access Key ID string from the CSV file in Step 2.
   - `AWS_SECRET_ACCESS_KEY`: Paste the encrypted Secret Access Key string from the CSV file in Step 2.
   - `MAIL_PASSWORD`: Enter the **App Password** consisting of 16 characters provided from your Google account.
   - `AWS_REGION`: Enter `ap-southeast-1`.
![Overview](/images/5-Workshop/Workshop-img/ECR-3.jpg)


## Expected results

After completing this lab, you must achieve the following results:
- IAM User account `github-actions-deployer` works stably and you keep the credential CSV file secure.
- Two ECR repositories (`globalmart-frontend` and `globalmart-backend`) are ready to receive push commands.
- All Access Keys, Secrets and Mail account have been securely encrypted on the GitHub system.
