# Confluence MCP Server (Atlassian)

## Quick Start
```bash
# GitHub Copilot configuration using Docker
{
  "servers": {
    "confluence": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "CONFLUENCE_URL",
        "-e", "CONFLUENCE_USERNAME", 
        "-e", "CONFLUENCE_API_TOKEN",
        "ghcr.io/sooperset/mcp-atlassian:latest"
      ],
      "env": {
        "CONFLUENCE_URL": "https://your-company.atlassian.net",
        "CONFLUENCE_USERNAME": "your-email@company.com",
        "CONFLUENCE_API_TOKEN": "your-api-token"
      }
    }
  }
}
```

## Prerequisites
- Docker installed and running
- VS Code with GitHub Copilot extension
- Confluence Cloud or Server/Data Center access (version 6.0+)
- Atlassian API token or Personal Access Token
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### Step 1: Pull the Docker Image
```bash
docker pull ghcr.io/sooperset/mcp-atlassian:latest
```

### Step 2: Set Up Authentication

Choose one of the following authentication methods:

#### Option A: API Token (Cloud) - Recommended
1. Go to [https://id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
2. Click "Create API token"
3. Name your token (e.g., "Copilot MCP Server")
4. Copy the generated token immediately

#### Option B: Personal Access Token (Server/Data Center)
1. In Confluence, go to your profile → Personal Access Tokens
2. Click "Create token"
3. Set name and expiry date
4. Copy the token immediately

### Step 3: Configure GitHub Copilot

Create or edit `.vscode/mcp.json` in your project root:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "confluence-url",
      "description": "Confluence URL (e.g., https://company.atlassian.net)"
    },
    {
      "type": "promptString", 
      "id": "confluence-username",
      "description": "Your Confluence email/username"
    },
    {
      "type": "promptString",
      "id": "confluence-token",
      "description": "Confluence API token",
      "password": true
    }
  ],
  "servers": {
    "confluence": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "CONFLUENCE_URL",
        "-e", "CONFLUENCE_USERNAME",
        "-e", "CONFLUENCE_API_TOKEN",
        "ghcr.io/sooperset/mcp-atlassian:latest"
      ],
      "env": {
        "CONFLUENCE_URL": "${input:confluence-url}",
        "CONFLUENCE_USERNAME": "${input:confluence-username}",
        "CONFLUENCE_API_TOKEN": "${input:confluence-token}"
      }
    }
  }
}
```

### Step 4: Restart VS Code
Restart VS Code to apply the MCP server configuration.

## Configuration Options

### Environment Variables

Add these to the `env` section for additional configuration:

- `CONFLUENCE_SPACES_FILTER`: Filter by space keys (e.g., "DEV,TEAM,DOC")
- `READ_ONLY_MODE`: Set to "true" to disable write operations
- `MCP_VERBOSE`: Set to "true" for detailed logging
- `ENABLED_TOOLS`: Comma-separated list of enabled tools

### Example with Filters
```json
{
  "servers": {
    "confluence": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "CONFLUENCE_URL",
        "-e", "CONFLUENCE_USERNAME",
        "-e", "CONFLUENCE_API_TOKEN",
        "-e", "CONFLUENCE_SPACES_FILTER",
        "-e", "READ_ONLY_MODE",
        "ghcr.io/sooperset/mcp-atlassian:latest"
      ],
      "env": {
        "CONFLUENCE_URL": "https://company.atlassian.net",
        "CONFLUENCE_USERNAME": "user@company.com",
        "CONFLUENCE_API_TOKEN": "your-token",
        "CONFLUENCE_SPACES_FILTER": "DEV,DOCS,TEAM",
        "READ_ONLY_MODE": "false"
      }
    }
  }
}
```

## Features

- **Search & Discovery**: Find pages and content across Confluence spaces
- **Content Creation**: Create and update Confluence pages
- **Space Management**: Browse and manage Confluence spaces
- **Template Support**: Use Confluence templates for consistent documentation
- **Bulk Operations**: Perform operations across multiple pages or spaces

## Example Prompts

```
"Find our coding standards documentation in Confluence and summarize the key points"
"Create a technical design document in Confluence for the new API feature"
"Create meeting notes in Confluence for today's sprint planning"
"Search Confluence for information about our deployment process"
"Find all user stories in Confluence related to the authentication module"
"Update the API documentation in Confluence with the new endpoints"
```

## Common Use Cases

- **Knowledge Search**: Find and summarize existing documentation
- **Content Creation**: Create new pages and documentation
- **Meeting Management**: Manage meeting notes and action items
- **Process Documentation**: Document workflows and procedures

## Available Tools

The Confluence MCP server provides these tools (exact names may vary):

- `confluence_search`: Search for content across spaces
- `confluence_get_page`: Retrieve specific page content
- `confluence_create_page`: Create new pages
- `confluence_update_page`: Update existing pages
- `confluence_list_spaces`: List available Confluence spaces
- `confluence_get_space`: Get space information and pages

## Troubleshooting

### Common Issues

1. **Docker Not Running**: Ensure Docker is installed and running
   ```bash
   docker --version
   docker ps
   ```

2. **Authentication Failed**: Verify your API token and username
   - Check token hasn't expired
   - Verify username format (usually email for Cloud)

3. **Network Issues**: For server deployments, ensure network access
   ```bash
   docker run --rm curlimages/curl:latest curl -I https://your-confluence-url
   ```

4. **Permission Denied**: Ensure your account has appropriate permissions
   - Read access to spaces you want to search
   - Write access for content creation

### Debug Mode
Enable verbose logging for troubleshooting:

```json
{
  "env": {
    "MCP_VERBOSE": "true",
    "MCP_LOGGING_STDOUT": "true"
  }
}
```

## Advanced Configuration

### OAuth 2.0 Setup (Advanced)
For enhanced security, you can use OAuth 2.0 authentication:

1. Go to [Atlassian Developer Console](https://developer.atlassian.com/console/myapps/)
2. Create an OAuth 2.0 integration app
3. Configure required permissions for Confluence
4. Run the OAuth setup wizard:
   ```bash
   docker run --rm -i \
     -p 8080:8080 \
     -v "${HOME}/.mcp-atlassian:/home/app/.mcp-atlassian" \
     ghcr.io/sooperset/mcp-atlassian:latest --oauth-setup -v
   ```

### Read-Only Mode
For security in production environments:

```json
{
  "env": {
    "READ_ONLY_MODE": "true"
  }
}
```

### Space Filtering
Limit access to specific Confluence spaces:

```json
{
  "env": {
    "CONFLUENCE_SPACES_FILTER": "ENGINEERING,DOCS,PRODUCT"
  }
}
```

## Compatibility

| Product | Deployment | Support |
|---------|------------|---------|
| Confluence | Cloud | ✅ Fully supported |
| Confluence | Server/Data Center 6.0+ | ✅ Supported |

## Related Servers
- **Sequential Thinking**: For complex documentation workflows
- **Playwright**: For testing Confluence integrations

## Links
- [GitHub Repository](https://github.com/sooperset/mcp-atlassian)
- [Docker Hub](https://ghcr.io/sooperset/mcp-atlassian)
- [Atlassian API Documentation](https://developer.atlassian.com/cloud/confluence/rest/v2/)
- [Creating API Tokens](https://id.atlassian.com/manage-profile/security/api-tokens)