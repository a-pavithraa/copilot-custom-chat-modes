# Filesystem MCP Server

## Quick Start
```bash
# GitHub Copilot configuration  
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"]
    }
  }
}
```

## Prerequisites
- Node.js 18+ and npm
- VS Code with GitHub Copilot extension
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### For GitHub Copilot in VS Code

Create or edit `.vscode/mcp.json` in your project root:

#### Basic Configuration (Current Directory Access)
```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    }
  }
}
```

#### Multiple Directory Access
```json
{
  "servers": {
    "filesystem": {
      "command": "npx", 
      "args": [
        "-y", 
        "@modelcontextprotocol/server-filesystem",
        ".",
        "/home/user/projects",
        "/home/user/documents"
      ]
    }
  }
}
```

#### Read-Only Mode
```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem", 
        "--readonly",
        "."
      ]
    }
  }
}
```

## Features

- **File Operations**: Read, write, edit, and move files
- **Directory Management**: Create, list, and delete directories
- **File Search**: Search for files by name or pattern
- **Metadata Access**: Get file information and statistics
- **Directory Tree**: Generate hierarchical directory structures
- **Access Control**: Restrict access to specific directories
- **Media Support**: Read images and other media files

## Example Prompts

```
"Read the contents of package.json and analyze the dependencies"
"Create a new directory called 'components' in the src folder"
"Search for all .js files in the project that contain 'useState'"
"Write a new README.md file with project documentation"
"Move all .test.js files to a tests directory"
"Show me the directory tree structure of this project"
```

## Common Use Cases

- **File Analysis**: Read and analyze source code, configuration files, and documentation
- **Project Management**: Create directories, organize files, and manage project structure
- **Code Search**: Find files containing specific patterns or functions
- **File Generation**: Create new files with generated content

## Available Tools

The Filesystem MCP server provides these tools:

### File Operations
- `read_text_file`: Read text content from files
- `read_media_file`: Read images and media files (returns as base64)
- `read_multiple_files`: Read multiple files in a single operation
- `write_file`: Create new files or overwrite existing ones
- `edit_file`: Make targeted edits to existing files

### Directory Operations  
- `create_directory`: Create new directories
- `list_directory`: List contents of directories
- `directory_tree`: Generate hierarchical directory structure
- `list_allowed_directories`: Show accessible directories

### File Management
- `move_file`: Move or rename files and directories
- `search_files`: Search for files by name or pattern
- `get_file_info`: Get file metadata (size, permissions, dates)

## Configuration Options

### Command Line Arguments
- Directory paths: Specify which directories to allow access to
- `--readonly`: Enable read-only mode (prevents write operations)
- `--help`: Show help information

### Security Configuration
```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "--readonly",
        "./src",
        "./docs",
        "./config"
      ]
    }
  }
}
```

## Directory Access Control

### Best Practices
1. **Principle of Least Privilege**: Only grant access to directories that are needed
2. **Use Relative Paths**: Use `.` for current directory access when appropriate
3. **Read-Only Mode**: Use `--readonly` flag when write access isn't needed
4. **Explicit Directory Lists**: Specify exact directories instead of broad access

### Example Configurations

#### Development Environment
```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", ".", "./tests", "./docs"]
    }
  }
}
```

#### Production/CI Environment
```json
{
  "servers": {
    "filesystem": {
      "command": "npx", 
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "--readonly", "./src", "./dist"]
    }
  }
}
```

## Troubleshooting

### Common Issues

1. **Permission Denied**: Check file/directory permissions
   ```bash
   ls -la /path/to/directory
   chmod 755 /path/to/directory  # If needed
   ```

2. **Directory Not Accessible**: Verify the directory is in the allowed list
3. **NPX Package Not Found**: Ensure stable internet connection
   ```bash
   npm cache clean --force
   npx -y @modelcontextprotocol/server-filesystem --help
   ```

4. **Read-Only Errors**: Check if server is configured with `--readonly` flag

### Debug Mode
Enable verbose logging by checking VS Code's Output panel and selecting "MCP" from the dropdown.

### Testing Access
Test server functionality with simple operations:
```
Ask Copilot: "List the contents of the current directory"
Ask Copilot: "Show me the file information for package.json"
```

## Security Considerations

### File Access Security
- Only grant access to necessary directories
- Use read-only mode when write access isn't required
- Avoid granting access to system directories
- Regular audit of allowed directories

### Sensitive File Protection  
- Exclude directories containing secrets (`.env`, `.git`, etc.)
- Use `.gitignore` patterns to identify sensitive files
- Consider separate filesystem servers for different access levels

## Performance Considerations

### Large File Handling
- Server handles text files efficiently
- Media files are base64 encoded (increases size)
- Consider file size limits for very large files

### Directory Traversal
- Deep directory structures may impact performance
- Use specific directory paths instead of broad access
- Consider caching for frequently accessed directories

## Related Servers
- **Playwright**: For web-based file operations
- **Sequential Thinking**: For complex file management workflows  
- **GitLab File Fetcher**: For remote repository files

## Links
- [GitHub Repository](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)
- [NPM Package](https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem)
- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)

## Usage Examples

### Project Analysis
```
"Analyze the project structure and identify the main components"
"Read all package.json files in the project and compare dependencies"
"Find all TODO comments in the codebase"
```

### File Management
```
"Organize all image files into an assets/images directory"  
"Create a backup of all configuration files"
"Generate a project documentation file based on the README files"
```

### Code Generation
```
"Create component files based on the existing patterns in src/components"
"Generate test files for all the modules in the utils directory"
"Create a new feature branch structure with necessary directories"
```