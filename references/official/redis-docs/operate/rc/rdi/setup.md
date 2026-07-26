---
title: "Prepare source database"
source: "https://redis.io/docs/latest/operate/rc/rdi/setup/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/rdi/setup"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Prepare source database"
---
# Prepare source database

Prepare your source database, network setup, and database credentials for Data integration.

            Redis Cloud
          

## Prepare source database

Before using the pipeline, you must first prepare your source database to use the Debezium connector for change data capture (CDC). See [[operate/rc/rdi#prerequisites|Prerequisites]] to find a list of supported source databases and database versions.

See [Prepare source databases](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/) to find steps for your database type:

[MongoDB Atlas](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/mongodb/)
[Snowflake](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/snowflake/)
Hosted on an AWS EC2 instance:

[MySQL and mariaDB](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/my-sql-mariadb/)
[Oracle](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/oracle/)
[SQL Server](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/sql-server/)
[PostgreSQL](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/postgresql/)

Hosted on AWS RDS or AWS Aurora:

[AWS Aurora PostgreSQL and AWS RDS PostgreSQL](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/aws-aurora-rds/aws-aur-pgsql/)
[AWS Aurora MySQL and AWS RDS MySQL](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/aws-aurora-rds/aws-aur-mysql/)
[AWS RDS SQL Server](/docs/latest/integrate/redis-data-integration/data-pipelines/prepare-dbs/aws-aurora-rds/aws-rds-sqlserver/)

See the [[integrate/redis-data-integration/architecture#overview|RDI architecture overview]] for more information about CDC.

## Get cluster account ID

Before you can set up your source connectivity and secrets, you need the AWS Account ID for your Redis Cloud cluster so that you can give it access to your connectivity and secrets.

On the [Redis Cloud console](https://cloud.redis.io/), go to your target database and select the Data Integration tab.

Select Add pipeline.

    
    	
    
    [](/docs/latest/images/rc/rdi/rdi-workspace-add-pipeline.png)

Select your source database type. The following database types are supported:

MySQL
mariaDB
Oracle
SQL Server
PostgreSQL
MongoDB
Snowflake

    
    	
    
    [](/docs/latest/images/rc/rdi/rdi-select-source-db.png)

Enter a name for your source database in the Source name field. This is a name for the source database that will appear on Redis Cloud.

Select Continue to source to move to the Source configuration step.

[](/docs/latest/images/rc/rdi/rdi-continue-to-source-button.png)

Under Source connectivity, save the provided ARN and extract the AWS account ID for the account associated with your Redis Cloud cluster from it.

[](/docs/latest/images/rc/rdi/rdi-setup-connectivity-arn.png)

The AWS account ID is the string of numbers after `arn:aws:iam::` in the ARN. For example, if the ARN is `arn:aws:iam::123456789012:role/redis-data-pipeline`, the AWS account ID is `123456789012`.

If your source database is accessible via the public endpoint and you want to use public connectivity for your data pipeline, select Public endpoint and save the Redis Cloud outbound IP address to add to your source database's allow list.

Select Save & exit to exit pipeline setup. You'll come back here when you [[operate/rc/rdi/define|define your source connection and data pipeline]].

## Set up AWS Private Link connectivity

Note:
  If your source database is accessible via a public endpoint and you want to use public connectivity for your data pipeline, proceed to [Share source database credentials](#share-source-database-credentials).

If your source database is not accessible via a public endpoint, you need to set up an endpoint service through AWS PrivateLink to be able to connect to it.

The following diagrams show the network setup for the different database setups:

Database hosted on an AWS EC2 instance:

[](/docs/latest/images/rc/rdi/rdi-setup-diagram-ec2.png)

Database hosted on AWS RDS or AWS Aurora:

[](/docs/latest/images/rc/rdi/rdi-setup-diagram-aurora.png)

Select the steps for your database setup.

        EC2 instance
      

        AWS RDS or Aurora
      

To set up PrivateLink for a database hosted on an EC2 instance:

[Create a network load balancer](#create-network-load-balancer-ec2) that will route incoming HTTP requests to your database.
[Create an endpoint service](#create-endpoint-service-ec2) through AWS PrivateLink.

### Create network load balancer

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Compute > EC2. [Create a network load balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html#configure-load-balancer) with the following settings:

In Basic configuration:

Scheme: Select Internal.
Load balancer IP address type: Select IPv4.

In Network mapping, select the VPC and availability zone associated with your source database.

In Security groups, select the security group associated with your source database, or another security group that allows traffic from PrivateLink and allows traffic to the database.

In Listeners and routing:

Select Create target group to [create a target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-target-group.html) with the following settings:

In Specify group details:

Target type: Select Instances.
Protocol : Port: Select TCP, and then enter the port number where your database is exposed.
The IP address type and VPC should be selected already and match the VPC you selected earlier.

In Register targets, select the EC2 instance that runs your source database, enter the port, and select Include as pending below. Then, select Create target group to create your target group. Return to Listeners and routing in the Network Load Balancer setup.

Set the following Listener properties:

Protocol: Select TCP.
Port: Enter your source database's port.
Default action: Select the target group you created in the previous step.

Review the network load balancer settings, and then select Create load balancer to continue.

After the network load balancer is active, select Security.

If you selected the same security group as your source database, you must not enforce security group rules on PrivateLink traffic. Select Edit and then deselect Enforce inbound rules on PrivateLink traffic, and then select Save changes.

Select the security group ID to open the Security group settings.

Select Edit inbound rules, then Add rule to add a rule with the following settings:

Type: Select HTTP.
Source: Select Anywhere - IPv4.
Select Save rules to save your changes.

Select Actions > Edit Load Balancer Attributes.

Under Load balancer targets selection policy select Enable cross-zone load balancing.
Click the Save Changes button.

### Create endpoint service

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Networking & Content Delivery > VPC. There, select PrivateLink and Lattice > Endpoint services. [Create an endpoint service](https://docs.aws.amazon.com/vpc/latest/privatelink/create-endpoint-service.html) with the following settings:

In Available load balancers, select the [network load balancer](#create-network-load-balancer-ec2) you created.
In Additional settings, choose the following settings:

Require acceptance for endpoint: Select Acceptance required.
Supported IP address types: Select IPv4.

Select Create to create the endpoint service.

After you create the endpoint service, you need to add Redis Cloud as an Allowed Principal on your [endpoint service VPC permissions](https://docs.aws.amazon.com/vpc/latest/privatelink/configure-endpoint-service.html#add-remove-permissions).

In the Redis Cloud Console, copy the Amazon Resource Name (ARN) provided in the Setup connectivity section.
Return to the endpoint service list on the [Amazon VPC console](https://console.aws.amazon.com/vpc/). Select the endpoint service you just created.
Navigate to Allow principals tab.
Add the Redis Cloud ARN you copied and choose Allow principals.
Save the service name for later.

For more details on AWS PrivateLink, see [Share your services through AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/privatelink-share-your-services.html).

To set up PrivateLink for a database hosted on AWS RDS or AWS Aurora:

To connect to your RDS or Aurora database, we recommend using a Lambda function approach. This provides a reliable and secure connection method for all database types.

[Create a network load balancer](#create-network-load-balancer-rds) that will route incoming requests to your database.
[Create an endpoint service](#create-endpoint-service-rds) through AWS PrivateLink.
[Set up Lambda function connectivity](#setup-lambda-function) to route requests to your database.

Note:
  If you have specific requirements that necessitate using RDS Proxy instead of the recommended Lambda function approach, see the [[operate/rc/rdi/rds-proxy|RDS Proxy setup guide]]. Note that RDS Proxy is not recommended and does not work with PostgreSQL.

### Create network load balancer

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Compute > EC2. [Create a network load balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html#configure-load-balancer) with the following settings:

In Basic configuration:

Scheme: Select Internal.
Load balancer IP address type: Select IPv4.

In Network mapping, select the VPC and availability zone associated with your source database.

In Security groups, select the security group associated with your source database, or another security group that allows traffic from PrivateLink and allows traffic to the database.

In Listeners and routing:

Select Create target group to [create a target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-target-group.html) with the following settings:

In Specify group details:

Target type: Select IP Addresses.
Protocol : Port: Select TCP, and then enter the port number where your database is exposed.
The IP address type and VPC should be selected already and match the VPC you selected earlier.

In Register targets, enter the static IP address of your database, enter the port, and select Include as pending below. Then, select Create target group to create your target group. Return to Listeners and routing in the Network Load Balancer setup.

To get the static IP address of your database, run the following command on an EC2 instance in the same VPC as the database:

```sh
$ nslookup <database-endpoint>
```

Replace `<database-endpoint>` with the endpoint of your RDS or Aurora database.

Set the following Listener properties:

Protocol: Select TCP.
Port: Enter your source database's port.
Default action: Select the target group you created in the previous step.

Review the network load balancer settings, and then select Create load balancer to continue.

After the network load balancer is active, select Security.

If you selected the same security group as your source database, you must not enforce security group rules on PrivateLink traffic. Select Edit and then deselect Enforce inbound rules on PrivateLink traffic, and then select Save changes.

Select the security group ID to open the Security group settings.

Select Edit inbound rules, then Add rule to add a rule with the following settings:

Type: Select HTTP.
Source: Select Anywhere - IPv4.
Select Save rules to save your changes.

Select Actions > Edit Load Balancer Attributes.

Under Load balancer targets selection policy select Enable cross-zone load balancing.
Click the Save Changes button.

### Create endpoint service

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Networking & Content Delivery > VPC. There, select PrivateLink and Lattice > Endpoint services. [Create an endpoint service](https://docs.aws.amazon.com/vpc/latest/privatelink/create-endpoint-service.html) with the following settings:

In Available load balancers, select the [network load balancer](#create-network-load-balancer-rds) you created.
In Additional settings, choose the following settings:

Require acceptance for endpoint: Select Acceptance required.
Supported IP address types: Select IPv4.

Select Create to create the endpoint service.

After you create the endpoint service, you need to add Redis Cloud as an Allowed Principal on your [endpoint service VPC permissions](https://docs.aws.amazon.com/vpc/latest/privatelink/configure-endpoint-service.html#add-remove-permissions).

In the Redis Cloud Console, copy the Amazon Resource Name (ARN) provided in the Setup connectivity section.
Return to the endpoint service list on the [Amazon VPC console](https://console.aws.amazon.com/vpc/). Select the endpoint service you just created.
Navigate to Allow principals tab.
Add the Redis Cloud ARN you copied and choose Allow principals.
Save the service name for later.

For more details on AWS PrivateLink, see [Share your services through AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/privatelink-share-your-services.html).

### Set up Lambda function connectivity

Note:
  Setting up the Lambda function is optional but recommended for production environments. The Lambda function provides automatic failover handling and a more robust connection to your RDS or Aurora database.

The Lambda function monitors RDS failover events and automatically updates the NLB Target Group to point to the new primary instance's IP address. This ensures RDI reconnects automatically after a failover.

#### Option 1: Use the Redis Terraform module

Redis provides a ready-to-use Terraform module that automates the Lambda function deployment. This is the recommended approach.

##### Prerequisites

[Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) >= 1.5.7
[AWS CLI](https://aws.amazon.com/cli/) configured with credentials
The ARNs from the Network Load Balancer and Endpoint Service you created in the previous steps

##### Required variables

Before deploying the Lambda module, gather the following information:

Variable
Description
Where to find it

`identifier`
A unique name for the Lambda resources
Choose a descriptive name (e.g., `rdi-failover-handler`)

`db_endpoint`
Your RDS cluster or instance endpoint
AWS Console â RDS â Your database â Connectivity

`db_port`
Your database port
AWS Console â RDS â Your database â Connectivity (default: `5432` for PostgreSQL, `3306` for MySQL, `1433` for SQL Server)

`elb_tg_arn`
The NLB Target Group ARN
AWS Console â EC2 â Target Groups â Your target group

`rds_arn`
The RDS cluster or instance ARN
AWS Console â RDS â Your database â Configuration

`rds_cluster_identifier`
The RDS cluster identifier
AWS Console â RDS â Your cluster name

##### Deploy the Lambda module

Clone the Redis cloud automation repository:

```bash
git clone https://github.com/redis/rdi-cloud-automation.git
cd rdi-cloud-automation/modules/aws-rds-lambda
```

Create a `terraform.tfvars` file with your configuration:

```hcl
identifier             = "rdi-failover-handler"
db_endpoint            = "your-cluster.cluster-xxxxxxxxx.us-east-1.rds.amazonaws.com"
db_port                = 5432
elb_tg_arn             = "arn:aws:elasticloadbalancing:us-east-1:123456789012:targetgroup/your-tg/xxxxxxxxx"
rds_arn                = "arn:aws:rds:us-east-1:123456789012:cluster:your-cluster"
rds_cluster_identifier = "your-cluster"
```

Initialize and apply Terraform:

```bash
terraform init
terraform apply
```

##### How the Lambda function works

The deployed Lambda function:

Monitors RDS events: Subscribes to RDS failover events via SNS
Detects failover: When a failover occurs, RDS triggers an SNS notification
Resolves new IP: The Lambda function queries DNS to get the new primary's IP address
Updates NLB target: Automatically updates the NLB Target Group with the new IP

This process typically completes within 30-60 seconds for Aurora, or 60-120 seconds for standard RDS.

##### Verify the deployment

After deployment, verify the Lambda function is configured correctly:

Check the Lambda function in AWS Console â Lambda â Functions
Verify the environment variables are set correctly:

`Cluster_EndPoint`: Your RDS endpoint
`RDS_Port`: Your database port
`NLB_TG_ARN`: Your NLB Target Group ARN

Check the SNS subscription in AWS Console â SNS â Subscriptions

#### Option 2: Full infrastructure deployment

For new deployments, Redis provides a complete Terraform example that deploys the entire infrastructure including the RDS database, NLB, PrivateLink, and Lambda function.

See the [AWS RDS PrivateLink Failover Example](https://github.com/redis/rdi-cloud-automation/tree/main/examples/aws-rds-privatelink-failover) for:

Multi-engine support (PostgreSQL, MySQL, SQL Server)
Automatic CDC user creation
Complete VPC and networking setup
Lambda-based failover handling

#### Option 3: Manual Lambda setup

For custom implementations, refer to the AWS documentation:
[Access Amazon RDS across VPCs using AWS PrivateLink and Network Load Balancer](https://aws.amazon.com/blogs/database/access-amazon-rds-across-vpcs-using-aws-privatelink-and-network-load-balancer/)

## Share source database credentials

You need to share your source database credentials and certificates in an Amazon secret with Redis Cloud so that the pipeline can connect to your database.

To do this, you need to:

[Create an encryption key](#create-encryption-key) using AWS Key Management Service with the right permissions.
[Create secrets](#create-database-credentials-secrets) containing the source database credentials encrypted using that key.

### Create encryption key

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Key Management Service. [Create an encryption key](https://docs.aws.amazon.com/kms/latest/developerguide/create-symmetric-cmk.html) with the following settings:

In Step 1 - Configure key:

Key type: Select Symmetric.
Key usage: Select Encrypt and decrypt.
Under Advanced options, set the following:

Key material origin: Select KMS - recommended.
Regionality: Select Single-Region key.

In Step 2 - Add labels, add an alias and description for the key.
In Step 3 - Define key administrative permissions, under Key deletion, select Allow key administrators to delete this key.
In Step 4 - Define key usage permissions, under Other AWS accounts, select Add another AWS account. Enter the AWS account ID for the Redis Cloud cluster that you saved earlier.

Review the key policy and key settings, and then select Finish to create the key.

### Create database credentials secrets

To let Redis Cloud access your source database, you need to create AWS secrets for the source database's credentials and certificates.

The required secrets depend on your source database's security configuration. The following table shows the required secrets for each configuration:

Security configuration
Required secrets

Username and password only
Credentials secret (username and password for the RDI pipeline user)

Username and key pair only (Snowflake source databases only)
Credentials secret (username for the RDI pipeline user)Private key secret

TLS connection
Credentials secret (username and password for the RDI pipeline user)CA Certificate secret (server certificate)

mTLS connection
Credentials secret (username and password for the RDI pipeline user)CA Certificate secret (server certificate)Client certificate secretClient key secret

mTLS connection with client key passphrase
Credentials secret (username and password for the RDI pipeline user)CA Certificate secret (server certificate)Client certificate secretClient key secretClient key passphrase secret

Note:

When creating secrets for TLS or mTLS, ensure that all certificates and keys are in `PEM` format. The only exception to this is that for PostgreSQL, the private key `SOURCE_DB_KEY` secret must be in `DER` format. If you have a key in `PEM` format, you must convert it to `DER` before creating the `SOURCE_DB_KEY` secret using the command:

```bash
openssl pkcs8 -topk8 -inform PEM -outform DER \
    -in /path/to/myclient.pem \
    -out /path/to/myclient.pk8 -nocrypt
```

This command assumes that the private key is not encrypted. See the [`openssl` documentation](https://docs.openssl.org/master/) to learn how to convert an encrypted private key.

Select a tab to learn how to create the required secret.

        Credentials secret
      

        CA Certificate secret
      

        Client certificate secret
      

        Client key secret
      

        Client key passphrase secret
      

        Private key secret
      

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type Other type of secret with the following settings:

Key/value pairs: Enter the following key/value pairs.

`username`: Database username for the RDI pipeline user
`password`: Database password for the RDI pipeline user

Note:
  Snowflake source databases that use user/key pair authentication should only enter the `username` for the database.

Encryption key: Select the [encryption key](#create-encryption-key) you created earlier.

Resource permissions: Add the following permissions to your secret to allow the Redis data pipeline to access your secret. Replace `<AWS ACCOUNT ID>` with the AWS account ID for the Redis Cloud cluster that you saved earlier.

```json
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Sid" : "RedisDataIntegrationRoleAccess",
        "Effect" : "Allow",
        "Principal" : "*",
        "Action" : [ "secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret" ],
        "Resource" : "*",
        "Condition" : {
            "StringLike" : {
                "aws:PrincipalArn" : "arn:aws:iam::<AWS ACCOUNT ID>:role/redis-data-pipeline-secrets-role"
            }
        }
    } ]
}
```

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. Save the secret ARN to use when you [[operate/rc/rdi/define|define your source database]].

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type Other type of secret with the following settings:

Key/value pairs: Select Plaintext and enter the server certificate.

Encryption key: Select the [encryption key](#create-encryption-key) you created earlier.

Resource permissions: Add the following permissions to your secret to allow the Redis data pipeline to access your secret. Replace `<AWS ACCOUNT ID>` with the AWS account ID for the Redis Cloud cluster that you saved earlier.

```json
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Sid" : "RedisDataIntegrationRoleAccess",
        "Effect" : "Allow",
        "Principal" : "*",
        "Action" : [ "secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret" ],
        "Resource" : "*",
        "Condition" : {
            "StringLike" : {
                "aws:PrincipalArn" : "arn:aws:iam::<AWS ACCOUNT ID>:role/redis-data-pipeline-secrets-role"
            }
        }
    } ]
}
```

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. Save the secret ARN to use when you [[operate/rc/rdi/define|define your source database]].

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type Other type of secret with the following settings:

Key/value pairs: Select Plaintext and enter the client certificate.

Encryption key: Select the [encryption key](#create-encryption-key) you created earlier.

Resource permissions: Add the following permissions to your secret to allow the Redis data pipeline to access your secret. Replace `<AWS ACCOUNT ID>` with the AWS account ID for the Redis Cloud cluster that you saved earlier.

```json
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Sid" : "RedisDataIntegrationRoleAccess",
        "Effect" : "Allow",
        "Principal" : "*",
        "Action" : [ "secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret" ],
        "Resource" : "*",
        "Condition" : {
            "StringLike" : {
                "aws:PrincipalArn" : "arn:aws:iam::<AWS ACCOUNT ID>:role/redis-data-pipeline-secrets-role"
            }
        }
    } ]
}
```

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. Save the secret ARN to use when you [[operate/rc/rdi/define|define your source database]].

Use the [AWS CLI create-secret command](https://docs.aws.amazon.com/cli/latest/reference/secretsmanager/create-secret.html) or the [AWS CreateSecret API endpoint](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html) to create a binary secret containing the client key.

For example, using the AWS CLI, run the following command:

```sh
aws secretsmanager create-secret \
    --name <secret-name> \
    --secret-binary fileb://<path-to-client-key> \
    --kms-key-id <encryption-key-arn>
```

Where:

`<secret-name>` - Name of the secret
`<path-to-client-key>` - Path to the client key file
`<encryption-key-arn>` - ARN of the [encryption key](#create-encryption-key) you created earlier

After you create the secret, you need to add permissions to allow the data pipeline to access it.

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. Select the private key secret you just created and then select Edit permissions.

Add the following permissions to your secret. Replace `<AWS ACCOUNT ID>` with the AWS account ID for the Redis Cloud cluster that you saved earlier.

```json
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Sid" : "RedisDataIntegrationRoleAccess",
        "Effect" : "Allow",
        "Principal" : "*",
        "Action" : [ "secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret" ],
        "Resource" : "*",
        "Condition" : {
            "StringLike" : {
                "aws:PrincipalArn" : "arn:aws:iam::<AWS ACCOUNT ID>:role/redis-data-pipeline-secrets-role"
            }
        }
    } ]
}
```

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. Save the secret ARN to use when you [[operate/rc/rdi/define|define your source database]].

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type Other type of secret with the following settings:

Key/value pairs: Select Plaintext and enter the client key passphrase.

Encryption key: Select the [encryption key](#create-encryption-key) you created earlier.

Resource permissions: Add the following permissions to your secret to allow the Redis data pipeline to access your secret. Replace `<AWS ACCOUNT ID>` with the AWS account ID for the Redis Cloud cluster that you saved earlier.

```json
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Sid" : "RedisDataIntegrationRoleAccess",
        "Effect" : "Allow",
        "Principal" : "*",
        "Action" : [ "secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret" ],
        "Resource" : "*",
        "Condition" : {
            "StringLike" : {
                "aws:PrincipalArn" : "arn:aws:iam::<AWS ACCOUNT ID>:role/redis-data-pipeline-secrets-role"
            }
        }
    } ]
}
```

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. Save the secret ARN to use when you [[operate/rc/rdi/define|define your source database]].

This secret is required for Snowflake source databases that use key-pair authentication.

In the [AWS Management Console](https://console.aws.amazon.com/), use the Services menu to locate and select Security, Identity, and Compliance > Secrets Manager. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type Other type of secret with the following settings:

Key/value pairs: Select Plaintext and enter the private key in plain text PEM format.

Encryption key: Select the [encryption key](#create-encryption-key) you created earlier.

Resource permissions: Add the following permissions to your secret to allow the Redis data pipeline to access your secret. Replace `<AWS ACCOUNT ID>` with the AWS account ID for the Redis Cloud cluster that you saved earlier.

```json
{
    "Version" : "2012-10-17",
    "Statement" : [ {
        "Sid" : "RedisDataIntegrationRoleAccess",
        "Effect" : "Allow",
        "Principal" : "*",
        "Action" : [ "secretsmanager:GetSecretValue", "secretsmanager:DescribeSecret" ],
        "Resource" : "*",
        "Condition" : {
            "StringLike" : {
                "aws:PrincipalArn" : "arn:aws:iam::<AWS ACCOUNT ID>:role/redis-data-pipeline-secrets-role"
            }
        }
    } ]
}
```

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. Save the secret ARN to use when you [[operate/rc/rdi/define|define your source database]].

## Next steps

After you have set up your source database and prepared connectivity and credentials, select Define source database to [[operate/rc/rdi/define|define your source connection and data pipeline]].

[](/docs/latest/images/rc/rdi/rdi-define-source-database.png)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page