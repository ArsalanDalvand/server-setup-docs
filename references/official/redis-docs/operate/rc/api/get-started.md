---
title: "Get started with the REST API"
source: "https://redis.io/docs/latest/operate/rc/api/get-started/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/api/get-started"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Get started with the REST API"
---
# Get started with the REST API

Describes how Redis Cloud REST API uses keys to authenticate and authorize access.

            Redis Cloud
          

To use the Redis Cloud REST API, you need to:

Enable the API
Create an account key
Create a user key
Collect endpoint details

To use the keys to authenticate and authorize your request, include the keys with the request headers:

Key name
HTTPÂ headerÂ name
Description

AccountÂ key
`x-api-key`
Account-level key assigned to all users of an account

User key
`x-api-secret-key`
Personal key associated with a specific user and possibly limited to certain IP ranges

## Enable the API

The API is disabled on all accounts by default. You must [[operate/rc/api/get-started/enable-the-api|enable the API]] before you can use it.

## Account key

The account key identifies your specific account when you perform an API request.  This is the account responsible for your subscription.

Note:
  An account key is an account-level secret. Do not share this key with anyone not authorized to use the account.

You create the account key once when enabling API access.

If you need to change or delete your account key, please [contact support](https://redislabs.com/company/support/).

## User key

The user key is a personal key that belongs to a specific user having the Owner, Viewer, Billing admin, or Logs viewer role.  User keys are assigned to users when they're created.  Keys can belong to only one user; however, a user may have multiple keys.

You can view keys or copy their values only during the [[operate/rc/api/get-started/manage-api-keys|creation process]].

Note:
  User keys are personal secrets. Do not share them.

Individual owners can [[operate/rc/api/get-started/manage-api-keys|generate multiple user keys]]
for themselves, for separate apps, or for other owners, viewers, or log viewers within the same account.

Use key names to uniquely associate specific API requests to individual users or apps.

Doing so lets you [audit API requests](/docs/latest/operate/rc/api/examples/audit-system-logs/) using the system log, which tracks the key used to authenticate each request.

## Authentication using API keys

Every API request must use the account key and a user key to authenticate.

The keys are provided as [[operate/rc/api/get-started/use-rest-api#use-the-curl-http-client|HTTP request headers]], shown earlier.

## Authenticate a request

An API request successfully authenticates when:

The account and user keys are valid and properly defined in the HTTP request headers.

The user key is associated with the same account as the account key.

The request originates from a valid source IP address, as defined in a [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) allow list associated with the user key.

This requirement applies when you've [[operate/rc/api/get-started/manage-api-keys#manage-cidr-allow-list|defined a CIDR allow list]] for the secret key.

## More info

To learn more, see:

[[operate/rc/api/get-started/manage-api-keys|Manage API keys]]
[[operate/rc/api/get-started/use-rest-api|Use the API]]
[[operate/rc/api/api-reference|Full API reference]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page