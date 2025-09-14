# MCP Servers for GitHub Copilot

MCP (Model Context Protocol) server configurations to extend GitHub Copilot capabilities.

## Quick Setup
```bash
# Create configuration file
mkdir -p .vscode && touch .vscode/mcp.json  # macOS/Linux
mkdir .vscode && type nul > .vscode\mcp.json  # Windows
```

## Available Servers

| Server | Type | Auth | Description |
|--------|------|------|-------------|
| [Filesystem](./core/filesystem.md) | Core | None | Local file and directory operations |
| [Git](./core/git.md) | Core | None | Git version control operations |
| [Playwright](./web/playwright.md) | Core | None | Browser automation via accessibility tree |
| [Sequential Thinking](./ai-ml/sequential-thinking.md) | Core | None | Structured problem-solving framework |
| [Figma](./development/figma.md) | Dev | API Key | Design-to-code conversion |
| [Confluence](./development/confluence.md) | Dev | Token | Documentation search & management |
| [GitLab Analyzer](./internal/gitlab-repo-analyzer.md) | Internal | Token | Repository architecture analysis |
| [GitLab Fetcher](./internal/gitlab-file-fetcher.md) | Internal | Token | File retrieval with caching |

## Installation Profiles

### Minimal (Core Only)
```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    },
    "git": {
      "command": "npx",
      "args": ["@cyanheads/git-mcp-server"]
    },
    "playwright": {
      "command": "npx", 
      "args": ["@playwright/mcp@latest"]
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

### Full Stack (+ Design Tools)
Add to minimal profile:
```json
{
  "servers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--figma-api-key=${input:figma-token}", "--stdio"]
    }
  },
  "inputs": [
    {
      "type": "promptString",
      "id": "figma-token",
      "description": "Figma API token",
      "password": true
    }
  ]
}
```

### Enterprise (+ Internal Tools)
Add to full stack profile:
```json
{
  "servers": {
    "confluence": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "CONFLUENCE_URL", "-e", "CONFLUENCE_USERNAME", "-e", "CONFLUENCE_API_TOKEN", "ghcr.io/sooperset/mcp-atlassian:latest"]
    },
    "gitlab-analyzer": {
      "command": "./.vscode/gitlab-repo-analyzer/gitlab-repo-analyzer",
      "args": ["--stdio"]
    }
  }
}
```

## Common Workflows

- **Design → Code**: Figma → Sequential Thinking → Filesystem → Git → Playwright
- **Documentation**: Confluence → Sequential Thinking → Filesystem → Git
- **Code Analysis**: GitLab Fetcher → Filesystem → GitLab Analyzer → Sequential Thinking → Git
- **Local Development**: Filesystem → Git → Sequential Thinking → Playwright
- **Version Control**: Git → Filesystem → Sequential Thinking

## Troubleshooting

**Server not found**: Check VS Code MCP extension enabled  
**Auth failed**: Verify API tokens and permissions  
**Docker issues**: Ensure Docker running  
**Debug mode**: Add `"MCP_VERBOSE": "true"` to server env

## Links

- [GitHub Copilot MCP Guide](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/extend-copilot-chat-with-mcp)
- [Model Context Protocol](https://modelcontextprotocol.io/)