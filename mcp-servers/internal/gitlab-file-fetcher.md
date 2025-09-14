# GitLab File Fetcher MCP Server

## Quick Start
```bash
# GitHub Copilot configuration
{
  "servers": {
    "gitlab-file-fetcher": {
      "command": "./path/to/gitlab-file-fetcher",
      "args": ["--stdio"]
    }
  }
}
```

## Prerequisites
- GitLab instance (Cloud or self-hosted)
- GitLab personal access token with read access
- VS Code with GitHub Copilot extension
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### Step 1: Set Up the Golang Server

#### macOS/Linux
```bash
# The project should be cloned in .vscode/ directory
# If not already cloned, clone the internal repository:
# git clone <your-internal-gitlab-file-fetcher-repo> .vscode/gitlab-file-fetcher

# Navigate to project and build the server
cd .vscode/gitlab-file-fetcher

# Pull latest changes
git pull origin master

# Build the server
go build -o gitlab-file-fetcher ./cmd/server
```

#### Windows
```cmd
REM The project should be cloned in .vscode\ directory
REM If not already cloned, clone the internal repository:
REM git clone <your-internal-gitlab-file-fetcher-repo> .vscode\gitlab-file-fetcher

REM Navigate to project and build the server
cd .vscode\gitlab-file-fetcher

REM Pull latest changes
git pull origin master

REM Build the server
go build -o gitlab-file-fetcher.exe .\cmd\server
```

### Step 2: Set Up Authentication

