---
title: "Create data pipeline"
source: "https://redis.io/docs/latest/operate/rc/rdi/define/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/rdi/define"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create data pipeline"
---
# Create data pipeline

Define the source connection and data pipeline.

            Redis Cloud
          

After you have [[operate/rc/rdi/setup|prepared your source database]] and connection information, and [[operate/rc/rdi/create-workspace|created the workspace for your pipeline]], you can set up your new pipeline.

In the [Redis Cloud console](https://cloud.redis.io/), go to your target database and select the Data Integration tab. You'll see your subscription's workspace. Select Add pipeline to add a pipeline to the workspace.

You can also go to the Pipelines page from the left-hand menu and select Add pipeline from your workspace. Or, you can go to the Data Integration tab of your subscription and select Add pipeline from your workspace.

[](/docs/latest/images/rc/rdi/rdi-workspace-add-pipeline.png)

If you've started to create a pipeline, you'll see a draft pipeline. Select More actions > Resume pipeline setup to continue with pipeline setup.

[](/docs/latest/images/rc/rdi/rdi-workspace-resume-setup.png)

Creating a pipeline is split into the following steps:

[Pipeline setup](#pipeline-setup): Defines the source database type and the target database.
[Source configuration](#source-configuration): Defines and tests the source database connectivity and credentials.
[Dataset](#dataset): Defines the data to import to your Redis database.
[Transformations](#transformations): Defines how records are stored in Redis.
[Review & deploy](#review-and-deploy): Shows your pipeline details and deploys it.

## Pipeline setup

In the Pipeline setup step:

Select your source database type. The following database types are supported:

MySQL
mariaDB
Oracle
SQL Server
PostgreSQL
Snowflake (Preview)
[](/docs/latest/images/rc/rdi/rdi-select-source-db.png)

Enter a name for your source database in the Source name field. This is a name for the source database that will appear on Redis Cloud.

Select the target Redis Cloud database from the Target database list.

[](/docs/latest/images/rc/rdi/rdi-choose-target.png)

Select Continue to source to move to the Source configuration step.

[](/docs/latest/images/rc/rdi/rdi-continue-to-source-button.png)

## Source configuration

During the Source configuration step, you'll share the connectivity information and credentials you created when you prepared your source database.

This step is separated into three expandable sections:

[Source connectivity](#source-connectivity) defines the connectivity method for your source database.
[Secrets](#secrets) defines the authentication needed for your source database.
[Source configuration](#source-configuration-section) defines the source-specific settings for your database connection.

When you've completed all three sections, select Test source to test Redis Cloud's connection with the source database. After the test completes, select Continue to dataset to move to the [Dataset](#dataset) step.

[](/docs/latest/images/rc/rdi/rdi-continue-to-dataset-button.png)

### Source connectivity

Select whether you want your pipeline to connect to your database using AWS Private Link or using the Public endpoint.

If your pipeline uses AWS PrivateLink, enter the Private Link service name of the [[operate/rc/rdi/setup#set-up-connectivity|PrivateLink connected to your source database]].

[](/docs/latest/images/rc/rdi/rdi-source-configuration-source-connectivity-privatelink.png)

Select Connect to Private Link to test your Private Link connectivity. This will take a few minutes.

If Redis Cloud can't find your PrivateLink connection, make sure that the PrivateLink service name is correct and that Redis Cloud is listed as an Allowed Principal for your VPC. See [[operate/rc/rdi/setup#set-up-connectivity|Set up connectivity]] for more info.

If your pipeline uses the public endpoint, enter the source IP address or hostname in the Source IP address / Hostname field.

[](/docs/latest/images/rc/rdi/rdi-source-configuration-source-connectivity-public.png)

### Secrets

[](/docs/latest/images/rc/rdi/rdi-source-configuration-secrets.png)

Enter the ARN of your [[operate/rc/rdi/setup#create-database-credentials-secrets|database credentials secret]] in the Credentials secret ARN field.

For Snowflake source databases, select the authentication method that matches the [[operate/rc/rdi/setup#create-database-credentials-secrets|credentials secret]] you created:

Password: The credentials secret must contain both `username` and `password`.
Key-pair: The credentials secret must contain the `username` (if a `password` is present, it is ignored). Enter the ARN of your [[operate/rc/rdi/setup#create-database-credentials-secrets|client key secret]] in the Private Key Secret ARN field. This secret must contain the private key in plain text PEM format.

Under Transit security:

If your database requires TLS, select TLS. Enter the ARN of your [[operate/rc/rdi/setup#create-database-credentials-secrets|CA certificate secret]] in the CA Certificate Secret ARN field.

    
    	
    
    [](/docs/latest/images/rc/rdi/rdi-define-tls.png)

If your database requires mTLS, select mTLS.

    
    	
    
    [](/docs/latest/images/rc/rdi/rdi-define-mtls.png)

Enter the following secrets in the fields:

CA Certificate Secret ARN: [[operate/rc/rdi/setup#create-database-credentials-secrets|CA certificate secret]]
Client Certificate Secret ARN: [[operate/rc/rdi/setup#create-database-credentials-secrets|Client certificate secret]]
Client Private Key Secret ARN: [[operate/rc/rdi/setup#create-database-credentials-secrets|Client key secret]]
Password secret ARN for secret store (Optional): [[operate/rc/rdi/setup#create-database-credentials-secrets|Client key passphrase secret]]

Select Validate to check that Redis Cloud can access your secrets.

### Source configuration

In this section, you'll enter your database details. This depends on your database type, and can include:

Port: The database's port
Database(s): Your database's name, or the root database (PostgreSQL, Oracle only), or a comma-separated list of one or more databases you want to connect to (SQL Server and MongoDB only)
Database Server ID: Unique ID for the replication client. Enter a number that is not used by any existing replication clients (mySQL and mariaDB only)
PDB: Name of the Oracle pluggable database (Oracle only)
Connection string: The connection information for your database (MongoDB only)

Under Collector properties, Select Edit advanced properties to configure additional optional properties for your pipeline.

[](/docs/latest/images/rc/rdi/rdi-source-configuration-collector-properties.png)
[](/docs/latest/images/rc/rdi/rdi-advanced-properties.png)

You can add collector source properties in the Collector source properties section and collector sink properties in the Collector sink properties section. See the RDI configuration file reference for all available [[integrate/redis-data-integration/reference/config-yaml-reference#sourcesadvancedsource|collector source properties]] and [[integrate/redis-data-integration/reference/config-yaml-reference#sourcesadvancedsink|collector sink properties]]. Select Save properties to return to Source configuration.

## Dataset

In this step, you'll select the data that you want to import and synchronize with your primary database.

Note:
  This step may change depending on your source database.

[](/docs/latest/images/rc/rdi/rdi-dataset-empty.png)

In the Schemas section, select the schema(s) you want to migrate to the target database from the list.

[](/docs/latest/images/rc/rdi/rdi-dataset-schema-selected.png)

When you select a schema, you will see its tables in the Tables section. Redis Cloud will automatically select all tables for import. You can de-select any columns you do not wish to import to your Redis database.

Select a table to view its columns in the Columns section. You can de-select any columns you do not wish to import.

[](/docs/latest/images/rc/rdi/rdi-select-columns.png)

If any tables are missing a unique key, a warning will appear in the Data modeling section. Select Show affected to filter the Tables section to the tables without a unique key.

[](/docs/latest/images/rc/rdi/rdi-dataset-missing-unique-key.png)

For these tables, select the key icon next to the column that defines a unique key.

[](/docs/latest/images/rc/rdi/rdi-unique-key-selected.png)

Select Continue to transformations to move to the Transformations step.

[](/docs/latest/images/rc/rdi/rdi-continue-to-transformations-button.png)

## Transformations

In this step, you'll choose how the pipeline will store your data in Redis.

[](/docs/latest/images/rc/rdi/rdi-transformations.png)

Select how your records will be stored in Redis. You can choose Hash or JSON.

Under Transformation jobs, you can supply one or more [transformation job files](/docs/latest/integrate/redis-data-integration/data-pipelines/transform-examples/) that specify how you want to transform the captured data before writing it to the target. Select Upload jobs to upload your job files. When you upload job files, Redis Cloud will validate the job files to check for errors.

Select Edit advanced properties to add any processor properties to control how the data is processed.

[](/docs/latest/images/rc/rdi/rdi-processor-advanced-properties.png)

See the [[integrate/redis-data-integration/reference/config-yaml-reference#processors|RDI configuration file reference]] for all available processor properties.

## Review and deploy

Review the tables you selected in the Review and deploy step. If everything looks correct, select Deploy pipeline to start ingesting data from your source database.

[](/docs/latest/images/rc/rdi/rdi-confirm-deploy.png)

At this point, the data pipeline will ingest data from the source database to your target Redis database. This process will take time, especially if you have a lot of records in your source database.

After this initial sync is complete, the data pipeline enters the change streaming phase, where changes are captured as they happen. Changes in the source database are added to the target within a few seconds of capture.

You can view the status of your data pipeline in the Data pipeline tab of your database. See [View and edit data pipeline](/docs/latest/operate/rc/rdi/view-edit/) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page