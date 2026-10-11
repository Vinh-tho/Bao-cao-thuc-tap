---
title: "Week 5 Worklog"
date: 2026-08-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Objectives Week 5:

- Learn the roles of CloudWatch (monitoring) and CloudTrail (API auditing).
- Practice working with Log Groups, Metrics, Dashboards and Alarms.
- Set up email alerting through SNS.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Study CloudWatch (logs, metrics, alarms, dashboards) and CloudTrail (management events, trail, event history); distinguish monitoring from auditing. | 29/08/2026 | 30/08/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/> |
| Mon – Tue | Build CloudWatch Dashboards for EC2 and Lambda; inspect the Lambda log group created in week 4. | 31/08/2026 | 01/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed – Thu | Study Metrics and Alarm states (OK / In alarm / Insufficient data), thresholds and evaluation periods. | 02/09/2026 | 03/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | Create the SNS topic Eshop-Alert-Topic with an email subscription (confirm the subscription email); create an alarm on CPUUtilization ≥ 80% that notifies the topic. Enable a CloudTrail trail and inspect Event history. | 04/09/2026 | 04/09/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| SNS topic | Eshop-Alert-Topic, type Standard |
| SNS subscription | Protocol Email, endpoint = administrator email, status Confirmed after clicking the link |
| CloudWatch alarm | Eshop-High-CPU-Alarm; metric ECS → ClusterName, ServiceName → CPUUtilization (Eshop-ECS-Cluster / Eshop-Backend-Service); static threshold ≥ 80; action when In alarm → Eshop-Alert-Topic |
| CloudTrail trail | Eshop-Audit-Trail (quick trail create) writing to an auto-created S3 bucket aws-cloudtrail-logs-… |
| Event history | Entries observed: CreateTrail, CreateBucket, StartLogging with fields Event name, Event time, User name, Event source, Resource name |

### Knowledge gained:

- Metrics are identified by namespace and dimensions (for ECS: ClusterName and ServiceName); an alarm evaluates a metric against a threshold over a period.
- Alarm states: OK, In alarm and Insufficient data; notifications can be tied to state changes through SNS.
- SNS email subscriptions stay Pending until confirmed.
- CloudWatch answers 'how is the system doing' (logs, metrics); CloudTrail answers 'who did what' (API calls) and Event history keeps management events for 90 days.
- The alarm threshold (80%) is for human notification, while the Auto Scaling target (70%) added in week 8 acts earlier and automatically.

### Difficulties and solutions:

- Alerts did not arrive until the SNS subscription was confirmed. → Learned that email subscriptions stay Pending until the confirmation link is clicked.

### Achievements Week 5:

- Clearly distinguished CloudWatch (what the system is doing) from CloudTrail (who did what via API).
- Created dashboards and tracked logs and metrics.
- Configured an alarm with SNS email notification end to end.
- Prepared the monitoring pieces reused in the final project (Eshop-High-CPU-Alarm, Eshop-Audit-Trail).

### Deliverables and evidence:

- Alert e-mail received from AWS Notifications.
- CloudTrail Event history listing the API calls just performed.

### Plan for next week:

- Learn ALB and Auto Scaling with Launch Template and ASG.
