---
title: "Set up Redis for Bedrock"
source: "https://redis.io/docs/latest/integrate/amazon-bedrock/set-up-redis/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/amazon-bedrock/set-up-redis"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Set up Redis for Bedrock"
---
# Set up Redis for Bedrock

Shows how to set up your Redis database for Amazon Bedrock.

You need to set up your Redis Cloud database before you can set it as the vector database in Amazon Bedrock. To do this, you need to:

[Sign up for Redis Cloud and create a database](#sign-up-create-subscription)
[Enable Transport Layer Security (TLS) for the database and save the certificates](#get-certs)
[Store database credentials in AWS secrets manager](#store-secret)
[Create a vector index in your database](#create-vector-index) for Bedrock to use

After you set up the database, you can use the database information to set it as your knowledge base database when you [[integrate/amazon-bedrock/create-knowledge-base|create a knowledge base]].

## Sign up and create a database

To set up a Redis Cloud instance for Bedrock, you need to:

[Sign up for Redis Cloud](#sign-up) if you do not already have an account.
[Create a database](#create-sub) to use for your Bedrock knowledge base.

### Sign up for Redis Cloud using AWS Marketplace

Select the [Redis Cloud](https://aws.amazon.com/marketplace/pp/prodview-mwscixe4ujhkq?sr=0-1&ref_=beagle&applicationId=AWSMPContessa) AWS marketplace link from Bedrock to be taken to the Redis Cloud plan listing.

[](/docs/latest/images/rc/aws-marketplace-rc-flexible-plan.png)

Subscribe to Redis Cloud listing, locate the Set Up Your Account button, and then select it to begin mapping your Redis Cloud account with your AWS Marketplace account.

[](/docs/latest/images/rc/aws-marketplace-account-setup-button.png)

Sign in to the [Redis Cloud console](https://cloud.redis.io).

Select the Redis account to be mapped to your AWS Marketplace account and confirm that your payment method will change and that the connection cannot be undone.

[](/docs/latest/images/rc/aws-marketplace-map-account-dialog.png)

Use the Map account button to confirm your choice.

Once your Redis account is mapped to your AWS Marketplace account, a message appears in the upper, left corner of the account panel.

[](/docs/latest/images/rc/aws-marketplace-billing-badge.png)

In addition, AWS Marketplace is reported as the selected payment method.

### Create a database

In the [Redis Cloud console](https://cloud.redis.io/), select New database.

[](/docs/latest/images/rc/button-database-new.png)

When the New database page appears, select Pro to create a Pro plan.

[](/docs/latest/images/rc/create-database-subscription-pro-new.png)

After you select Pro, the Database settings section will appear. For this guide, continue with Easy create to get started faster.

[](/docs/latest/images/rc/create-pro-db-settings.png)

If you'd like to select all of the configuration options yourself, select Custom settings. See [Create a Redis Cloud Pro database](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/#custom-settings) for more details.

Redis will generate a database name for you. If you want to change it, you can do so in the Database name field.

[](/docs/latest/images/rc/pro-easy-create-vendor.png)

Select Amazon Web Services as the cloud vendor and select a region.

In the Database Sizing section:

[](/docs/latest/images/rc/pro-easy-create-size-throughput.png)

Select [High availability](/docs/latest/operate/rc/databases/configuration/high-availability/) if it is not already selected.

Set the Dataset size of your database based on the amount of data that Bedrock will pull from your Simple Storage Service (S3) [bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-buckets-s3.html). See [Find out the size of your S3 buckets](https://aws.amazon.com/blogs/storage/find-out-the-size-of-your-amazon-s3-buckets/) to find out how much knowledge base data is stored in your S3 bucket and pick the closest size, rounded up, from the table below.

Total Size of Documents in S3
Database size without replication
Database size with replication

10,000 kb
135 Mb
270 Mb

100,000 kb
1.35 Gb
2.7 Gb

1,000,000 kb
13.5 Gb
27 Gb

10,000,000 kb
135 Gb
270 Gb

For more information on sizing, see the [Bedrock integration blog post](https://redis.io/blog/amazon-bedrock-integration-with-redis-enterprise/#right-size-your-database-for-amazon-bedrock).

Select View all settings to review the database settings that we selected for you.

[](/docs/latest/images/rc/pro-easy-create-optimal-settings.png)

If you want to change these settings, select [Switch to custom settings](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/#custom-settings).

You will not need to enter a payment method, as it's automatically assigned to your AWS Marketplace account. Select Confirm & pay to create your new database.

[](/docs/latest/images/rc/button-create-db-confirm-pay.png)

Note that databases are created in the background.  While they are provisioning, you aren't allowed to make changes.  (The process generally takes 10-15 minutes.)

Use the Databases list to check the status of your subscription.  You will also receive an email when your database is ready to use.

## Enable TLS and get certificates

For your database to be fully secure, you must enable [[operate/rc/security/database-security/tls-ssl#enable-tls|Transport Layer Security (TLS)]] for your database with client authentication.

Select Databases from the [Redis Cloud console](https://cloud.redis.io/) menu and then select your database from the list.

From the database's Configuration screen, select the Edit button:

[](/docs/latest/images/rc/button-database-edit.png)

In the Security section, use the Transport layer security (TLS) toggle to enable TLS:

[](/docs/latest/images/rc/database-details-configuration-tab-security-tls-toggle.png)

Select Download server certificate to download the Redis Cloud certificate bundle `redis_ca.pem`:

[](/docs/latest/images/rc/button-database-config-security-server-ca-download.png)

Select the Mutual TLS (require client authentication) checkbox to require client authentication.

Select Add client certificate to add a certificate.

[](/docs/latest/images/rc/mtls-add-client-certificate.png)

Either provide an [X.509 client certificate](https://en.wikipedia.org/wiki/X.509) or chain in PEM format for your client or select Generate to create one:

[](/docs/latest/images/rc/database-details-configuration-tab-security-tls-client-auth-certificate.png)

If you generate your certificate from the Redis Cloud console, a Download certificate button will appear after it is generated. Select it to download the certificate.

[](/docs/latest/images/rc/mtls-download-certificate.png)

The download contains:

`redis-db-<database_id>.crt` â the certificate's public key.

`redis-db-<database_id>.key` â the certificate's private key.

Note:
  You must download the certificate using the button at this point.  After your changes have been applied, the full bundle of public and private keys will no longer be available for download.

If you provide a client certificate, you will see the certificate details before you save your changes.

[](/docs/latest/images/rc/mtls-certificate-details.png)

To apply your changes and enable TLS, select the Save database button:

[](/docs/latest/images/rc/button-database-save.png)

## Store database credentials in AWS secrets manager

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type Other type of secret with the following key/value fields:

`username`: Database username
`password`: Database password
`serverCertificate`: Contents of the [[operate/rc/security/database-security/tls-ssl#download-certificates|server certificate]] (`redis_ca.pem`)
`clientCertificate`: Contents of the client certificate (`redis_user.crt`)
`clientPrivateKey`: Contents of the client private key (`redis_user_private.key`)

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page.

## Create a vector index in your database

After your Redis Cloud database is set up, create a search index with a vector field using [FT.CREATE](/docs/latest/commands/ft.create/) as your knowledge base for Amazon Bedrock. You can accomplish this using Redis Insight or `redis-cli`.

### Redis Insight

[Redis Insight](/docs/latest/develop/tools/insight/) is a free Redis GUI that allows you to visualize and optimize your data in Redis.

To create your vector index in Redis Insight:

[Download and install Redis Insight](https://redis.io/insight/) if you don't have it already.

In the [Redis Cloud console](https://cloud.redis.io/), in your database's Configuration tab, select the Connect button next to your database to open the connection wizard.

[](/docs/latest/images/rc/button-connect.png#no-click)

In the connection wizard, under Redis Insight Desktop, select Public Endpoint. Select Open with Redis Insight to connect to the database with Redis Insight.

Select Use TLS. In the CA Certificate section, select Add new CA certificate. Give the certificate a name in the Name field, and enter the contents of `redis_ca.pem` into the Certificate field.

[](/docs/latest/images/rc/ri-bedrock-add-ca-cert.png)

Select Requires TLS Client Authentication. In the Client Certificate section, select Add new certificate. Give the certificate a name in the Name field. Enter the contents of `redis_user.crt` into the Certificate field, and the contents of `redis_user_private.key` into the Private Key field.

[](/docs/latest/images/rc/ri-bedrock-add-client-cert.png)

Select Add Redis Database to connect to the database.

Select your database alias to connect to your database. Select the Workbench icon to go to the workbench.

[](/docs/latest/images/rc/ri-bedrock-workbench.png)

Enter the [FT.CREATE](/docs/latest/commands/ft.create/) command to create an index.

```text
FT.CREATE <index_name>                    
    ON HASH                
    SCHEMA
        "<text_field>" TEXT
        "<metadata_field>" TEXT                   
        "<vector_field>"  VECTOR FLAT     
            6                          
            "TYPE" "FLOAT32"            
            "DIM" 1536                   
            "DISTANCE_METRIC" "COSINE"
```

Replace the following fields:

`<index_name>` with the vector index name
`<text_field>` with the text field name
`<metadata_field>` with the metadata field name
`<vector_field>` with the vector field name

Select Run to create the index.

[](/docs/latest/images/rc/ri-bedrock-run-button.png)

### redis-cli

The [[develop/tools/cli|`redis-cli`]] command-line utility lets you connect and run Redis commands directly from the command line. To use `redis-cli`, you can [install Redis](/docs/latest/operate/oss_and_stack/stack-with-enterprise/install/).

Public endpoint and port details are available from the Databases list or the database's Configuration screen. Select Connect to view how to connect to your database with `redis-cli`.

```sh
redis-cli -h <endpoint> -p <port> --tls --cacert redis_ca.pem \
    --cert redis_user.crt --key redis_user_private.key
```

After you are connected with `redis-cli`, create an index using [FT.CREATE](/docs/latest/commands/ft.create/).

```text
FT.CREATE <index_name>                    
    ON HASH                
    SCHEMA
        "<text_field>" TEXT
        "<metadata_field>" TEXT                   
        "<vector_field>"  VECTOR FLAT     
            6                          
            "TYPE" "FLOAT32"            
            "DIM" 1536                   
            "DISTANCE_METRIC" "COSINE"
```

Replace the following fields:

`<index_name>` with the vector index name
`<text_field>` with the text field name
`<metadata_field>` with the metadata field name
`<vector_field>` with the vector field name

## Next steps

After your Redis database is set up, you can use it to [[integrate/amazon-bedrock/create-knowledge-base|create a knowledge base]] in Amazon Bedrock.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page