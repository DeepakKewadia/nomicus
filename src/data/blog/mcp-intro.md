---
author: Deepak Kewadia
pubDatetime: 2025-07-21T20:40:08Z
modDatetime: 2025-07-21T18:59:05Z
title: "Breaking Down MCP: Week 1 – The Problem and Its Significance"
featured: false
draft: false
tags:
  - mcp
  - ai
canonicalURL: ''
description: Discover the problem MCP solves and why it’s a game-changer in this byte-sized introduction.. 
---

## MCP (Model Context Protocol)

**MCP** is proposed by [**Anthropic**](https://www.anthropic.com/news/model-context-protocol) to standardize the whole tools calling mechanism in AI domain.
you can think of it as Type-C Port in your system, you can power the machine, transfer data etc., through the the same port. So this is the standardization which MCP provides we can connect multiple tools like web search, custom python code functions etc. without any extra hazels.

**Without MCP**:
We are facing MXN Problem with current approach, suppose we have 2 hosts application and 3 tools. now we have to make 2x3=6 connection. which is over burdening the hosts with management of multiple connections.

![without_mcp](@/assets/images/without_mcp.png)

## MCP Architecture

MCP uses Client-Server Architecture where one client can connect to multiple servers.
this is based on the JSON-RPC 2.0 message communication format (JSON-RPC is a lightweight remote procedure call protocol encoded in JSON).

![with_mcp](@/assets/images/with_mcp.png)

Using MCP, Our MxN problem shrink down to M+N for 2 host and 3 tools we required 5 connections.

**Different components:**
**Client**:
- Client usually resides in host environment and responsible for maintaining 1:1 connection with the server. client is exposed to low-level protocol details.
**Host**:
- Host is the interface with User and client, it receives commands from user and initiate the connections. 
**Server**:
- Server are the places where our core logic resides, currently MCP supports Tools, Prompts & Resources. this is responsible to execute the code logic, provide the LLM prompts & interact with resources.

Now let's look into JSON-RPC in brief.

Each message block contains below required fields.
- A unique identifier (`id`)
- The method name to invoke (e.g., `tools/call`)
- Parameters for the method (if any)

MCP defines three types of messages.

- Request
- Response 
- Notification

```JSON
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "add",
    "arguments": {
      "a": 10,
      "b": 20
    }
  }
}
```

Now we have the message format with us but how this messages are transmitted to the client/server this part is defined by the MCP. Currently two types of transport mechanisms are supported by the MCP,
We will look into this in next blog.