Create a GitLab personal access token:
1. Go to GitLab → User Settings → Access Tokens
2. Create token with scopes: `read_api`, `read_repository`
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
    "gitlab-file-fetcher": {
      "command": "./.vscode/gitlab-file-fetcher/gitlab-file-fetcher",
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
    "gitlab-file-fetcher": {
      "command": ".\\.vscode\\gitlab-file-fetcher\\gitlab-file-fetcher.exe",
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

- **File Retrieval**: Fetch individual files from GitLab repositories
- **Batch Operations**: Retrieve multiple files in a single request
- **Branch Support**: Access files from any branch or tag
- **Content Caching**: Cache frequently accessed files for faster retrieval
- **Binary Detection**: Automatically handle binary vs text files
- **Path Navigation**: Browse repository directory structures

## Example Prompts

```
"Fetch and analyze the main.go file from the 'backend-service' repository"
"Get the docker-compose.yml file from 'infrastructure' repo and review the configuration"
"Fetch the README.md and API documentation from the 'api-gateway' repository"
"Get all .js files from the 'frontend/src' directory and analyze the code structure"
"Compare the staging and production config files from the 'config' repository"
"Fetch all security-related configuration files and review for vulnerabilities"
```

## Common Use Cases

- **Code Analysis**: Retrieve and analyze source code files
- **Configuration Management**: Access and review configuration files
- **Documentation Access**: Fetch README files and documentation
- **Multi-File Operations**: Batch retrieve files for analysis

## Configuration Options

### Environment Variables
- `GITLAB_URL`: GitLab instance URL (required)
- `GITLAB_TOKEN`: Personal access token (required)
- `LOG_LEVEL`: Logging level (debug, info, warn, error)
- `CACHE_TTL`: File cache time-to-live in minutes (default: 30)
- `MAX_FILE_SIZE`: Maximum file size to fetch in MB (default: 10)
- `CONCURRENT_REQUESTS`: Number of concurrent GitLab API requests (default: 5)

### Command Line Arguments
- `--stdio`: Use standard input/output for MCP communication
- `--port <port>`: Run as HTTP server on specified port
- `--config <file>`: Path to configuration file
- `--cache-dir <path>`: Directory for file caching
- `--verbose`: Enable verbose logging

### Advanced Configuration Example

#### macOS/Linux
```json
{
  "servers": {
    "gitlab-file-fetcher": {
      "command": "./.vscode/gitlab-file-fetcher/gitlab-file-fetcher",
      "args": ["--stdio", "--cache-dir", "./gitlab-cache"],
      "env": {
        "GITLAB_URL": "https://gitlab.company.com",
        "GITLAB_TOKEN": "your-token",
        "LOG_LEVEL": "info",
        "CACHE_TTL": "15",
        "MAX_FILE_SIZE": "5",
        "CONCURRENT_REQUESTS": "3"
      }
    }
  }
}
```

#### Windows
```json
{
  "servers": {
    "gitlab-file-fetcher": {
      "command": ".\\.vscode\\gitlab-file-fetcher\\gitlab-file-fetcher.exe",
      "args": ["--stdio", "--cache-dir", ".\\gitlab-cache"],
      "env": {
        "GITLAB_URL": "https://gitlab.company.com",
        "GITLAB_TOKEN": "your-token",
        "LOG_LEVEL": "info",
        "CACHE_TTL": "15",
        "MAX_FILE_SIZE": "5",
        "CONCURRENT_REQUESTS": "3"
      }
    }
  }
}
```

## Available Operations

### Single File Operations
- `get_file`: Retrieve a specific file by path
- `get_file_content`: Get file content with metadata
- `file_exists`: Check if a file exists in the repository
- `get_file_info`: Get file metadata (size, type, last modified)

### Batch Operations
- `get_files`: Retrieve multiple files by path list
- `get_directory`: Get all files in a directory
- `search_files`: Search for files by pattern or content
- `get_changed_files`: Get files modified in recent commits

### Repository Operations
- `list_branches`: List all branches in the repository
- `list_tags`: List all tags in the repository
- `get_repository_info`: Get repository metadata

## API Usage Examples

### Fetching a Single File
```json
{
  "method": "get_file",
  "params": {
    "repository": "group/project",
    "file_path": "src/main.go",
    "branch": "main"
  }
}
```

### Batch File Retrieval
```json
{
  "method": "get_files",
  "params": {
    "repository": "group/project",
    "file_paths": [
      "src/config.yaml",
      "docker-compose.yml",
      "README.md"
    ],
    "branch": "develop"
  }
}
```

### Directory Listing
```json
{
  "method": "get_directory",
  "params": {
    "repository": "group/project",
    "directory_path": "src/api",
    "recursive": true,
    "file_pattern": "*.go"
  }
}
```

## Caching Strategy

### File Caching
- Files are cached locally to reduce API calls
- Cache keys include repository, path, branch, and commit SHA
- Cache automatically invalidates based on TTL or file changes

### Cache Management
- Clear cache: Delete files in cache directory
- Cache statistics: Available through management API
- Selective cache clearing: By repository or file pattern

### Performance Benefits
- Reduced GitLab API rate limit usage
- Faster subsequent file access
- Offline access to previously fetched files

## Troubleshooting

### Common Issues

1. **GitLab Connection Failed**: Verify URL and token
   ```bash
   curl -H "Authorization: Bearer YOUR_TOKEN" \
        "https://gitlab.com/api/v4/projects"
   ```

2. **File Not Found**: Check repository path and branch
   ```bash
   ./gitlab-file-fetcher --test-connection
   ```

3. **Rate Limit Exceeded**: Reduce concurrent requests or increase cache TTL
4. **Large File Issues**: Increase `MAX_FILE_SIZE` or use streaming

### Debug Mode
Enable detailed logging:
```json
{
  "env": {
    "LOG_LEVEL": "debug"
  }
}
```

### Health Check
```bash
# Test server health
curl http://localhost:8080/health

# Test GitLab connectivity
./gitlab-file-fetcher --test-gitlab-connection
```

## Performance Optimization

### Request Batching
- Batch multiple file requests to reduce API calls
- Use directory operations instead of individual file requests
- Implement request queuing for large operations

### Caching Strategy
- Tune cache TTL based on file change frequency
- Use repository-specific cache policies
- Implement cache warming for frequently accessed files

### Memory Management
- Stream large files instead of loading into memory
- Implement file size limits to prevent memory exhaustion
- Use compression for text file caching

## Security Considerations

### Access Control
- Use read-only GitLab tokens
- Implement path restrictions to prevent unauthorized access
- Log all file access for audit purposes

### Data Protection
- Encrypt cached files containing sensitive data
- Implement secure token storage
- Use HTTPS for all GitLab API communications

### Rate Limiting
- Respect GitLab API rate limits
- Implement exponential backoff for failed requests
- Use connection pooling for better performance

## Related Servers
- **GitLab Repository Analyzer**: For comprehensive repository analysis
- **Sequential Thinking**: For complex file analysis workflows
- **Playwright**: For testing configuration changes

## Links
- Internal GitLab Repository: `<your-internal-repo-link>`
- GitLab API Documentation: https://docs.gitlab.com/ee/api/
- GitLab Personal Access Tokens: https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html

## Development Notes

### Building from Source

#### macOS/Linux
```bash
# Install dependencies
go mod download

# Run tests
go test ./...

# Build binary
go build -o gitlab-file-fetcher ./cmd/server

# Build for different platforms
GOOS=linux GOARCH=amd64 go build -o gitlab-file-fetcher-linux ./cmd/server
GOOS=windows GOARCH=amd64 go build -o gitlab-file-fetcher.exe ./cmd/server
GOOS=darwin GOARCH=amd64 go build -o gitlab-file-fetcher-darwin ./cmd/server
```

#### Windows
```cmd
REM Install dependencies
go mod download

REM Run tests
go test .\...

REM Build binary
go build -o gitlab-file-fetcher.exe .\cmd\server

REM Build for different platforms
set GOOS=linux& set GOARCH=amd64& go build -o gitlab-file-fetcher-linux .\cmd\server
set GOOS=darwin& set GOARCH=amd64& go build -o gitlab-file-fetcher-darwin .\cmd\server
set GOOS=windows& set GOARCH=amd64& go build -o gitlab-file-fetcher-windows.exe .\cmd\server
```

### Configuration File Format (config.yaml)
```yaml
gitlab:
  url: "https://gitlab.company.com"
  token: "your-token"
  timeout: "30s"
  
cache:
  ttl: 30
  directory: "./cache"
  max_size: "100MB"

fetcher:
  max_file_size: 10
  concurrent_requests: 5
  retry_attempts: 3
  
logging:
  level: "info"
  file: "gitlab-file-fetcher.log"
```

### Testing
```bash
# Unit tests
go test ./internal/...

# Integration tests (requires GitLab access)
go test -tags=integration ./tests/...

# Load tests
go test -tags=load -timeout=30m ./tests/load/
```