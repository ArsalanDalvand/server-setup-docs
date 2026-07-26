---
title: "Create and manage Data Integration workspace"
source: "https://redis.io/docs/latest/operate/rc/rdi/create-workspace/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/rdi/create-workspace"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create and manage Data Integration workspace"
---
# Create and manage Data Integration workspace

Create and manage the infrastructure for your Data Integration pipelines.

            Redis Cloud
          

Before you can create your first Data Integration pipeline for a Redis Cloud subscription, you must first deploy the cloud infrastructure needed to host the pipeline and run the workers associated with the pipeline. In Redis Cloud, this is called a Workspace. Each Pro subscription can have one Data integration workspace. You only need to set up the workspace once - any pipelines you create for your subscription will run on the workspace until you delete it. You won't be charged for a workspace until you start running your Data Integration pipeline.

## Create a Data Integration workspace

To create a Data Integration workspace for an existing [Pro subscription](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/):

From the Redis Cloud console, select Data Integration from the left-hand menu. If you don't have any workspaces yet, select Create workspace to go to the Create workspace page.

[](/docs/latest/images/rc/rdi/rdi-create-workspace-button.png)

If you already have a workspace deployed, you'll see your current workspaces. Select New workspace to go to the Create workspace page.

[](/docs/latest/images/rc/rdi/rdi-new-workspace-button.png)

You can also go to the Data Integration tab from your subscription or database page and select Create workspace to go to the Create workspace page for your subscription.

[](/docs/latest/images/rc/rdi/rdi-create-workspace-button.png)

Select your Pro subscription from the list if it's not already selected.

[](/docs/latest/images/rc/rdi/rdi-create-workspace-select-subscription.png)

A Data Integration subnet (CIDR) is automatically generated for you. If, for any reason, a CIDR is not generated, enter a valid CIDR that does not conflict with your applications or other databases.

[](/docs/latest/images/rc/rdi/rdi-create-workspace-cidr.png)

Select Create workspace to create your workspace.

[](/docs/latest/images/rc/rdi/rdi-create-workspace-button.png)

Your workspace will be created in the background. You can select Create pipeline to [[operate/rc/rdi/define|create your pipeline]] while the workspace is provisioning, or you can select Create pipeline later to go back to the Redis Cloud console.

## View workspace status and details

You can view your workspace in one of the following ways:

From the Redis Cloud console, go to the Data integration page, or
From your subscription, select the Data Integration tab

[](/docs/latest/images/rc/rdi/rdi-workspace-add-pipeline.png)

There, you'll see your workspace, as well as any pipeline that you've created in it. To see your workspace details, including the deployment CIDR and region information, select More actions > Workspace details.

[](/docs/latest/images/rc/rdi/rdi-workspace-more-actions.png)

## Delete workspace

Warning:
  Make sure to [delete your data pipeline](/docs/latest/operate/rc/rdi/view-edit/#delete-pipeline) before deleting your workspace.

To delete your workspace, select More actions > Delete workspace from your workspace.

[](/docs/latest/images/rc/rdi/rdi-workspace-more-actions.png)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page