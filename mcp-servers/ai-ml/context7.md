# Context7 MCP Server

**Category**: AI/ML  
**Auth**: None  
**Description**: Up-to-date code documentation and examples for any library or framework

## Overview

Context7 MCP pulls up-to-date, version-specific documentation and code examples straight from the source and places them directly into your prompt. No more outdated examples or hallucinated APIs.

**Key Features:**
- Real-time documentation fetching
- Version-specific code examples
- No authentication required
- Support for popular libraries and frameworks
- Direct integration with prompts using `use context7`

## Usage

Add `use context7` to any prompt in Copilot:

```txt
Create a Next.js middleware that checks for a valid JWT in cookies and redirects unauthenticated users to `/login`. use context7
```

```txt
Configure a Cloudflare Worker script to cache JSON API responses for five minutes. use context7
```

## Installation

```json
{
  "servers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    }
  }
}
```

## Benefits

- **No outdated code**: Always current examples and documentation
- **Accurate APIs**: Real APIs instead of hallucinated ones
- **Version-specific**: Documentation matches the exact version you're using
- **No tab-switching**: Everything in your prompt context
- **No authentication**: Works without any setup or keys

## Common Use Cases

- **Framework Setup**: Get latest setup instructions for React, Vue, Angular
- **API Integration**: Current endpoints and authentication methods
- **Library Usage**: Up-to-date examples for popular packages
- **Best Practices**: Current patterns and recommendations

## Notes

- No setup or configuration required
- Works with most popular JavaScript/TypeScript libraries
- Continuously updated documentation database
- Pulls documentation directly from official sources

## Repository

- **Source**: [upstash/context7](https://github.com/upstash/context7)
- **NPM**: [@upstash/context7-mcp](https://www.npmjs.com/package/@upstash/context7-mcp)