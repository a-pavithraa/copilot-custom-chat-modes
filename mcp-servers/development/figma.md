# Figma MCP Server

## Quick Start
```bash
# GitHub Copilot configuration
{
  "servers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--figma-api-key=YOUR_FIGMA_API_KEY", "--stdio"]
    }
  }
}
```

## Prerequisites
- Node.js 18+ and npm
- VS Code with GitHub Copilot extension  
- Figma personal access token
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### Step 1: Create Figma API Token

1. Go to your [Figma Account Settings](https://www.figma.com/settings)
2. Navigate to "Personal access tokens"
3. Click "Create new token"
4. Name your token (e.g., "Copilot MCP Server")
5. Copy the generated token

### Step 2: Configure GitHub Copilot

Create or edit `.vscode/mcp.json` in your project root:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "figma-api-key",
      "description": "Figma personal access token",
      "password": true
    }
  ],
  "servers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--figma-api-key=${input:figma-api-key}", "--stdio"]
    }
  }
}
```

### Alternative: Environment Variable Setup

You can also set the API key as an environment variable:

```json
{
  "servers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--stdio"],
      "env": {
        "FIGMA_API_KEY": "your-figma-api-token-here"
      }
    }
  }
}
```

### Step 3: Restart VS Code

Restart VS Code to apply the MCP server configuration.

## Platform-Specific Configuration

### Windows
For Windows users, use this configuration:

```json
{
  "servers": {
    "figma": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "figma-developer-mcp", "--figma-api-key=YOUR_FIGMA_API_KEY", "--stdio"]
    }
  }
}
```

## Features

- **Design-to-Code**: Convert Figma designs directly into code components
- **Layout Information**: Access detailed positioning, sizing, and styling data
- **Component Analysis**: Understand design system components and their properties
- **Optimized Context**: Filtered Figma API responses for better AI accuracy

## Example Prompts

```
"Implement this Figma design as a React component: https://figma.com/file/ABC123"
"Extract the color palette and typography from this Figma file: [paste URL]"
"Analyze the button components in this Figma design system: [paste URL]" 
"Recreate this dashboard layout from Figma using CSS Grid: [paste URL]"
"Convert this Figma mobile design to responsive CSS: [paste URL]"
"Generate Tailwind classes based on this Figma component: [paste URL]"
```

## Common Use Cases

- **Design Implementation**: Convert Figma designs to production code
- **Design System Analysis**: Extract patterns and components
- **Responsive Design**: Adapt designs for different screen sizes
- **Token Extraction**: Generate design tokens from Figma styles

## How It Works

1. Paste a Figma file, frame, or group URL in Copilot chat
2. The MCP server fetches design data from Figma's API
3. Design information is simplified and optimized for AI processing
4. Copilot uses the structured design data to generate accurate code

## Supported URL Formats

- Full Figma files: `https://www.figma.com/file/ABC123/My-Design`
- Specific frames: `https://www.figma.com/file/ABC123/My-Design?node-id=1%3A2`
- Components: `https://www.figma.com/file/ABC123/My-Design?node-id=component-id`

## Troubleshooting

### Common Issues

1. **Invalid API Key**: Verify your Figma personal access token is correct
2. **Permission Denied**: Ensure the token has access to the Figma file
3. **File Not Found**: Check that the Figma URL is accessible and public or shared with your account

### Debug Mode
Add debugging arguments for troubleshooting:

```json
{
  "servers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--figma-api-key=YOUR_KEY", "--stdio", "--verbose"]
    }
  }
}
```

## Configuration Options

- `--figma-api-key`: Your Figma personal access token (required)
- `--stdio`: Use standard input/output for communication (recommended)
- `--port`: Specify a custom port (optional)
- `--verbose`: Enable debug logging (optional)

## Related Servers
- **Playwright**: For testing implemented designs in browsers
- **Sequential Thinking**: For complex design-to-code workflows

## Links
- [GitHub Repository](https://github.com/GLips/Figma-Context-MCP)
- [Framelink Documentation](https://www.framelink.ai/docs/quickstart)
- [Figma API Documentation](https://www.figma.com/developers/api)
- [Creating Figma Access Tokens](https://help.figma.com/hc/en-us/articles/8085703771159-Manage-personal-access-tokens)
- [Demo Video](https://youtu.be/6G9yb-LrEqg)