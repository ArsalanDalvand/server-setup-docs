---
title: "Enable Google Cloud Private Service Connect"
source: "https://redis.io/docs/latest/operate/rc/security/private-service-connect/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/private-service-connect"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Enable Google Cloud Private Service Connect"
---
# Enable Google Cloud Private Service Connect

Private Service Connect creates a private endpoint that allows secure connections to Redis Cloud databases without exposing your application VPC.

            Redis Cloud
          

[Private Service Connect](https://cloud.google.com/vpc/docs/private-service-connect) (PSC) creates a private endpoint that allows secure connections to Redis Cloud databases without exposing your application's [virtual private cloud](https://en.wikipedia.org/wiki/Virtual_private_cloud) (VPC).

This feature is only available for Redis Cloud Pro subscriptions hosted on Google Cloud.

## Considerations

You can use Private Service Connect as an alternative to [[operate/rc/security/vpc-peering|VPC peering]], or you can enable both for your subscription.

Compared to VPC peering, Private Service Connect:

Only exposes the private endpoint instead of the entire application VPC network.

Allows producer (Redis Cloud VPC) and consumer (application VPC) CIDR ranges to overlap.

Has slightly higher network latency than VPC peering due to load balancing requirements.

Note:
  Larger clusters are more likely to experience increased latency with Private Service Connect versus VPC peering.

Consider using VPC peering and Private Service Connect in parallel for the following situations:

When migrating from one connectivity solution to the other.

If different applications need to connect to the same database but have different latency or security requirements.

## Set up Private Service Connect

To set up Private Service Connect, you need to:

[Configure Private Service Connect](#configure-psc) in the Redis Cloud console.

[Create Private Service Connect endpoints](#create-endpoints) in the application VPC.

From the Redis Cloud console, review and [accept the Private Service Connect endpoint connection](#accept-psc).

### Configure PSC

First, configure Private Service Connect in Redis Cloud:

Select Subscriptions from the [Redis Cloud console](https://cloud.redis.io/) menu and then select your subscription from the list.

Select the Connectivity tab and then Private Service Connect.

Select the Create connection button:

[](/docs/latest/images/rc/button-subscription-connectivity-psc-create-connection.png)

Read the Latency and cost impact message and select Accept and continue:

[](/docs/latest/images/rc/button-subscription-connectivity-psc-accept-continue.png)

For Create connection, enter the following Endpoint details:

SettingÂ name
Description

Google Cloud project ID
Google Cloud project ID

VPC name
Name of the VPC that hosts your application

Subnet name
Name of your VPC's subnet of IP address ranges

Endpoint name
Prefix used to create PSC endpoints in the consumer application VPC, so endpoint names appear in Google Cloud as endpoint name prefix + endpoint number

Continue to the Add connections step:

[](/docs/latest/images/rc/button-subscription-continue.png)

Select either Bash Shell or PowerShell and then download or copy the provided `gcloud` script for later:

[](/docs/latest/images/rc/subscription-connectivity-psc-gcloud-script.png)

Select Continue to save this endpoint configuration:

[](/docs/latest/images/rc/button-subscription-continue.png)

### Create endpoints

Now that you have a pending Private Service Connect entry, you need to create the endpoints in your application's VPC:

If you have not already done so, [enable Cloud DNS](https://cloud.google.com/dns/docs/set-up-dns-records-domain-name) for your Google Cloud project.

Note:
  Since it can take some time for the DNS changes to become active, we recommend you wait 10 minutes before running the `gcloud` script in the next steps.

If you already have a copy of the `gcloud` script shown earlier during the Add connections step, you can continue to the next step.

Otherwise, return to your Redis Cloud subscription's Connectivity > Private Service Connect screen and select Complete setup for the pending endpoint:

[](/docs/latest/images/rc/button-subscription-connectivity-psc-complete-setup.png)

Download or copy the script.

Use the [`gcloud` CLI](https://cloud.google.com/sdk/gcloud) to run the script.

Warning:
  To ensure the `gcloud` script configures the endpoints correctly, do not make any changes to it.

The `gcloud` script creates a PSC endpoint and a DNS response policy in the consumer application VPC.

### Accept PSC connection

After the `gcloud` script finishes creating the Private Service Connect endpoint, you need to accept the connection in Redis Cloud:

In the Redis Cloud console, return to your subscription's Connectivity > Private Service Connect screen.

Find your pending endpoint connection in the list and select Accept:

[](/docs/latest/images/rc/button-subscription-connectivity-psc-accept.png)

### Connect to database

Once your Private Service Connect endpoint is active, you can connect your application to a database:

From your subscription's Connectivity > Private Service Connect tab, select the Connect button for the active endpoint:

[](/docs/latest/images/rc/button-connect.png)

Select a database from the list.

Copy the endpoint and use it in your application to connect to your database.

Note:
  Once your Private Service Connect endpoint is active, you must connect to the database from the Private Service Connect endpoint if you want to connect with Private Service connect. If you previously used the public or private endpoint to connect to your database, you must migrate any connections to the Private Service Connect endpoint.

## Deactivate Private Service Connect

To deactivate Private Service Connect for your subscription:

Select Subscriptions from the [Redis Cloud console](https://cloud.redis.io/) menu and then select your subscription from the list.

Select the Connectivity tab and then Private Service Connect.

For each endpoint:

Select the Delete PSC endpoint button:

[](/docs/latest/images/rc/icon-delete-teal.png)

Copy the provided `gcloud` script from the Remove endpoint dialog.

Run the `gcloud` script with [`gcloud` CLI](https://cloud.google.com/sdk/gcloud) to delete the endpoint and DNS response policy.

After you remove all endpoints, select the Actions button to see a list of available actions:

[](/docs/latest/images/rc/icon-psc-actions.png)

Select Remove service and then Confirm:

[](/docs/latest/images/rc/button-subscription-connectivity-psc-remove-service-confirm.png)

Once you remove all of your Private Service Connect endpoints and deactivate it, you must migrate any connections from a Private Service Connect endpoint to the public or private endpoint of your database.

## Limitations

Private Service Connect has the following limitations in Redis Cloud:

Although Redis Cloud supports using Private Service Connect with Enterprise clustering, you cannot use the [Cluster API](/docs/latest/operate/rc/databases/create-database/#oss-cluster-api) with Private Service Connect enabled.
The pre-handoff feature of [[develop/clients/sch#redis-cloud|Smart client handoffs]] is not currently supported with Private Service Connect, but relaxed timeouts are available and enabled by default.

## Continue learning with Redis University

See the [Redis Cloud security overview course](https://university.redis.io/course/0b1hkhejlitg3x) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page