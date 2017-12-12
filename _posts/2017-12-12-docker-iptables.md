---
layout: post
title: Using iptables with docker to simulate network resource failure
---


## The Task

During some forensic failure analysis, where a particular Redis cluster was
suspected of causing memory issues, we wanted to simulate failure of redis, 
or a network glitch that would cause the link to redis to fail.

The service in question is a Spring Boot java service running in a Docker container on and EC2 server.

Redis is provisioned via Elasticache.

We looked at a few possibilities
  - modify a SecurityGroup to disable access to redis
    - this was not a great option, as the redis cluster is shared infrastructure, 
      other consumers may be using it.
  - use Route53 dns name to connect to redis, then redirect that name to an invalid address
    - this would likely not work - as we want to fail the service while the JVM is running.  In this scenario, the redis connection pool is already working.
  - use iptables to disable the traffic once the service is running.

This last one seems like a good option, fairly straightforward.

## The Problem

At first glance, it seems straightforward.  Just disable all traffic to the nodes
in the redis cluster using iptables.  A quick script helps us find the IP 
addresses of the nodes, and block them one at a time:

```
for ip in $(nslookup elasticache.somewhere.aws.company.net|grep Address|grep -v \#53|cut -d\  -f 2)
do
        echo "disabling access to $ip"
        iptables -A INPUT -s $ip -j DROP
        iptables -A OUTPUT -d $ip -j DROP
done
sudo iptables -S
```

This should block all traffic to/from the IPs of the redis cluster nodes.  
Obviously.

Wrong.

The problem is that Docker itself uses iptables to manage its own networking on 
the host. What we have done is blocked the host itself from contacting redis, 
not the docker containers runnin on that host.

## The Solution

The real answer was found here:

[https://serverfault.com/questions/704643/steps-for-limiting-outside-connections-to-docker-container-with-iptables](https://serverfault.com/questions/704643/steps-for-limiting-outside-connections-to-docker-container-with-iptables)


Disable all access to the redis port on the docker network: 
```
sudo iptables -I DOCKER-USER -i eth0 -p tcp -m tcp --sport 6379 -j DROP
```

To re-enable access to the redis port on the docker network:
```
sudo iptables -D DOCKER-USER -i eth0 -p tcp -m tcp --sport 6379 -j DROP
```
