---
title: "Configure client apps"
source: "https://redis.io/docs/latest/integrate/redis-mcp/client-conf/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-mcp/client-conf"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Configure client apps"
---
# Configure client apps

Configure client apps to use the Redis MCP server.

When you have [[integrate/redis-mcp/install|installed]]
the Redis MCP server, you must also configure your client app to use it.
The sections below describe the ways you can do this.

## Smithery

[Smithery](https://smithery.ai/) provides a searchable repository of scripts
that add configurations for many MCP services to client apps.
The easiest way to configure your client is to use the
[Smithery tool for Redis MCP](https://smithery.ai/server/@redis/mcp-redis).

When you select your client from the Install bar on the Redis MCP page,
you will see a command line that you can copy and paste into a terminal.
Running this command will configure your client app to use Redis MCP. (Note
that you must have [Node.js](https://nodejs.org/en) installed to run
the Smithery scripts.) For example, the command line for
[Claude Desktop](https://claude.ai/download) is

```bash
npx -y @smithery/cli@latest install @redis/mcp-redis --client claude
```

The script will prompt you for the information required to connect to
your Redis database.

## Manual configuration

You can also add the configuration for Redis MCP to your client app
manually. The exact method varies from client to client but the
basic approach is similar in each case. The sections below
give the general configuration details for some common MCP client tools:

[Augment Code](#augment-code)
[Claude Desktop](#claude-desktop)
[OpenAI Agents](#openai-agents)
[VS Code with GitHub Copilot](#vs-code-with-github-copilot)

### Augment Code

Redis supports Augment Code's
[Easy MCP](https://docs.augmentcode.com/setup-augment/mcp#easy-mcp%3A-one-click-integrations)
feature in [VSCode](https://docs.augmentcode.com/setup-augment/mcp#getting-started-with-easy-mcp)
and the [JetBrains IDEs](https://docs.augmentcode.com/jetbrains/setup-augment/mcp#getting-started-with-easy-mcp)
to install the server in seconds:

Open the Augment settings panel and
navigate to the MCP pane.
Click the "+" button next to Redis in the
Easy MCP installation list and enter the connection details for your Redis database.
Click Install to start using Redis MCP.

If you need to supply environment variables or command line parameters:

Click the Add MCP button underneath the list of Easy MCP integrations.
Enter `Redis` in the name field and paste the appropriate command line in the
command field (see
[[integrate/redis-mcp/install#configuration|Configuration]] for
more information about the available command line options).
Click the + Variable button to add any environment variables that you need.
Click Add to add the server.

### Claude Desktop

First, locate the configuration file by selecting Settings from the menu, then selecting the Developer tab, and then clicking the Edit Config button. Open this JSON file and add your settings as shown below to run Redis MCP with [`uvx`](https://docs.astral.sh/uv/guides/tools/#running-tools):

```json
{
    "mcpServers": {
        .
        .
        "redis-mcp-server": {
            "type": "stdio",
            "command": "uvx",
            "args": [
                "--from", "redis-mcp-server@latest",
                "redis-mcp-server",
                "--url", "redis://localhost:6379/0"
            ]
        }
    }
        .
        .
  }
```

You can also optionally set the environment for the command shell here in the
`env` section:

```json
"redis": {
    "type": "stdio",
    "command": "uvx",
    "args": [
        "--from", "redis-mcp-server@latest",
        "redis-mcp-server",
        "--url", "redis://localhost:6379/0"
    ],
    "env": {
        "REDIS_HOST": "<your_redis_database_hostname>",
        "REDIS_PORT": "<your_redis_database_port>",
        "REDIS_PWD": "<your_redis_database_password>",
        "REDIS_SSL": True|False,
        "REDIS_CA_PATH": "<your_redis_ca_path>",
        "REDIS_CLUSTER_MODE": True|False
    }
}
```

If you are using
[[integrate/redis-mcp/install#install-using-docker|Docker]]
to deploy the server, change the `command` and `args` sections of the
configuration as shown below:

```json
"redis": {
    "command": "docker",
    "args": ["run",
            "--rm",
            "--name",
            "redis-mcp-server",
            "-i",
            "-e", "REDIS_HOST=<redis_hostname>",
            "-e", "REDIS_PORT=<redis_port>",
            "-e", "REDIS_USERNAME=<redis_username>",
            "-e", "REDIS_PWD=<redis_password>",
            "mcp-redis"]
}
```

For more information about using MCP with Claude Desktop, see
[Connect to Local MCP Servers](https://modelcontextprotocol.io/quickstart/user).

### OpenAI Agents

The
[`redis_assistant.py`](https://github.com/redis/mcp-redis/blob/main/examples/redis_assistant.py)
file in the [`mcp-redis`](https://github.com/redis/mcp-redis) repository contains an
example of how to configure OpenAI Agents to use Redis MCP.

To use this example, install the `openai-agents` library, ensure
you have exported the OpenAI token in the `OPENAI_API_KEY` environment variable
and run the `redis_assistant.py` script:

```bash
pip install openai-agents
export OPENAI_API_KEY=<your_openai_api_key>
python3 redis_assistant.py
```

See the
[OpenAI Agents SDK documentation](https://openai.github.io/openai-agents-python/mcp/)
for more information about using MCP servers.

### VS Code with GitHub Copilot

To use Redis MCP with VS Code, first add the following to your `settings.json`
file to enable the
[agent mode tools](https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode):

```json
{
  "chat.agent.enabled": true
}
```

Then, add the following lines to `settings.json` to run Redis MCP
with [`uvx`](https://docs.astral.sh/uv/guides/tools/#running-tools):

```json
.
  .
"mcp": {
    "servers": {
        "Redis MCP Server": {
        "type": "stdio",
        "command": "uvx", 
        "args": [
            "--from", "redis-mcp-server@latest",
            "redis-mcp-server",
            "--url", "redis://localhost:6379/0"
        ]
        },
    }
},
  .
  .
```

You can also add
[[integrate/redis-mcp/install#environment-variables|environment variables]]
in the `env` section of the configuration:

```json
"Redis MCP Server": {
    .
    .
  "env": {
    "REDIS_HOST": "<your_redis_database_hostname>",
    "REDIS_PORT": "<your_redis_database_port>",
    "REDIS_USERNAME": "<your_redis_database_username>",
    "REDIS_PWD": "<your_redis_database_password>",
      .
      .
  }
}
```

See
[Use MCP servers in VS Code](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)
for more information.

## Redis Cloud MCP

If you are using
[[integrate/redis-mcp/install#redis-cloud-mcp|Redis Cloud MCP]],
the configuration is similar to [basic MCP](#manual-configuration), but with a
few differences. Set the client to run the server using the `node` command, as shown
in the example for Claude Desktop below:

```json
{
  "mcpServers": {
    "mcp-redis-cloud": {
      "command": "node",
      "args": ["--experimental-fetch", "<absolute_path_to_project_root>/dist/index.js"],
      "env": {
        "API_KEY": "<redis_cloud_api_key>",
        "SECRET_KEY": "<redis_cloud_api_secret_key>"
      }
    }
  }
}
```

Here, the environment includes the Redis Cloud API key and API secret key
(see [[operate/rc/api|Redis Cloud REST API]] for more
information).

If you are deploying Redis Cloud MCP with Docker, use a configuration like
the following to launch the server with the `docker` command:

```json
{
  "mcpServers": {
    "redis-cloud": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "API_KEY=<your_redis_cloud_api_key>",
        "-e",
        "SECRET_KEY=<your_redis_cloud_api_secret_key>",
        "mcp/redis-cloud"
      ]
    }
  }
}
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page