# GitHub Copilot Repository Instructions

Custom chat modes and MCP server configurations for GitHub Copilot. Setup guide: [MCP Servers Documentation](../mcp-servers/README.md).

## MCP Servers - Install When Needed

**🗃️ Filesystem**: Local file operations - *Install if you want direct file management*  
**🔄 Git**: Version control integration - *Install if you want automated Git workflows*  
**🌐 Playwright**: Browser automation & testing - *Install for E2E testing* ([repo](https://github.com/microsoft/playwright-mcp))  
**🧠 Sequential Thinking**: Step-by-step analysis - *Install for complex problem solving* ([repo](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking))  
**🎨 Figma**: Design to code conversion - *Install for Figma design analysis* ([repo](https://github.com/GLips/Figma-Context-MCP))  
**📚 Confluence**: Wiki search - *Install if you use Confluence docs* ([repo](https://github.com/modelcontextprotocol/servers))  
**🔍 GitLab Analyzer**: Repository analysis - *Install for codebase insights* ([repo](https://github.com/zereight/gitlab-mcp))  
**📁 GitLab File Fetcher**: Smart file access - *Install for GitLab file operations* ([repo](https://github.com/zereight/gitlab-mcp))

## Installation Triggers

**MUST suggest MCP installation when users say:**

**Figma**: "analyze figma design", "compare figma with implementation", "generate code from figma", "figma design system"  
**Playwright**: "test website", "browser automation", "e2e testing", "create test plan", "web scraping"  
**Sequential Thinking**: "step by step", "break down problem", "complex analysis", "architectural decision"  
**Confluence**: "search confluence", "team wiki", "internal docs", "documentation search"  
**GitLab Analyzer**: "analyze repository", "codebase analysis", "code quality review", "project structure"  
**GitLab File Fetcher**: "fetch gitlab file", "compare files across repos", "gitlab file operations"

**Response**: "To [capability], you'll need [Server Name] MCP. Install by updating .vscode/mcp.json?"

## Usage Guidelines

- **Prefer MCP servers** when task matches capabilities
- **Check configuration** before using servers  
- **Combine servers** for complex workflows
- **Provide alternatives** if server unavailable

## Installation Guidelines

**External Servers (NPX)**: Only modify `.vscode/mcp.json` - no other files  
**Internal Servers (Go)**: Clone to `.vscode/`, build binaries, update `mcp.json` paths