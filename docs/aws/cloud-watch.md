# Cloud Watch

Amazon CloudWatch is a monitoring and observability service built for DevOps engineers, developers, site reliability engineers (SREs), and IT managers. CloudWatch provides you with data and actionable insights to monitor your applications, respond to system-wide performance changes, optimize resource utilization, and get a unified view of operational health. CloudWatch collects monitoring and operational data in the form of logs, metrics, and events, providing you with a unified view of AWS resources, applications, and services that run on AWS and on-premises servers. You can use CloudWatch to detect anomalous behavior in your environments, set alarms, visualize logs and metrics side by side, take automated actions, troubleshoot issues, and discover insights to keep your applications
running smoothly. - [documentations](https://aws.amazon.com/cloudwatch/)

CloudWatch provide some key functionality

- Devops can measure performance and decide to scale up or down instances.
- Business and Data scientist can analysis traffic behavior and use data to back their decision
- Developers can monitor server health and make sure get notified when service is down

## Metric

AWS allow you to configure metric on all services they provide.

You can select a matric by how the metrics are accumulated. For Example, on EC2 there will be a groupping by "By Auto Scaling Group" and "Per-Instance Metrics".

### EC2

- CPUUtilization - Check if instance is overload, anlyse
- StatusCheck - monitor if application is up and running

### ELB

- RequestCount - number of request coming hitting the ELB - use by ASG to determine to scale up and down
- HTTPCode_ELB_5XX - check number of request that fail cause by server errors

### RDS / DynamoDb

- FreeStorageSpace - make sure storage have enough space
- Latency - how long does it take to read and write db

## CloudWatch Alarms

Alarm allow us to trigger actions based on metric.

### Alarm Status

- OK - meets the threshold, and will trigger action
- INSUFFICIENT_DATA - haven't have enough data, it can be still loading
- Alarm - doesn't meet the threshold, nothing will happen

### Peroid

- How often will the data be monitored.

### Triggerble Actions

- Auto Scaling
- SNS Notification
- Lamda
- EC2 actions

## Logs

- Allow log collections from platforms such as ECS2, Lamda, Route53, etc...
- Allow archive storage in S3
