---
title: "Amazon Bedrock"
source: "https://redis.io/docs/latest/integrate/amazon-bedrock/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/amazon-bedrock"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Amazon Bedrock"
---
# Amazon Bedrock

Shows how to use your Redis database with Amazon Bedrock to customize foundational models.

[Amazon Bedrock](https://aws.amazon.com/bedrock/) streamlines GenAI deployment by offering foundational models (FMs) as a unified API, eliminating complex infrastructure management. It lets you create AI-powered [Agents](https://aws.amazon.com/bedrock/agents/) that execute complex tasks. Through [Knowledge Bases](https://aws.amazon.com/bedrock/knowledge-bases/) within Amazon Bedrock, you can seamlessly tether FMs to your proprietary data sources using retrieval-augmented generation (RAG). This direct integration amplifies the FM's intelligence based on your organization's resources.

Amazon Bedrock lets you choose Redis Cloud as the [vector database](https://redis.io/solutions/vector-search/) for your agent's Knowledge Base. Once Redis Cloud is integrated with Amazon Bedrock, it automatically reads text documents from your Amazon Simple Storage Service (S3) buckets. This process lets the large language model (LLM) pinpoint and extract pertinent context in response to user queries, ensuring your AI agents are well-informed and grounded in their responses.

For more information about the Redis integration with Amazon Bedrock, see the [Amazon Bedrock integration blog post](https://redis.io/blog/amazon-bedrock-integration-with-redis-enterprise/).

To fully set up Bedrock with Redis Cloud, you will need to do the following:

[[integrate/amazon-bedrock/set-up-redis|Set up a Redis Cloud subscription and vector database]] for Bedrock.

[[integrate/amazon-bedrock/create-knowledge-base|Create a knowledge base]] connected to your vector database.

[[integrate/amazon-bedrock/create-agent|Create an agent]] connected to your knowledge base.

## More info

[Amazon Bedrock integration blog post](https://redis.io/blog/amazon-bedrock-integration-with-redis-enterprise/)
[Detailed steps](https://github.com/redis-applied-ai/aws-redis-bedrock-stack/blob/main/README.md)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page