# Git MCP Server

## Quick Start
```bash
# GitHub Copilot configuration
{
  "servers": {
    "git": {
      "command": "npx",
      "args": ["@cyanheads/git-mcp-server"]
    }
  }
}
```

## Prerequisites
- Node.js 20+ and npm
- Git installed and accessible in system PATH
- VS Code with GitHub Copilot extension
- Access to Copilot (with MCP servers policy enabled for organization/enterprise users)

## Installation

### For GitHub Copilot in VS Code

Create or edit `.vscode/mcp.json` in your project root:

#### Basic Configuration
```json
{
  "servers": {
    "git": {
      "command": "npx",
      "args": ["@cyanheads/git-mcp-server"],
      "env": {
        "MCP_LOG_LEVEL": "info"
      }
    }
  }
}
```

#### With Commit Signing
```json
{
  "servers": {
    "git": {
      "command": "npx", 
      "args": ["@cyanheads/git-mcp-server"],
      "env": {
        "MCP_LOG_LEVEL": "info",
        "GIT_SIGN_COMMITS": "true"
      }
    }
  }
}
```

## Features

- **Complete Git Operations**: Clone, commit, push, pull, branch, merge, rebase
- **Repository Management**: Status, logs, diffs, tags, stash operations
- **Advanced Workflows**: Worktree management, cherry-picking, conflict resolution
- **Safety Features**: Confirmation required for destructive operations
- **Commit Signing**: GPG/SSH signing support for verified commits
- **Session Management**: Persistent working directory across operations

## Example Prompts

```
"Show me the current git status and recent commits"
"Create a new feature branch called 'user-authentication' and switch to it"
"Commit all staged changes with message 'Add user login functionality'"
"Show me the diff between the current branch and main"
"Merge the feature branch into main and push changes"
"Create a new tag 'v1.2.0' for the current commit"
```

## Common Use Cases

- **Development Workflow**: Branch management, committing, and merging changes
- **Code Review**: Viewing diffs, commit history, and file changes
- **Release Management**: Tagging versions and managing releases
- **Collaboration**: Pulling changes, resolving conflicts, and pushing updates

## Available Tools

### Repository & Staging
- `git_init`: Initialize new repository
- `git_clone`: Clone remote repositories  
- `git_add`: Stage changes for commit
- `git_status`: Check working directory status
- `git_clean`: Remove untracked files (requires confirmation)

### Committing & History
- `git_commit`: Create commits with conventional messages
- `git_log`: View commit history with filtering
- `git_diff`: Show changes between commits/branches/working tree
- `git_show`: Inspect Git objects (commits, tags)

### Branching & Merging  
- `git_branch`: List, create, delete, rename branches
- `git_checkout`: Switch branches or commits
- `git_merge`: Merge branches together
- `git_rebase`: Re-apply commits on different base
- `git_cherry_pick`: Apply specific commits from other branches

### Remote Operations
- `git_remote`: Manage remote repository connections
- `git_fetch`: Download objects and refs from remote
- `git_pull`: Fetch and integrate with another repository
- `git_push`: Update remote refs with local changes

### Advanced Workflows
- `git_tag`: Create, list, or delete tags
- `git_stash`: Temporarily store modified files
- `git_worktree`: Manage multiple working trees
- `git_set_working_dir`: Set persistent working directory for session
- `git_wrapup_instructions`: Get standard workflow for finalizing changes

## Configuration Options

### Environment Variables
- `MCP_LOG_LEVEL`: Logging level (debug, info, warning, error) - default: "info"
- `GIT_SIGN_COMMITS`: Enable commit signing ("true"/"false") - default: "false"
- `MCP_TRANSPORT_TYPE`: Transport mechanism (stdio/http) - default: "stdio"

### Advanced Configuration Example
```json
{
  "servers": {
    "git": {
      "command": "npx",
      "args": ["@cyanheads/git-mcp-server"],
      "env": {
        "MCP_LOG_LEVEL": "debug",
        "GIT_SIGN_COMMITS": "true"
      }
    }
  }
}
```

