---
title: "Test HA failover"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/installation/ha-test/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/installation/ha-test"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Test HA failover"
---
# Test HA failover

Learn how to perform HA failover testing for Redis Data Integration (RDI) to ensure high availability and reliability of your data integration setup.

## Setup

Ensure that RDI is up and running on both primary and secondary nodes.
Run the following command and verify and that each instance should show healthy and running `rdi-api` and `rdi-operator` pods.

```
kubectl -n rdi get pods

# Example output:
NAME                                   READY   STATUS      RESTARTS      AGE
collector-api-577d95bfd8-5wbg6         1/1     Running     0             12m
collector-source-95f45bcf7-vwn5l       1/1     Running     0             12m
fluentd-zq2lc                          1/1     Running     0             72m
logrotate-29530445-j729x               0/1     Completed   0             14m
logrotate-29530450-dprr2               0/1     Completed   0             9m40s
logrotate-29530455-mfmzw               0/1     Completed   0             4m40s
processor-f66655469-h7nw2              1/1     Running     0             12m
rdi-api-f75df6796-qwqjw                1/1     Running     0             72m
rdi-metrics-exporter-d57cdf8c8-wjzb5   1/1     Running     0             72m
rdi-operator-7f7f6c7dfd-5qmjd          1/1     Running     0             71m
rdi-reloader-77df5f7854-lwmvz          1/1     Running     0             71m
```

Identify the leader node - this is the one that has a running `collector-source` pod.

## Performing the HA Failover Testing

To perform HA, you can simulate a connection failure between the leader and the RDI database by blocking the network traffic. You can do this by running the following commands on the leader node:

Identify the RDI database IP (replace `<hostname>` with your own hostname):

```
dig +short <hostname>

# Example:
# dig +short my.redis.hostname.com

# Example output:
54.78.220.161
```

For each of the IPs returned by the above command, run the following command to block the traffic:

```
sudo iptables -I FORWARD -d <database_ip> -j DROP

# With the IP from the example above, the command would be:
sudo iptables -I FORWARD -d 54.78.220.161 -j DROP
```

The default configuration for the leader lock is 60 seconds, so it may take up to 2 minutes for the failover to occur.
Meanwhile you can follow the logs of the operator to see the failover process:

```
kubectl -n rdi logs rdi-operator-7f7f6c7dfd-5qmjd -f
```

In about 10 seconds you will start seeing log entries from the leader saying that it could not acquire the leadership.
When the leader lock expires, the second node will acquire the leadership and you will see log entries from the second node indicating that it has become the leader.

## Cleanup

To clean up after the test, remove the `iptables` rule that you added to block the traffic:

`sudo iptables -D FORWARD -d <databse_ip> -j DROP`

Use `sudo iptables -S | grep <database_ip>` to verify that the rule has been removed.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page