# Sequential Thinking MCP Server

## Quick Start
```bash
# GitHub Copilot configuration
{
  "servers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

## Prerequisites
- Node.js 18+ and npm
- VS Code with GitHub Copilot extension
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### Method 1: NPX Installation (Recommended)

Create or edit `.vscode/mcp.json` in your project root:

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

### Method 2: Docker Installation

```json
{
  "servers": {
    "sequential-thinking": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "mcp/sequentialthinking"
      ]
    }
  }
}
```

To build the Docker image locally:
```bash
docker build -t mcp/sequentialthinking -f src/sequentialthinking/Dockerfile .
```

### Step 3: Restart VS Code

Restart VS Code to apply the MCP server configuration.

## Features

- **Dynamic Problem-Solving**: Break down complex problems into manageable, sequential steps
- **Reflective Thinking**: Revise and refine thoughts dynamically during the problem-solving process
- **Alternative Reasoning**: Branch into different reasoning paths when needed
- **Solution Verification**: Generate hypotheses and verify them through structured thinking
- **Iterative Refinement**: Continuously improve solutions through multiple thinking cycles

## How It Works

The Sequential Thinking MCP server provides a structured framework for AI to:

1. **Analyze Problems**: Break complex questions into smaller, manageable components
2. **Think Step-by-Step**: Process information sequentially with clear reasoning chains
3. **Revise and Refine**: Adjust thinking based on new insights or contradictions
4. **Generate Hypotheses**: Form testable solutions and verify them systematically
5. **Branch Reasoning**: Explore alternative approaches when the current path seems insufficient

## Example Prompts

```
"Design a distributed caching algorithm with sequential thinking for step-by-step reasoning"
"Plan the architecture for a microservices system using sequential thinking to consider all aspects"
"Debug this performance issue using sequential thinking to systematically eliminate possibilities" 
"Analyze these user requirements using sequential thinking to identify potential conflicts and gaps"
"Compare database options for our use case using sequential thinking to weigh pros and cons"
"Review this pull request using sequential thinking to check for security, performance, and maintainability"
```

## Common Use Cases

- **Algorithm Design**: Break down complex algorithms into manageable steps
- **Architecture Planning**: Systematic approach to system design
- **Debugging**: Methodical problem-solving for complex issues
- **Decision Making**: Structured comparison of options and trade-offs

## Tool Parameters

The Sequential Thinking tool accepts these parameters:

- `thought`: Current thinking step or analysis
- `nextThoughtNeeded`: Boolean indicating if more thinking is required
- `thoughtNumber`: Current step number in the sequence
- `totalThoughts`: Estimated total number of thinking steps needed
- `isRevision`: Boolean indicating if this thought revises previous thinking
- `revisesThought`: Which thought number is being reconsidered
- `branchFromThought`: Starting point for alternative reasoning paths
- `branchId`: Identifier for the current reasoning branch

## Configuration

### Basic Configuration
The server works out of the box with no additional configuration required.

### Advanced Configuration
For custom setups, you can modify the server behavior through environment variables or command-line arguments (refer to the server documentation for specific options).

## Benefits for Development Workflows

1. **Systematic Problem Solving**: Ensures no critical aspects are overlooked
2. **Transparent Reasoning**: Provides clear audit trails for decisions
3. **Iterative Improvement**: Allows for continuous refinement of solutions
4. **Error Reduction**: Systematic approach reduces logical inconsistencies
5. **Knowledge Transfer**: Documented thinking process helps team understanding

## Troubleshooting

### Common Issues

1. **NPX Package Not Found**: Ensure you have a stable internet connection for package installation
   ```bash
   npm cache clean --force
   npx -y @modelcontextprotocol/server-sequential-thinking --help
   ```

2. **Docker Image Issues**: Build the image locally if pull fails
   ```bash
   docker build -t mcp/sequentialthinking -f src/sequentialthinking/Dockerfile .
   ```

3. **Server Not Responding**: Check VS Code output panel for MCP server logs

### Debug Mode
Enable verbose logging by checking VS Code's Output panel and selecting "MCP" from the dropdown.

## Integration Tips

### Best Practices
- Use for complex, multi-step problems that benefit from structured thinking
- Allow the server to complete full thinking cycles before expecting final answers
- Combine with other MCP servers for comprehensive problem-solving workflows

### Performance Considerations
- Sequential thinking adds processing time but improves solution quality
- Best suited for complex problems where accuracy is more important than speed
- Consider using for planning phases rather than simple queries

## Related Servers
- **Playwright**: For systematic testing workflows
- **Figma**: For structured design-to-code processes
- **Confluence**: For complex documentation and knowledge management tasks

## Links
- [GitHub Repository](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)
- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [NPM Package](https://www.npmjs.com/package/@modelcontextprotocol/server-sequential-thinking)