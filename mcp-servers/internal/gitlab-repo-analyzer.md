# GitLab Repository Analyzer MCP Server

## Quick Start
```bash
# GitHub Copilot configuration
{
  "servers": {
    "gitlab-repo-analyzer": {
      "command": "./path/to/gitlab-repo-analyzer",
      "args": ["--port", "8081"]
    }
  }
}
```

## Prerequisites
- GitLab instance (Cloud or self-hosted)
- GitLab personal access token with API access
- VS Code with GitHub Copilot extension
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### Step 1: Set Up the Golang Server

#### macOS/Linux
```bash
# The project should be cloned in .vscode/ directory
# If not already cloned, clone the internal repository:
# git clone <your-internal-gitlab-repo-analyzer-repo> .vscode/gitlab-repo-analyzer

# Navigate to project and build the server
cd .vscode/gitlab-repo-analyzer

# Pull latest changes
git pull origin master

# Build the server
go build -o gitlab-repo-analyzer ./cmd/server
```

#### Windows
```cmd
REM The project should be cloned in .vscode\ directory
REM If not already cloned, clone the internal repository:
REM git clone <your-internal-gitlab-repo-analyzer-repo> .vscode\gitlab-repo-analyzer

REM Navigate to project and build the server
cd .vscode\gitlab-repo-analyzer

REM Pull latest changes
git pull origin master

REM Build the server
go build -o gitlab-repo-analyzer.exe .\cmd\server
```

### Step 2: Set Up Authentication

Create a GitLab personal access token:
1. Go to GitLab → User Settings → Access Tokens
2. Create token with scopes: `api`, `read_repository`, `read_user`
3. Copy the generated token

### Step 3: Configure GitHub Copilot

Create or edit `.vscode/mcp.json` in your project root:

#### macOS/Linux Configuration
```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "gitlab-url",
      "description": "GitLab instance URL (e.g., https://gitlab.com)"
    },
    {
      "type": "promptString",
      "id": "gitlab-token", 
      "description": "GitLab personal access token",
      "password": true
    }
  ],
  "servers": {
    "gitlab-repo-analyzer": {
      "command": "./.vscode/gitlab-repo-analyzer/gitlab-repo-analyzer",
      "args": ["--stdio"],
      "env": {
        "GITLAB_URL": "${input:gitlab-url}",
        "GITLAB_TOKEN": "${input:gitlab-token}",
        "LOG_LEVEL": "info"
      }
    }
  }
}
```

#### Windows Configuration
```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "gitlab-url",
      "description": "GitLab instance URL (e.g., https://gitlab.com)"
    },
    {
      "type": "promptString",
      "id": "gitlab-token", 
      "description": "GitLab personal access token",
      "password": true
    }
  ],
  "servers": {
    "gitlab-repo-analyzer": {
      "command": ".\\.vscode\\gitlab-repo-analyzer\\gitlab-repo-analyzer.exe",
      "args": ["--stdio"],
      "env": {
        "GITLAB_URL": "${input:gitlab-url}",
        "GITLAB_TOKEN": "${input:gitlab-token}",
        "LOG_LEVEL": "info"
      }
    }
  }
}
```

### Step 4: Restart VS Code
Restart VS Code to apply the MCP server configuration.

## Features

- **Repository Discovery**: Find and analyze GitLab repositories
- **File Tree Analysis**: Understand repository structure and organization
- **Architecture Detection**: Identify architectural patterns and technologies
- **Dependency Analysis**: Analyze project dependencies and relationships
- **Code Quality Metrics**: Assess code quality and technical debt
- **Documentation Analysis**: Evaluate documentation completeness

## Example Prompts

```
"Analyze the architecture of the 'backend-service' repository and identify its main components"
"What technologies and frameworks are used in the 'frontend-app' repository?"
"Assess the code quality and identify potential technical debt in repository 'legacy-system'"
"Analyze the dependencies in 'microservice-auth' and identify any security vulnerabilities"
"Review the documentation completeness for the 'api-gateway' repository"
"Analyze 'old-monolith' repository and suggest a migration strategy to microservices"
```

## Common Use Cases

- **Architecture Analysis**: Understand repository structure and design patterns
- **Technology Assessment**: Identify frameworks, libraries, and tech stack
- **Quality Evaluation**: Assess code quality and technical debt
- **Migration Planning**: Plan modernization and refactoring strategies

## Configuration Options

### Environment Variables
- `GITLAB_URL`: GitLab instance URL (required)
- `GITLAB_TOKEN`: Personal access token (required)
- `LOG_LEVEL`: Logging level (debug, info, warn, error)
- `CACHE_TTL`: Analysis cache time-to-live in minutes (default: 60)
- `MAX_FILE_SIZE`: Maximum file size to analyze in KB (default: 1024)
- `EXCLUDED_PATHS`: Comma-separated list of paths to exclude from analysis

