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

![image.png]([image.png](https://github.com/user-attachments/assets/9da5e77f-1285-4877-9e31-346b759fb544))

### 1.1 Create Topic

![image.png](image%201.png)

### 1.2 Type → `Standard`, Name → `Global-Topic`, Keep Default Settings → Click Create Topic

![image.png](image%202.png)

## 2. Create Email Subscription

![image.png](image%203.png)

### 2.1 Protocol → `Email`, Endpoint → [`Rajpardeshi205@gmail.com`](mailto:Rajpardeshi205@gmail.com) , Click Create Subscription

![image.png](image%204.png)

![image.png](image%205.png)

### 2.2 Status

Initially:

```
Pending Confirmation
```

![image.png](image%206.png)

### 2.3 Check Mail For Activate → Click Confirm Subscription

![image.png](image%207.png)

![image.png](image%208.png)

### 2.4 Status

After Confirmation:

```
Confirmed
```

![image.png](image%209.png)

## 3. Test SNS Notification

![image.png](image%2010.png)

### 3.1 Add Subject And Message → Click Publish Message

![image.png](image%2011.png)

![image.png](image%2012.png)

### 3.2 **Verification →** Check Your Email Inbox.

Expected Result:

```
SNS Email Notification Received
```

![image.png](image%2013.png)

## 4.  **Create CloudWatch Alarm**

![image.png](image%2014.png)

### 4.1 Create Alarm → Type → Classic, Select Metric:

```
EC2
→ Per-Instance Metrics
→ CPUUtilization
```

![image.png](image%2015.png)

#### 4.2 Configure → Period → 30 Seconds

```jsx
Period: 30 Seconds 
Threshold Type: Static 
Condition: Greater Than 
Threshold Value: 30%
```

![image.png](image%2016.png)

#### 4.3 Select Threshold (When Alarm Gonna Triggers) → 30% Utilization

```jsx
When Threshold Is Reached:

Stop This EC2 Instance
```

![image.png](image%2017.png)

### 4.4 Configure Notification→ Select An Existing SNS Topic → `Global-Topic`

Whenever The Alarm Enters The Alarm State:

```
Send Notification
```

![image.png](image%2018.png)

### 4.5 EC2 Action → Add EC2 Action

![image.png](image%2019.png)

### 4.6 Add EC2 Action

```
Stop This Instance
```

![image.png](image%2020.png)

### 4.7 Alarm Name → `Global-Alarm`

![image.png](image%2021.png)

![image.png](image%2022.png)

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

![image.png](image%2023.png)

## 6. Monitoring And Verification

### 6.1 CloudWatch Dashboard

```jsx
Monitor:

CPUUtilization

Expected Result:

CPU Usage > 30%
```

![image.png](image%2024.png)

### 6.2  Alarm Status

```jsx
Expected State:

In Alarm
```

![image.png](image%2025.png)

### 6.3 **EC2 Instance State**

```jsx
Expected State:

Stopped

CloudWatch Automatically Stops The Instance When CPU Utilization Exceeds The Configured Threshold.
```

![image.png](image%2026.png)

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

![image.png](image%2027.png)

# Learning Outcomes

- Amazon SNS Fundamentals
- CloudWatch Metrics And Alarms
- EC2 Automated Actions
- Infrastructure Monitoring
- Event-Driven Automation
- AWS Operational Best Practices

# Conclusion

This Project Demonstrates A Cloud-Native Monitoring And Alerting Solution On AWS Using Amazon CloudWatch And Amazon SNS. When CPU Utilization Exceeds The Defined Threshold, CloudWatch Automatically Triggers An Alarm, Sends An Email Notification Through SNS, And Stops The EC2 Instance. This Implementation Showcases Real-Time Monitoring, Automated Remediation, And Cost-Effective Infrastructure Management Using AWS Services.
