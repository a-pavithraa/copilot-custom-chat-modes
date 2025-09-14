# MCP Server Setup Instructions for GitHub Copilot

> **⚠️ DEPRECATED**: This guide has been superseded by the comprehensive [MCP Servers Documentation](../mcp-servers/README.md). Please use the new documentation for the latest server configurations and setup instructions.

This guide helps you set up MCP (Model Context Protocol) servers with GitHub Copilot. You can install individual servers or explore additional servers from the community.

## Prerequisites

- Node.js 18+ and npm
- VS Code 1.103+ with the GitHub Copilot extension
- Valid API tokens for the services you want to use
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Quick commands (cross-platform)

Create the `.vscode` folder and an `mcp.json` file:

```bash
# macOS / Linux
mkdir -p .vscode && touch .vscode/mcp.json

# Windows (PowerShell/CMD)
mkdir .vscode 2>nul || true; type nul > .vscode\mcp.json
```

## Pre-configured servers in this repository

### Playwright MCP

Example `mcp.json` snippet:

```json
{
    "servers": {
        "playwright": {
            "command": "npx",
            "args": ["@playwright/mcp@latest"]
        }
    }
}
```

### Sequential Thinking MCP

Example `mcp.json` snippet

```json
{
    "servers": {
        "sequential-thinking": {
            "command": "npx",
            "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
        }
    }
}
```

### GitLab MCP

Step 1 — generate a GitLab personal access token with scopes: `api`, `read_api`, `read_repository`.

Step 2 — example `mcp.json` input and server entry:

```json
{
    "inputs": [
        {
            "type": "promptString",
            "id": "gitlab-token",
            "description": "GitLab token to read API",
            "password": true
        }
    ],
    "servers": {
        "GitLab MCP": {
            "type": "stdio",
            "command": "npx",
            "args": ["-y", "@zereight/mcp-gitlab"],
            "env": {
                "GITLAB_PERSONAL_ACCESS_TOKEN": "${input:gitlab-token}",
                "GITLAB_API_URL": "https://gitlab.com/api/v4",
                "GITLAB_READ_ONLY_MODE": "true"
            }
        }
    }
}
```

### Figma MCP

Step 1 — generate a Figma personal access token (Settings → Personal access tokens).

Step 2 — example `mcp.json` entry:

```json
{
    "inputs": [
        {
            "type": "promptString",
            "id": "figma-api-key",
            "description": "Figma API key",
            "password": true
        }
    ],
    "servers": {
        "Figma MCP": {
            "command": "npx",
            "args": [
                "-y",
                "figma-developer-mcp",
                "--figma-api-key=${input:figma-api-key}",
                "--stdio"
            ]
        }
    }
}
```

## Important: Installation Process

**Only the `mcp.json` file needs to be modified for MCP server installation. No other files in your project need to be touched or modified.**

## Start MCP servers

After updating your `mcp.json` file:

1. Save `mcp.json` in the `.vscode` folder.
2. Open the `mcp.json` file in VS Code and click the "Start" button that appears at the top of the editor.
3. Enter tokens/API keys when prompted.
4. Wait for discovery — VS Code will discover the server tools and store them for later sessions.
5. Open Copilot Chat → select "Agent" → click the tools icon (🔧) to verify servers are available.

**⚠️ Important**: You must manually start the MCP servers by clicking the "Start" button in VS Code after updating `mcp.json`. The servers will not start automatically.

## Next Steps

After following the setup above, **please start your MCP servers and verify they are working**. If you encounter any issues or the servers fail to start, ask for troubleshooting help and we can provide additional options including:

- Alternative startup methods
- Manual server startup commands 
- VS Code settings configuration
- Common error solutions

## How to verify MCP servers are working

1. Open Copilot Chat → Agent mode → click the tools icon (🔧).
2. Confirm the servers (Playwright MCP, GitLab MCP, Figma MCP) are listed.
3. Try a simple command (e.g., ask Copilot to navigate to a website and check for console errors for Playwright).

## Troubleshooting

- MCP servers not starting: verify Node.js 18+, restart VS Code, check `mcp.json` syntax, click the Start button, enable `chat.mcp.discovery.enabled`.
- Manual testing: use the `npx` commands above to pinpoint which server fails.
- Token errors: regenerate tokens with correct scopes and verify permissions.

## Token security tips

- GitLab: use minimal scopes, set expirations, and for self-hosted instances update `GITLAB_API_URL`.
- Figma: tokens may expire; revoke old tokens when rotating.

## Installing other MCP servers

If you need MCP servers not listed above:

1. **Check if Playwright MCP is installed**: If not, install it using the configuration above and start it first.

2. **Browse available servers**: Visit [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) to see the community list of available MCP servers.

3. **Follow installation instructions**: Each server in the awesome-mcp-servers repository includes:
   - Installation command (usually `npx package-name`)
   - Configuration example for `mcp.json`
   - Required environment variables or API keys
   - Usage examples

4. **Installation process**: 
   - **Only update the `mcp.json` file** - no other files in your project need modification
   - Add the server configuration to your existing `mcp.json`
   - **You must manually start the server** by clicking the "Start" button in VS Code

5. **General installation pattern**: Most servers follow this pattern:
   ```json
   {
     "servers": {
       "server-name": {
         "command": "npx",
         "args": ["-y", "package-name"],
         "env": {
           "API_KEY": "${input:api-key-id}"
         }
       }
     }
   }
   ```

5. **Add inputs for API keys** (if required):
   ```json
   {
     "inputs": [
       {
         "type": "promptString",
         "id": "api-key-id",
         "description": "Description of the API key",
         "password": true
       }
     ]
   }
   ```

## Advanced options

- Self-hosted GitLab: set `GITLAB_API_URL` to `https://your-gitlab.com/api/v4`.
- For write access, set `GITLAB_READ_ONLY_MODE` to `false`.
- Additional GitLab env flags (example):

```json
{
    "USE_GITLAB_WIKI": "true",
    "USE_MILESTONE": "true",
    "USE_PIPELINE": "true"
}
```