### Command Line Arguments
- `--stdio`: Use standard input/output for MCP communication
- `--port <port>`: Run as HTTP server on specified port
- `--config <file>`: Path to configuration file
- `--verbose`: Enable verbose logging

### Advanced Configuration Example

#### macOS/Linux
```json
{
  "servers": {
    "gitlab-repo-analyzer": {
      "command": "./.vscode/gitlab-repo-analyzer/gitlab-repo-analyzer",
      "args": ["--stdio", "--config", "./.vscode/gitlab-repo-analyzer/config.yaml"],
      "env": {
        "GITLAB_URL": "https://gitlab.company.com",
        "GITLAB_TOKEN": "your-token",
        "LOG_LEVEL": "debug",
        "CACHE_TTL": "30",
        "EXCLUDED_PATHS": "node_modules,vendor,dist,build"
      }
    }
  }
}
```

#### Windows
```json
{
  "servers": {
    "gitlab-repo-analyzer": {
      "command": ".\\.vscode\\gitlab-repo-analyzer\\gitlab-repo-analyzer.exe",
      "args": ["--stdio", "--config", ".\\.vscode\\gitlab-repo-analyzer\\config.yaml"],
      "env": {
        "GITLAB_URL": "https://gitlab.company.com",
        "GITLAB_TOKEN": "your-token",
        "LOG_LEVEL": "debug",
        "CACHE_TTL": "30",
        "EXCLUDED_PATHS": "node_modules,vendor,dist,build"
      }
    }
  }
}
```

## Available Analysis Types

### Structural Analysis
- File and directory organization
- Module and package structure
- Code complexity metrics
- Architectural patterns detection

### Technology Analysis
- Programming languages used
- Frameworks and libraries
- Build tools and configuration
- Container and deployment setup

### Quality Analysis
- Code duplication detection
- Security vulnerability scanning
- Performance bottleneck identification
- Best practices compliance

### Documentation Analysis
- README completeness
- API documentation coverage
- Code comment density
- Architecture documentation

## Troubleshooting

### Common Issues

1. **GitLab Connection Failed**: Verify URL and token
   ```bash
   curl -H "Authorization: Bearer YOUR_TOKEN" https://gitlab.com/api/v4/user
   ```

2. **Server Binary Not Found**: Ensure the binary is built and path is correct
   ```bash
   ls -la ./gitlab-repo-analyzer
   chmod +x ./gitlab-repo-analyzer
   ```

3. **Permission Denied**: Check GitLab token scopes and repository access
4. **Analysis Timeout**: Increase timeout or reduce repository scope

### Debug Mode
Enable verbose logging:
```json
{
  "env": {
    "LOG_LEVEL": "debug"
  }
}
```

## Performance Considerations

### Caching
- Repository analysis results are cached to improve performance
- Cache TTL can be configured via `CACHE_TTL` environment variable
- Clear cache by restarting the server

### Resource Usage
- Large repositories may require increased memory allocation
- File size limits can be configured to control processing time
- Parallel analysis can be enabled for multiple repositories

## Security Considerations

- Store GitLab tokens securely using VS Code's secret storage
- Use read-only tokens when possible
- Implement IP restrictions for GitLab tokens
- Regularly rotate access tokens

## Related Servers
- **GitLab File Cache**: For efficient file content retrieval
- **Sequential Thinking**: For complex repository analysis workflows
- **Confluence**: For documenting analysis results

## Links
- Internal GitLab Repository: `<your-internal-repo-link>`
- GitLab API Documentation: https://docs.gitlab.com/ee/api/
- GitLab Access Tokens: https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html

## Development Notes

### Building from Source

#### macOS/Linux
```bash
# Install dependencies
go mod download

# Run tests
go test ./...

# Build binary
go build -o gitlab-repo-analyzer ./cmd/server

# Build for different platforms
GOOS=linux GOARCH=amd64 go build -o gitlab-repo-analyzer-linux ./cmd/server
GOOS=windows GOARCH=amd64 go build -o gitlab-repo-analyzer.exe ./cmd/server
GOOS=darwin GOARCH=amd64 go build -o gitlab-repo-analyzer-darwin ./cmd/server
```

#### Windows
```cmd
REM Install dependencies
go mod download

REM Run tests
go test .\...

REM Build binary
go build -o gitlab-repo-analyzer.exe .\cmd\server

REM Build for different platforms
set GOOS=linux& set GOARCH=amd64& go build -o gitlab-repo-analyzer-linux .\cmd\server
set GOOS=darwin& set GOARCH=amd64& go build -o gitlab-repo-analyzer-darwin .\cmd\server
set GOOS=windows& set GOARCH=amd64& go build -o gitlab-repo-analyzer-windows.exe .\cmd\server
```

### Configuration File Format (config.yaml)
```yaml
gitlab:
  url: "https://gitlab.company.com"
  token: "your-token"
analysis:
  cache_ttl: 60
  max_file_size: 1024
  excluded_paths:
    - "node_modules"
    - "vendor" 
    - "dist"
    - "build"
logging:
  level: "info"
```