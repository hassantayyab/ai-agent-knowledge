# Model Context Protocol: a USB‑C for agent tools (Chapter 11)

## Summary

- **MCP (Model Context Protocol)** standardizes how models/agents access tools, solving the “every provider + tool has its own interface” problem.
- Origin: proposed by a small Anthropic team in **Nov 2024**.
- Mental model: MCP is like a **USB‑C port** for AI apps, a universal adapter so compatible tools/agents can plug into each other.
- MCP has two primitives:
  - **Servers** expose a set of tools.
  - **Clients** discover tools from servers and invoke them.
- Ecosystem grew quickly as vendors + developers shipped servers and registries emerged (e.g., Smithery, PulseMCP, mcp.run).
- Remaining challenges are ecosystem-level: **discovery**, **quality**, and **configuration**.

## Core concepts

### What MCP is

MCP is an **open protocol** for connecting AI agents to tools (and, more generally, to other MCP-compatible systems). If your tool or agent “speaks MCP”, it can interoperate with other MCP-compatible components.

### Why MCP exists

Before MCP, each provider and tool author had their own way of defining and calling tools. MCP’s value is tool portability across models/frameworks.

### MCP primitives: servers and clients

- **Servers**: wrap sets of MCP tools. They (and the underlying tools) can be written in any language and communicate with clients (often over HTTP, or via stdio depending on setup).
- **Clients**: used by models/agents to:
  1. query servers for available tools
  2. request tool execution and receive results

The chapter notes MCP functions like a standard for remote code execution, akin to OpenAPI or RPC.

### When to use MCP

- If your agent needs lots of standard integrations (calendar/chat/email/web), use an **MCP client** to plug into third-party capabilities.
- If you’re building a tool you want other agents to use, ship an **MCP server**.

---

## Code example: building an MCP server + client (Mastra, TypeScript)

(Transcribed from the code images in the chapter.)

### `weatherTool.ts`

```ts
// --- weatherTool.ts ---
import { defineTool } from '@mastra/core/tool';

export const weatherTool = defineTool({
  name: 'weatherTool',
  description: 'Get the current weather for a city.',
  parameters: {
    city: { type: 'string', description: 'City name' },
  },
  async execute({ city }) {
    // Dummy implementation
    return `The weather in ${city} is sunny!`;
  },
});
```

### `weather-server.ts`

```ts
// --- weather-server.ts ---
import { MCPServer } from '@mastra/mcp';
import { weatherTool } from './weatherTool';

const server = new MCPServer({
  name: 'Weather Server',
  version: '1.0.0',
  tools: { weatherTool },
});

await server.startStdio();
```

### `agent.ts`

```ts
// --- agent.ts ---
import { MCPClient } from '@mastra/mcp';
import { Agent } from '@mastra/core/agent';
import { openai } from '@ai-sdk/openai';

const mcp = new MCPClient({
  servers: {
    weather: {
      command: 'npx',
      args: ['tsx', 'weather-server.ts'],
    },
  },
  timeout: 30000,
});

const agent = new Agent({
  name: 'Weather Agent',
  instructions: 'You can answer weather questions using the weather tool.',
  model: openai('gpt-4'),
  tools: await mcp.getTools(),
});
```

---

## Code example: proxy MCP server that aggregates other servers

(Transcribed from the code image in the chapter.)

```ts
import { MCPClient, MCPServer } from '@mastra/mcp';

// Step 1: Create an MCP client that connects to other MCP servers
const mcp = new MCPClient({
  servers: {
    weather: {
      // Connect to a remote MCP server via HTTP/SSE
      url: new URL('http://localhost:1234/sse'),
    },
    stocks: {
      // Or connect to a local MCP server via stdio
      command: 'npx',
      args: ['tsx', 'stock-server.ts'],
    },
  },
  timeout: 30000,
});

// Step 2: Expose all tools from the connected MCP servers via a new MCPServer
const server = new MCPServer({
  name: 'Proxy MCP Server',
  version: '1.0.0',
  tools: await mcp.getTools(), // Aggregate tools from all connected servers
});

// Step 3: Start the proxy MCP server (stdio)
await server.startStdio();
```

## What’s next for MCP (ecosystem challenges)

- **Discovery:** no centralized/standard tool discovery yet; multiple registries create fragmentation.
- **Quality:** no “verification badges” equivalent yet (registries are working on it).
- **Configuration:** providers still differ in config schemas and operational ergonomics.

## Practical takeaways

- If you want tool portability across models/frameworks, MCP is the direction of travel.
- App builders: start by consuming existing MCP servers via a client.
- Tool builders: shipping an MCP server increases reach without bespoke provider integrations.
