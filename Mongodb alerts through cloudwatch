# MongoDB Alerts Setup on AWS – Step-by-Step README Guide

## Objective

This document explains how to configure monitoring and alerts for a MongoDB database hosted on AWS.

The setup includes:

* Monitoring MongoDB server health
* Creating CloudWatch alarms
* Configuring SNS email notifications
* Monitoring CPU, Memory, Disk, and Database availability

---

# Prerequisites

Before starting, ensure the following are available:

* AWS Account access
* EC2 instance running MongoDB
* IAM permissions for:

  * CloudWatch
  * SNS
  * EC2
* CloudWatch Agent installed on the MongoDB server
* Email ID for alert notifications

---

# Architecture Flow

MongoDB Server → CloudWatch Agent → CloudWatch Metrics → CloudWatch Alarm → SNS Notification → Email Alert

---

# Step 1 – Launch / Verify MongoDB EC2 Server

1. Login to AWS Console
2. Open EC2 Dashboard
3. Verify MongoDB server is running
4. Connect to the server using SSH

```bash
ssh -i key.pem ec2-user@<PUBLIC-IP>
```

---

# Step 2 – Install CloudWatch Agent

## For Amazon Linux / RHEL

```bash
sudo yum update -y
sudo yum install amazon-cloudwatch-agent -y
```

## For Ubuntu

```bash
sudo apt update
sudo apt install amazon-cloudwatch-agent -y
```

Verify installation:

```bash
amazon-cloudwatch-agent-ctl -a status
```

---

# Step 3 – Configure CloudWatch Agent

Run the configuration wizard:

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

Select:

* EC2 environment
* Collect metrics
* Enable disk metrics
* Enable memory metrics
* Enable CPU metrics
* Enable logs if required

Save the configuration.

---

# Step 4 – Start CloudWatch Agent

```bash
sudo systemctl enable amazon-cloudwatch-agent
sudo systemctl start amazon-cloudwatch-agent
```

Check status:

```bash
sudo systemctl status amazon-cloudwatch-agent
```

---

# Step 5 – Verify Metrics in CloudWatch

1. Open AWS Console
2. Go to CloudWatch
3. Navigate to:

```text
CloudWatch → Metrics → CWAgent
```

4. Verify metrics such as:

* CPUUtilization
* mem_used_percent
* disk_used_percent
* disk_inodes_used

---

# Step 6 – Create SNS Topic for Alerts

1. Open SNS Console
2. Click Create Topic
3. Select:

   * Type: Standard
4. Enter Topic Name:

```text
mongodb-alerts-topic
```

5. Click Create Topic

---

# Step 7 – Create Email Subscription

1. Open created SNS Topic
2. Click Create Subscription
3. Select Protocol:

```text
Email
```

4. Enter email address
5. Click Create Subscription
6. Open mailbox and confirm subscription

---

# Step 8 – Create CloudWatch Alarm for CPU Usage

1. Open CloudWatch
2. Navigate:

```text
CloudWatch → Alarms → Create Alarm
```

3. Select Metric:

```text
EC2 → Per-Instance Metrics → CPUUtilization
```

4. Select MongoDB EC2 instance
5. Configure threshold:

| Configuration  | Value        |
| -------------- | ------------ |
| Threshold Type | Static       |
| Condition      | Greater than |
| Value          | 80           |
| Period         | 5 Minutes    |
| Evaluation     | 2 Datapoints |

6. Select SNS Topic:

```text
mongodb-alerts-topic
```

7. Alarm Name:

```text
mongodb-high-cpu-alert
```

8. Create Alarm

---

# Step 9 – Create Memory Usage Alarm

Navigate:

```text
CWAgent → mem_used_percent
```

Recommended Threshold:

| Metric       | Threshold |
| ------------ | --------- |
| Memory Usage | > 80%     |

Alarm Name:

```text
mongodb-memory-alert
```

---

# Step 10 – Create Disk Usage Alarm

Navigate:

```text
CWAgent → disk_used_percent
```

Recommended Threshold:

| Metric     | Threshold |
| ---------- | --------- |
| Disk Usage | > 85%     |

Alarm Name:

```text
mongodb-disk-alert
```

---

# Step 11 – Create MongoDB Service Monitoring

Check MongoDB service status:

```bash
sudo systemctl status mongod
```

Enable auto-start:

```bash
sudo systemctl enable mongod
```

Restart MongoDB:

```bash
sudo systemctl restart mongod
```

---

# Step 12 – Configure Log Monitoring (Optional)

MongoDB logs location:

```text
/var/log/mongodb/mongod.log
```

Add logs to CloudWatch Agent configuration.

Restart agent:

```bash
sudo systemctl restart amazon-cloudwatch-agent
```

---

# Step 13 – Test Alerts

## CPU Test

Run stress test:

```bash
yes > /dev/null
```

Monitor CloudWatch alarm state.

Stop test:

```bash
CTRL + C
```

---

# Step 14 – Verify Email Notifications

Once threshold is breached:

* CloudWatch Alarm changes to ALARM state
* SNS triggers notification
* Email alert is received

---

# Recommended Monitoring Thresholds

| Metric               | Recommended Threshold |
| -------------------- | --------------------- |
| CPU Usage            | 80%                   |
| Memory Usage         | 80%                   |
| Disk Usage           | 85%                   |
| Status Check Failed  | 1                     |
| MongoDB Service Down | Immediate Alert       |

---

# Best Practices

* Enable detailed monitoring on EC2
* Use separate alarms for Prod and Dev environments
* Configure backup monitoring
* Monitor EBS burst balance
* Enable CloudWatch dashboards
* Keep SNS notifications configured for multiple users

---

# Useful AWS Services

| Service          | Purpose                   |
| ---------------- | ------------------------- |
| CloudWatch       | Monitoring & Metrics      |
| SNS              | Email Notifications       |
| EC2              | MongoDB Hosting           |
| IAM              | Access Control            |
| CloudWatch Agent | Custom Metrics Collection |

---

# Troubleshooting

## CloudWatch Metrics Not Visible

Check agent status:

```bash
sudo systemctl status amazon-cloudwatch-agent
```

Restart agent:

```bash
sudo systemctl restart amazon-cloudwatch-agent
```

---

## SNS Email Not Received

* Verify email subscription confirmation
* Check spam folder
* Verify SNS topic configuration

---

## MongoDB Service Down

Check MongoDB status:

```bash
sudo systemctl status mongod
```

View logs:

```bash
sudo tail -f /var/log/mongodb/mongod.log
```

---

# Conclusion

MongoDB monitoring and alerting on AWS is successfully configured using:

* CloudWatch Metrics
* CloudWatch Alarms
* SNS Notifications
* CloudWatch Agent

This setup helps proactively monitor database health and receive alerts during high resource usage or service failures.
