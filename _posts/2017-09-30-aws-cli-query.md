---
layout: post
title: Search EC2 Instances by Tag:Name
---

Constantly navigating the console to find very specific bits of information can 
be a pain in the ass.  

Here is a CLI query to search by Tag:Name, and format the output nicely as a table.


```
aws --profile devops --region us-east-1 \
    ec2 describe-instances \
    --filters "Name=tag:Name,Values=*bemo*" 
    --query 'Reservations[*].Instances[*].{Name:Tags[?Key==`Name`].Value|[0],Type:InstanceType,State:State.Name,PrivateIp:PrivateIpAddress}' 
    --output table
---------------------------------------------------------------------------------
|                               DescribeInstances                               |
+-----------------------------------+------------------+----------+-------------+
|               Name                |    PrivateIp     |  State   |    Type     |
+-----------------------------------+------------------+----------+-------------+
|  ECS-NODE-dev-bemo                |  100.104.142.130 |  running |  t2.medium  |
|  dev-bemo-minder                  |  100.104.94.28   |  running |  t2.small   |
|  ECS-NODE-dev-bemo                |  100.104.88.13   |  running |  t2.medium  |
+-----------------------------------+------------------+----------+-------------+
```


