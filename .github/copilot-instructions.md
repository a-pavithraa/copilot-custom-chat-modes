# GitHub Copilot Repository Instructions

This repository contains custom chat modes and MCP server configurations for GitHub Copilot.

## MCP Server Setup

For detailed instructions on setting up MCP (Model Context Protocol) servers with GitHub Copilot, see [MCP Servers Documentation](../mcp-servers/README.md).

Available MCP servers in this repository:
- **Filesystem MCP**: Local file and directory operations
- **Git MCP**: Git version control operations
- **Playwright MCP**: Web automation and testing
- **Sequential Thinking MCP**: Step-by-step problem solving
- **Figma MCP**: Figma design integration
- **Confluence MCP**: Documentation search & management
- **GitLab Analyzer MCP**: Repository architecture analysis
- **GitLab File Fetcher MCP**: File retrieval with caching

## MCP Server Usage Guidelines

When responding to user requests, Copilot should leverage the appropriate MCP servers:

### Filesystem MCP Usage
Use Filesystem MCP for:
- Reading, writing, and editing local files
- Creating and managing directories
- Searching for files by name or pattern
- Getting file metadata and information

**Example prompts that should trigger Filesystem MCP:**
- "Read the package.json file and analyze the dependencies"
- "Create a new components directory in the src folder"
- "Search for all .js files containing 'useState'"
- "Show me the project directory structure"

### Git MCP Usage
Use Git MCP for:
- Version control operations (commit, push, pull, merge)
- Branch management and switching
- Repository status and history
- Tag creation and management

**Example prompts that should trigger Git MCP:**
- "Show me the current git status"
- "Create a new feature branch called 'user-auth'"
- "Commit all changes with message 'Add login functionality'"
- "Show me the commit history for the past week"

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

### Confluence MCP Usage
Use Confluence MCP for:
- Documentation search and retrieval
- Content creation and management
- Knowledge base queries
- Meeting notes management

**Example prompts that should trigger Confluence MCP:**
- "Find our coding standards documentation and summarize the key points"
- "Create a technical design document for the new API feature"
- "Search Confluence for information about our deployment process"
- "Update the API documentation with the new endpoints"

### GitLab Analyzer MCP Usage
Use GitLab Analyzer MCP for:
- Repository architecture analysis
- Technology stack discovery
- Code quality assessment
- Dependency analysis

**Example prompts that should trigger GitLab Analyzer MCP:**
- "Analyze the architecture of the backend-service repository"
- "What technologies are used in the frontend-app repository?"
- "Assess the code quality of the legacy-system repository"
- "Analyze dependencies and identify security vulnerabilities"

### GitLab File Fetcher MCP Usage
Use GitLab File Fetcher MCP for:
- Retrieving specific files from GitLab repositories
- Batch file operations
- Configuration file analysis
- Multi-repository file comparison

**Example prompts that should trigger GitLab File Fetcher MCP:**
- "Fetch and analyze the main.go file from the backend-service repository"
- "Get the docker-compose.yml file and review the configuration"
- "Compare staging and production config files"
- "Fetch all security-related configuration files"

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
- Use the Filesystem and Git servers for local development workflows
- Combine servers for complex workflows (e.g., Filesystem + Git + Sequential Thinking)
- For comprehensive setup instructions, refer to the [MCP Servers Documentation](../mcp-servers/README.md)

## MCP Server Installation Guidelines

### For External Servers (NPX-based)
When installing external MCP servers (Filesystem, Git, Playwright, Sequential Thinking, Figma, Confluence):
- **ONLY modify the `.vscode/mcp.json` file** - no other files should be created or modified
- **DO NOT create any documentation files** such as `MCP-README.md`, installation guides, or setup instructions
- **DO NOT generate additional configuration files** beyond what's required in `mcp.json`
- **Only provide the JSON configuration** that needs to be added to the existing `mcp.json` file

### For Internal Servers (Golang-based)
When installing internal MCP servers (GitLab Repo Analyzer, GitLab File Fetcher):
- **Clone repositories to `.vscode/` directory** as specified in the documentation
- **Build the Golang binaries** following the build instructions
- **Create necessary local folders and files** as required for the server setup
- **Modify `.vscode/mcp.json`** with the correct local binary paths
- Follow the specific setup instructions in the respective server documentation