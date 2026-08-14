# Cloud-Native Monitoring And Alerting Solution On AWS

# Project Overview

This Project Demonstrates How To Use Amazon SNS (Simple Notification Service) And Amazon CloudWatch To Automatically Monitor An EC2 Instance And Perform Actions When A Defined Threshold Is Reached.

In This Implementation:

- Amazon SNS Sends Email Notifications.
- Amazon CloudWatch Monitors EC2 CPU Utilization.
- CloudWatch Alarm Triggers When CPU Usage Exceeds 30%.
- SNS Sends An Alert Email.
- EC2 Instance Is Automatically Stopped.
- Stress Tool Is Used To Simulate High CPU Usage.

# Architecture

![Architecture Diagram](https://github.com/user-attachments/assets/9559200c-3952-4ec4-a806-2bed26536957)

# AWS Services Used

| Service | Purpose |
| --- | --- |
| Amazon EC2 | Compute Instance |
| Amazon SNS | Email Notification Service |
| Amazon CloudWatch | Monitoring And Alerting |

# **Implementation Steps**

## 1. Go Create SNS Topic

![image.png](https://github.com/user-attachments/assets/9da5e77f-1285-4877-9e31-346b759fb544)

### 1.1 Create Topic

![image%201.png](https://github.com/user-attachments/assets/ec1794fe-96d2-4d3f-baf8-cfd39443d04f)

### 1.2 Type → `Standard`, Name → `Global-Topic`, Keep Default Settings → Click Create Topic

![image%202.png](https://github.com/user-attachments/assets/547054b6-7ab3-4848-bbd2-771af9e2c252)

## 2. Create Email Subscription

![image%203.png](https://github.com/user-attachments/assets/c75a5075-a23f-47e1-a60f-1c8d8371dc38)

### 2.1 Protocol → `Email`, Endpoint → [`Rajpardeshi205@gmail.com`](mailto:Rajpardeshi205@gmail.com) , Click Create Subscription

![image%204.png](https://github.com/user-attachments/assets/51508005-a81e-47bd-8dfd-c8f0ec0f3b9b)

![image%205.png](https://github.com/user-attachments/assets/4533b23b-9756-431f-9a23-89ce1efdf3a7)

### 2.2 Status

Initially:

```
Pending Confirmation
```

![image%206.png](https://github.com/user-attachments/assets/a0fd1cf2-a978-4b97-b31c-6d3162bdf840)

### 2.3 Check Mail For Activate → Click Confirm Subscription

![image%207.png](https://github.com/user-attachments/assets/d3504d30-0da4-4455-9586-411f15f7b074)

![image%208.png](https://github.com/user-attachments/assets/4430df0d-2d2f-49f9-a9ac-d1d0cbccb343)

### 2.4 Status

After Confirmation:

```
Confirmed
```

![image%209.png](https://github.com/user-attachments/assets/b64d08b2-2fdc-4835-b374-58a8e2a1266a)

## 3. Test SNS Notification

![image%2010.png](https://github.com/user-attachments/assets/7189b210-47ae-4f58-ae54-8124619bfadf)

### 3.1 Add Subject And Message → Click Publish Message

![image%2011.png](https://github.com/user-attachments/assets/2e09bb47-b3d6-44a7-b4b2-1898d94347a7)

![image%2012.png](https://github.com/user-attachments/assets/7a579b39-50ae-4ecd-a450-3c6bd7434351)

### 3.2 **Verification →** Check Your Email Inbox.

Expected Result:

```
SNS Email Notification Received
```

![image%2013.png](https://github.com/user-attachments/assets/85ec4b9e-6104-49ab-9638-805369824691)

## 4.  **Create CloudWatch Alarm**

![image%2014.png](https://github.com/user-attachments/assets/bb371fc0-ddd7-41f1-b8b1-6a7e40f4e47b)

### 4.1 Create Alarm → Type → Classic, Select Metric:

```
EC2
→ Per-Instance Metrics
→ CPUUtilization
```

![image%2015.png](https://github.com/user-attachments/assets/e355b31e-c4fe-4cfc-b3e2-36d0d00164d3)

#### 4.2 Configure → Period → 30 Seconds

```jsx
Period: 30 Seconds 
Threshold Type: Static 
Condition: Greater Than 
Threshold Value: 30%
```

![image%2016.png](https://github.com/user-attachments/assets/8e485527-bb9b-4ab4-b432-d0d08c90e597)

#### 4.3 Select Threshold (When Alarm Gonna Triggers) → 30% Utilization

```jsx
When Threshold Is Reached:

Stop This EC2 Instance
```

![image%2017.png](https://github.com/user-attachments/assets/94c12230-483e-4895-8b6b-e40fea23f74d)

### 4.4 Configure Notification→ Select An Existing SNS Topic → `Global-Topic`

Whenever The Alarm Enters The Alarm State:

```
Send Notification
```

![image%2018.png](https://github.com/user-attachments/assets/80d9e946-75ae-4de3-870b-bd160d1f6ced)

### 4.5 EC2 Action → Add EC2 Action

![image%2019.png](https://github.com/user-attachments/assets/93d8c0b7-25a4-4374-b995-80f63c1f858b)

### 4.6 Add EC2 Action

```
Stop This Instance
```

![image%2020.png](https://github.com/user-attachments/assets/5f09c278-f8c3-4461-8910-efcfd269bfaf)

### 4.7 Alarm Name → `Global-Alarm`

![image%2021.png](https://github.com/user-attachments/assets/dd4dbc89-e717-4d28-a512-8602c48cbf6f)

![image%2022.png](https://github.com/user-attachments/assets/ba83da83-4004-4279-89bf-ac9d75b8fac8)

## 5. Generate CPU Load

### 5.1 Update Packages

```jsx
sudo yum update
```

### 5.2 Install Stress Tool

```jsx
 sudo yum install stress -y
```

### 5.3 Generate CPU Load

```jsx
 stress --cpu 40 --io 4 --vm 2 --vm-bytes 128M --timeout 5M &
 
stress -> Starts The Stress Testing Tool
--cpu 40 -> Creates 40 Worker Threads That Continuously Consume CPU Resources
--io 4 -> Creates 4 Threads Performing Continuous I/O Operations
--vm 2 -> Creates 2 Memory Allocation Workers
--vm-bytes 128M -> Each Memory Worker Allocates 128 MB RAM
--timeout 5M -> Runs The Test For 5 Minutes
& -> Runs The Process In The Background
jobs -> See The Process Running In Background
```

### 5.4 Verification

![image%2023.png](https://github.com/user-attachments/assets/c46ce364-105c-4666-98ce-73062352ca89)

## 6. Monitoring And Verification

### 6.1 CloudWatch Dashboard

```jsx
Monitor:

CPUUtilization

Expected Result:

CPU Usage > 30%
```

![image%2024.png](https://github.com/user-attachments/assets/7c9351d2-c08f-4c92-9583-967830167218)

### 6.2  Alarm Status

```jsx
Expected State:

In Alarm
```

![image%2025.png](https://github.com/user-attachments/assets/dcfc21dc-9ac5-4b06-a512-0aedba4ab900)

### 6.3 **EC2 Instance State**

```jsx
Expected State:

Stopped

CloudWatch Automatically Stops The Instance When CPU Utilization Exceeds The Configured Threshold.
```

![image%2026.png](https://github.com/user-attachments/assets/ab188c5a-7e72-423d-95f9-5a2457570a33)

### 6.4 **Email Notification**

Expected Result:

```
SNS Email Alert Received
```

Sample Notification:

```
ALARM: "Global-Alarm"

Threshold Crossed:
CPUUtilization > 30%

Instance Action:
Stopped
```

![image%2027.png](https://github.com/user-attachments/assets/a04c1724-91c8-4940-a594-bacdd806e526)

# Learning Outcomes

- Amazon SNS Fundamentals
- CloudWatch Metrics And Alarms
- EC2 Automated Actions
- Infrastructure Monitoring
- Event-Driven Automation
- AWS Operational Best Practices

# Conclusion

This Project Demonstrates A Cloud-Native Monitoring And Alerting Solution On AWS Using Amazon CloudWatch And Amazon SNS. When CPU Utilization Exceeds The Defined Threshold, CloudWatch Automatically Triggers An Alarm, Sends An Email Notification Through SNS, And Stops The EC2 Instance. This Implementation Showcases Real-Time Monitoring, Automated Remediation, And Cost-Effective Infrastructure Management Using AWS Services.
