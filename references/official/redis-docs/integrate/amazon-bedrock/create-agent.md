---
title: "Create a Bedrock agent"
source: "https://redis.io/docs/latest/integrate/amazon-bedrock/create-agent/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/amazon-bedrock/create-agent"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create a Bedrock agent"
---
# Create a Bedrock agent

Shows how to set up your Agent in Amazon Bedrock.

After you have [[integrate/amazon-bedrock/create-knowledge-base|created a knowledge base]], you can use it to create an agent on Amazon Bedrock.

Before you begin this guide, you will need:

An [AWS IAM Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) with [permissions for the Bedrock agent](https://docs.aws.amazon.com/bedrock/latest/userguide/security_iam_id-based-policy-examples.html).

A [[integrate/amazon-bedrock/create-knowledge-base|Bedrock knowledge base]] connected to a [[integrate/amazon-bedrock/set-up-redis|Redis Cloud vector database]].

## Create an agent

Sign in to the [AWS console](https://console.aws.amazon.com/).

Use the Services menu to locate and select Machine Learning > Amazon Bedrock.  This takes you to the Amazon Bedrock admin panel.

Select Agents > Create Agent to create your knowledge base.

[](/docs/latest/images/rc/bedrock-aws-button-create-agent.png)

In the Agent name section, enter a name and description for your agent.

Select whether or not you want the agent to be able to ask for additional information in the User input section.

Select the IAM role for the Bedrock agent in the IAM Permissions section.

Choose how long you want your idle session timeout to be in the Idle session timeout section. Select Next to continue.

In the Model details section, choose which model you want to use and enter the instructions for your agent. Select Next to continue.

In the Action groups section, you may specify any tasks you would like the agent to perform. Select Next to continue.

Select the [knowledge base](#create-a-knowledge-base) you created and summarize the information in the knowledge base in the Knowledge base instructions for Agent form. Select Add another knowledge base if you would like to add multiple knowledge bases.

[](/docs/latest/images/rc/bedrock-aws-button-add-knowledge-base.png)

Select Next to continue.

Review your agent before you create it. Select Create Agent to finish creation.

[](/docs/latest/images/rc/bedrock-aws-button-create-agent.png)

Amazon Bedrock will create your agent and link it to your knowledge base. This will take some time.

Your agent will have a status of Ready when it is ready to be tested.

[](/docs/latest/images/rc/bedrock-aws-status-agent-ready.png)

Select the name of your agent to view the versions and draft aliases of your agent. You can also test your agent by entering prompts in the Enter your message here field.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page