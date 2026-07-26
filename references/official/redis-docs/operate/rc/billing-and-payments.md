---
title: "Billing & payments"
source: "https://redis.io/docs/latest/operate/rc/billing-and-payments/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/billing-and-payments"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Billing & payments"
---
# Billing & payments

Describes how to view billing transactions and manage payment methods for Redis Cloud subscriptions.

            Redis Cloud
          

The Billing & Payments screen:

Shows recent transactions for your account
Helps you manage your payment methods
Applies coupon credits to your account

[](/docs/latest/images/rc/billing-billing-history-tab.png)

The following tabs are available:

The Billing History tab displays recent charges and payments.  Each transaction includes the following details:

Detail
Description

Date
Date the transaction was recorded

Description
Description of the transaction

Reference
Reference number

Amount
Transaction amount

Billing details may vary between regions.

You can [download invoices](#download-invoice) on this tab.

A Pay Now button appears in selected regions.

[](/docs/latest/images/rc/billing-payments-pay-now.png)

Select this button to pay your invoice.

The Payment Methods tab lists your current payment methods.  You can add a new payment method, associate different payment methods with specific subscriptions, and remove payment methods.

[](/docs/latest/images/rc/billing-payment-method-tab.png)

Select Add credit card to enter new credit card details, or Add marketplace account to add a [Google Cloud Marketplace](/docs/latest/operate/rc/cloud-integrations/gcp-marketplace/) or [AWS Marketplace](/docs/latest/operate/rc/cloud-integrations/aws-marketplace/) account.

The Credits tab shows coupon credits that have been applied to your account, if any.

[](/docs/latest/images/rc/billing-payments-credits-tab.png)

Detail
Description

Code
Coupon code

Coupon Amount
Amount credited to your account

Current Balance
Amount left

Date added
Date applied to your account

Expiration Date
Date the amount expires

You can [apply a coupon](#apply-coupon) on this tab.

## Download invoice

To download an invoice:

In the [Redis Cloud console](https://cloud.redis.io/), select Billing & Payments.

From the Billing History tab, locate and select the invoice.

Select the Download invoice icon displayed to the right of the invoice amount.

[](/docs/latest/images/rc/icon-billing-download.png)

The invoice is downloaded as an Acrobat PDF file.  Use your browser's download features to manage the file.

## Add credit card

To add a new credit card:

In the [Redis Cloud console](https://cloud.redis.io/), select Billing & Payments > Payment Methods.

Select Add credit card and add the credit card details.

If you have not already added a business address to your account, you must enter one. Redis uses your business address for communication, invoicing, and tax purposes.

[](/docs/latest/images/rc/billing-add-first-card.png)

If you already have a business address, you don't need to enter one here. See [[operate/rc/accounts/account-settings|Account settings]] to learn how to update your business address.

[](/docs/latest/images/rc/billing-add-credit-card.png)

If the card's billing address is the same as your account's business address, select Same as business address to fill the billing address with your business address details.

Select the Add Credit Card button to save your changes.

[](/docs/latest/images/rc/button-billing-save-card.png)

## Add marketplace account

If you have a [Google Cloud Marketplace](/docs/latest/operate/rc/cloud-integrations/gcp-marketplace/) or [AWS Marketplace](/docs/latest/operate/rc/cloud-integrations/aws-marketplace/) account already associated with another Redis Account that you own, you can add it to your current account.

To do this:

In the [Redis Cloud console](https://cloud.redis.io/), sign in to the account you would like to associate with your marketplace account.

Select Billing & Payments > Payment Methods.

Select Add Marketplace account.

[](/docs/latest/images/rc/add-marketplace-account.png)

Select the marketplace account you want to connect and confirm that it will pay for your Redis Cloud resources going forward.

Select Connect account.

[](/docs/latest/images/rc/button-connect-account.png)

Your marketplace account is now connected to your Redis Cloud account. If you would like to disconnect your marketplace account from your Redis Cloud account, select Disconnect.

## Apply coupon

Coupons apply credits to your Redis Cloud account.  To redeem a coupon:

In the [Redis Cloud console](https://cloud.redis.io/), select Billing & Payments and then select the Credits tab.

Enter the coupon code and then select the Apply button.

[](/docs/latest/images/rc/button-billing-payments-apply.png)

The value of the coupon is applied to your account when accepted.

For help, contact [Support](https://redis.io/support/).

Note:

Generally, charges are non-refundable.

For any special circumstances that may warrant a refund, please contact [Support](https://redis.io/support/) and be sure to provide detail about the reasons for the refund request.

## Download cost report

Select the Cost report button to download a report of all shard costs in CSV format.

[](/docs/latest/images/rc/button-cost-report-download.png)

The report gives a breakdown of all your Redis billing unit costs, per subscription and database, by month. Subscriptions under the minimum subscription price are broken down into actual billing unit prices and the difference from minimum price.

See [[operate/rc/billing-and-payments/cost-report|Cost report]] and [How to download and visualize the cost report](https://support.redislabs.com/hc/en-us/articles/30042563097874-How-to-Download-and-Visualize-Redis-Cloud-Cost-Report) for more information.

## Troubleshoot

See [Billing](https://support.redislabs.com/hc/en-us/categories/26174537325842-Billing) on the Redis knowledge base for help troubleshooting billing issues:

[Resolving payment failures in Redis Cloud](https://support.redislabs.com/hc/en-us/articles/26266042933522-Resolving-Payment-Failures-in-Redis-Cloud)
[Credit card retry behavior and troubleshooting](https://support.redislabs.com/hc/en-us/articles/27697187187986-Credit-Card-Retry-Behavior-and-Troubleshooting)
[Access invoices and payment links](https://support.redislabs.com/hc/en-us/articles/26265945230226-Access-Invoices-and-Payment-Links-in-the-Portal)
[How to find payment information](https://support.redislabs.com/hc/en-us/articles/26266104144914-How-to-Find-Payment-Information)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page