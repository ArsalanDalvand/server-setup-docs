---
title: "AI Models"
source: "https://stalw.art/docs/server/ai-models"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/ai-models"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AI Models"
---
# AI Models

      

Stalwart can call out to large language models for tasks such as [[spamfilter/llm|spam classification]], threat detection, and [[sieve/llm|message categorization]]. Any endpoint that exposes an OpenAI-compatible chat or text-completion API is supported, whether hosted by a provider such as OpenAI or Anthropic or run locally through a tool such as LocalAI. This choice lets operators balance cost, latency, and privacy according to the deployment.

## Configuration

[Section titled “Configuration”](#configuration)

Each AI endpoint is represented by an [[ref/object/ai-model|AiModel]] object (found in the WebUI under  Settings ›  AI). The relevant fields are:

[[ref/object/ai-model#name|`name`]]: short identifier for the model within Stalwart.
[[ref/object/ai-model#url|`url`]]: full URL of the OpenAI-compatible endpoint (for example `https://api.openai.com/v1/chat/completions`).
[[ref/object/ai-model#model|`model`]]: the model name to send to the endpoint, such as `gpt-4`.
[[ref/object/ai-model#modeltype|`modelType`]]: `Chat` for chat completions or `Text` for text completions. Default `Chat`.
[[ref/object/ai-model#timeout|`timeout`]]: maximum time to wait for a response. Default `"2m"`.
[[ref/object/ai-model#temperature|`temperature`]]: randomness of the response, in the range `0.0` to `1.0`. Default `0.7`.
[[ref/object/ai-model#allowinvalidcerts|`allowInvalidCerts`]]: whether to accept invalid TLS certificates. Default `false`. Recommended only for local or self-signed endpoints.
[[ref/object/ai-model#httpauth|`httpAuth`]]: authentication method, either `Unauthenticated`, `Basic`, or `Bearer`.
[[ref/object/ai-model#httpheaders|`httpHeaders`]]: additional HTTP headers sent with every request.

For example, a chat endpoint authenticated with a bearer token and an extra custom header:

```
{  "name": "chat",  "url": "https://api.openai.com/v1/chat/completions",  "model": "gpt-4",  "modelType": "Chat",  "timeout": "2m",  "temperature": 0.7,  "allowInvalidCerts": false,  "httpAuth": {    "@type": "Bearer",    "bearerToken": {      "@type": "Value",      "secret": "my-secret-token"    }  },  "httpHeaders": {    "X-My-Header": "my-value"  }}
```