## Working Directory Management

The Git server supports setting a persistent working directory for your session:

### Set Working Directory
```
Ask Copilot: "Set the working directory to /path/to/my/project"
```

### Repository Operations
Once a working directory is set, all Git operations will be performed in that context:
```
Ask Copilot: "Show git status" # Will show status for the set working directory
Ask Copilot: "Create a new branch called 'feature-x'" # Will create branch in the set repo
```

## Security Features

### Destructive Operation Protection
Commands like `git clean` and `git reset --hard` require explicit confirmation and cannot be executed accidentally.

### Path Sanitization
All file paths and repository paths are validated and sanitized to prevent directory traversal attacks.

### Commit Signing
When enabled, commits can be automatically signed with GPG or SSH keys for authenticity verification.

## Troubleshooting

### Common Issues

1. **Git Not Found**: Ensure Git is installed and in system PATH
   ```bash
   git --version  # Should show Git version
   which git      # Should show Git location
   ```

2. **NPX Package Installation Failed**: Check internet connection and npm cache
   ```bash
   npm cache clean --force
   npx @cyanheads/git-mcp-server --help
   ```

3. **Working Directory Not Set**: Set working directory before Git operations
   ```
   Ask Copilot: "Set working directory to ."
   ```

4. **Commit Signing Failures**: Verify GPG/SSH key setup if signing enabled
   ```bash
   git config --global user.signingkey  # Check signing key
   git config --global commit.gpgsign   # Check signing config
   ```

### Debug Mode
Enable debug logging for troubleshooting:
```json
{
  "servers": {
    "git": {
      "env": {
        "MCP_LOG_LEVEL": "debug"
      }
    }
  }
}
```

## Best Practices

### Workflow Organization
1. **Set Working Directory**: Always set the working directory first
2. **Check Status**: Review repository status before making changes
3. **Stage Selectively**: Use `git_add` to stage specific files
4. **Write Clear Commits**: Use descriptive commit messages
5. **Review Before Push**: Check diffs and logs before pushing

### Safety Guidelines
- Always review changes with `git_diff` before committing
- Use `git_status` to understand current repository state
- Test destructive operations in a separate branch first
- Keep commit messages descriptive and conventional

## Performance Considerations

### Large Repositories
- Use `git_log` with limits for large repositories
- Consider shallow clones for large remote repositories
- Use `git_fetch` instead of `git_pull` when you only need to check for changes

### Network Operations
- `git_clone`, `git_fetch`, `git_pull`, and `git_push` operations depend on network connectivity
- Large repositories may take significant time to clone
- Consider using SSH keys for authentication to avoid repeated credential prompts

## Related Servers
- **Filesystem**: For local file operations alongside Git
- **Sequential Thinking**: For planning complex Git workflows
- **Playwright**: For testing deployed applications after Git operations

## Links
- [GitHub Repository](https://github.com/cyanheads/git-mcp-server)
- [NPM Package](https://www.npmjs.com/package/@cyanheads/git-mcp-server)
- [Git Documentation](https://git-scm.com/doc)

## Usage Examples

### Development Workflow
```
"Initialize a new Git repository in the current directory"
"Add all JavaScript files to staging and commit with message 'Initial project setup'"
"Create and switch to a new branch called 'feature/user-profiles'"
"Show me what files have changed since the last commit"
```

### Release Management
```
"Create a new tag 'v2.1.0' for the current commit"
"Show me all tags in this repository"
"Generate release notes by comparing current branch with the previous tag"
```

### Collaboration
```
"Fetch the latest changes from origin without merging"
"Show me the difference between my branch and origin/main"
"Merge the main branch into my current feature branch"
"Push my current branch to origin and set up tracking"
```

### Repository Analysis
```
"Show me the commit history for the past week"
"Who was the last person to modify the package.json file?"
"Show me all commits that touched files in the src/components directory"
"Display the current repository's remote URLs"
```