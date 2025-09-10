# GitHub Copilot Repository Instructions

This repository contains custom chat modes and MCP server configurations for GitHub Copilot.

## MCP Server Setup

For detailed instructions on setting up MCP (Model Context Protocol) servers with GitHub Copilot, see [MCP Setup Guide](mcp-setup-guide.md).

Available MCP servers in this repository:
- **Playwright MCP**: Web automation and testing
- **Sequential Thinking MCP**: Step-by-step problem solving
- **GitLab MCP**: GitLab API integration
- **Figma MCP**: Figma design integration

## MCP Server Usage Guidelines

When responding to user requests, Copilot should leverage the appropriate MCP servers:

### Playwright MCP Usage
Use Playwright MCP for:
- Web testing and automation tasks
- Browser interactions and form filling
- Navigation testing and validation
- Console error checking
- DOM manipulation and verification

**Example prompts that should trigger Playwright MCP:**
- "Navigate to example.com and check if there are any console errors"
- "Go to the login page and fill out the form with test credentials"
- "Click the submit button and verify the page redirects correctly"
- "Test the navigation menu and ensure all links work"

### Sequential Thinking MCP Usage
Use Sequential Thinking MCP for:
- Complex problem solving requiring step-by-step analysis
- Architectural decision breakdowns
- Systematic debugging approaches
- Multi-step algorithm design

**Example prompts that should trigger Sequential Thinking MCP:**
- "Help me think through this complex algorithm step by step"
- "Break down this architectural decision into logical steps"
- "Analyze this problem methodically with structured reasoning"
- "Guide me through debugging this issue systematically"

### GitLab MCP Usage
Use GitLab MCP for:
- Issue management and tracking
- Merge request operations
- Branch management
- Pipeline status checking

**Example prompts that should trigger GitLab MCP:**
- "Show recent issues in my project"
- "Create a branch called 'feature/auth'"
- "List all merge requests that are currently open"
- "Show the pipeline status for the latest commit"

### Figma MCP Usage
Use Figma MCP for:
- Design system exploration
- Component analysis
- Code generation from designs
- Design token extraction

**Example prompts that should trigger Figma MCP:**
- "Show components in my design system"
- "Generate React code from this Figma design"
- "List all artboards in the current Figma file"
- "Export the design tokens from this Figma project"

## General Guidelines

- Always prefer using MCP servers when the task matches their capabilities
- Check that MCP servers are properly configured before attempting to use them
- If an MCP server is not available, provide alternative solutions
- When using Playwright MCP, avoid screenshot operations as image files are not supported in the current plan