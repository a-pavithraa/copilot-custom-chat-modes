# Playwright MCP Server

## Quick Start
```bash
# GitHub Copilot configuration
{
  "servers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

## Prerequisites
- Node.js 18 or newer
- VS Code with GitHub Copilot extension
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### For GitHub Copilot in VS Code

1. Create or edit `.vscode/mcp.json` in your project root:

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

2. Restart VS Code to apply the configuration.

### Quick Install Button
[![Install in VS Code](https://img.shields.io/badge/VS_Code-VS_Code?style=flat-square&label=Install%20Server&color=0098FF)](https://insiders.vscode.dev/redirect?url=vscode%3Amcp%2Finstall%3F%257B%2522name%2522%253A%2522playwright%2522%252C%2522command%2522%253A%2522npx%2522%252C%2522args%2522%253A%255B%2522%2540playwright%252Fmcp%2540latest%2522%255D%257D)

## Configuration Options

The Playwright MCP server supports various configuration arguments. Add them to the `args` array:

```json
{
  "servers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest",
        "--headless",
        "--browser", "chrome",
        "--device", "iPhone 15"
      ]
    }
  }
}
```

### Common Configuration Arguments

- `--headless`: Run browser in headless mode
- `--browser <browser>`: Choose browser (chrome, firefox, webkit, msedge)
- `--device <device>`: Emulate specific device (e.g., "iPhone 15")
- `--allowed-origins <origins>`: Semicolon-separated list of allowed origins
- `--blocked-origins <origins>`: Semicolon-separated list of blocked origins
- `--ignore-https-errors`: Ignore HTTPS certificate errors
- `--isolated`: Keep browser profile in memory only

## Features

- **Fast and lightweight**: Uses Playwright's accessibility tree, not pixel-based input
- **LLM-friendly**: Operates on structured data without requiring vision models
- **Deterministic**: Avoids ambiguity common with screenshot-based approaches
- **Cross-browser**: Supports Chrome, Firefox, WebKit, and Edge

## Example Prompts

```
"Navigate to example.com and extract all the product prices"
"Fill out the contact form on the website with test data"
"Test the login flow and verify the dashboard loads correctly" 
"Check if all navigation links are working on the homepage"
"Take a screenshot of the current page and highlight any broken images"
"Automate clicking through the checkout process and verify each step"
```

## Common Use Cases

- **Web Scraping**: Extract data from websites systematically
- **Form Testing**: Automate form filling and validation
- **UI Testing**: Test user interface interactions and flows
- **Content Validation**: Verify page elements and functionality

## Troubleshooting

### Common Issues

1. **Browser not found**: Ensure Node.js 18+ is installed
2. **Permission errors**: Check that the MCP server policy is enabled
3. **Network issues**: Use `--ignore-https-errors` for development sites

### Debug Mode
Add `--verbose` to the args array for detailed logging:

```json
{
  "servers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--verbose"]
    }
  }
}
```

## Related Servers
- **Sequential Thinking**: For complex multi-step web interactions
- **File System**: For saving scraped data or test results

## Links
- [GitHub Repository](https://github.com/microsoft/playwright-mcp)
- [Playwright Documentation](https://playwright.dev/)
- [GitHub Copilot MCP Guide](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/extend-copilot-chat-with-mcp)