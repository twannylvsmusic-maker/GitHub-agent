# GitHub Agent - Model Context Protocol (MCP)

A comprehensive guide and implementation examples for the Model Context Protocol (MCP).

## Table of Contents

- [What is Model Context Protocol (MCP)?](#what-is-model-context-protocol-mcp)
- [Key Features](#key-features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Usage Examples](#usage-examples)
- [API Reference](#api-reference)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## What is Model Context Protocol (MCP)?

The Model Context Protocol (MCP) is an open standard that enables AI assistants to securely connect to data sources and tools. It provides a standardized way for AI models to access external resources while maintaining security and privacy.

### Core Concepts

- **Servers**: MCP servers provide access to data sources and tools
- **Clients**: AI assistants that consume MCP services
- **Protocol**: Standardized communication between servers and clients
- **Security**: Built-in authentication and authorization mechanisms

## Key Features

- 🔒 **Secure**: Built-in authentication and permission management
- 🔌 **Extensible**: Easy to create custom MCP servers
- 🌐 **Standardized**: Open protocol for consistent integration
- ⚡ **Fast**: Efficient communication between clients and servers
- 🛠️ **Tool Access**: Direct access to external APIs and services
- 📊 **Data Sources**: Connect to databases, files, and web services

## Installation

### Prerequisites

- Node.js 16+ (for JavaScript/TypeScript implementations)
- Python 3.8+ (for Python implementations)
- Git (for version control)

### Install MCP Server

```bash
# Install the GitHub MCP server
npm install -g @modelcontextprotocol/server-github

# Or use npx (recommended)
npx @modelcontextprotocol/server-github
```

### Install MCP Client

```bash
# Install MCP client libraries
npm install @modelcontextprotocol/client
```

## Quick Start

### 1. Create MCP Configuration

Create a `mcp.json` configuration file:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_TOKEN": "your_github_token_here"
      }
    }
  }
}
```

### 2. Generate GitHub Token

1. Go to [GitHub Settings > Developer settings > Personal access tokens](https://github.com/settings/tokens)
2. Click "Generate new token (classic)"
3. Select required scopes:
   - `repo` - Full control of private repositories
   - `user` - Update user data
   - `read:org` - Read org and team membership
   - `workflow` - Update GitHub Action workflows
4. Copy the token and add it to your configuration

### 3. Start MCP Server

```bash
# Start the GitHub MCP server
npx @modelcontextprotocol/server-github
```

## Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `GITHUB_TOKEN` | GitHub Personal Access Token | Yes |
| `GITHUB_API_URL` | GitHub API base URL (default: https://api.github.com) | No |

### Server Configuration

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      },
      "timeout": 30000,
      "retries": 3
    }
  }
}
```

## Usage Examples

### Basic Repository Operations

```javascript
// Create a new repository
const repo = await mcp.createRepository({
  name: "my-new-repo",
  description: "A new repository created via MCP",
  private: false
});

// Search repositories
const results = await mcp.searchRepositories({
  query: "user:username language:javascript",
  perPage: 10
});

// Get repository contents
const contents = await mcp.getFileContents({
  owner: "username",
  repo: "repository",
  path: "README.md"
});
```

### File Operations

```javascript
// Create or update a file
await mcp.createOrUpdateFile({
  owner: "username",
  repo: "repository",
  path: "docs/guide.md",
  content: "# Guide\n\nThis is a comprehensive guide...",
  message: "Add comprehensive guide"
});

// Push multiple files
await mcp.pushFiles({
  owner: "username",
  repo: "repository",
  branch: "main",
  files: [
    {
      path: "src/index.js",
      content: "console.log('Hello, MCP!');"
    },
    {
      path: "package.json",
      content: JSON.stringify({
        name: "mcp-example",
        version: "1.0.0"
      }, null, 2)
    }
  ],
  message: "Initial project setup"
});
```

### Issue and Pull Request Management

```javascript
// Create an issue
const issue = await mcp.createIssue({
  owner: "username",
  repo: "repository",
  title: "Feature Request: Add MCP support",
  body: "I would like to add MCP support to this project.",
  labels: ["enhancement", "mcp"]
});

// Create a pull request
const pr = await mcp.createPullRequest({
  owner: "username",
  repo: "repository",
  title: "Add MCP integration",
  head: "feature/mcp-integration",
  base: "main",
  body: "This PR adds MCP integration to the project."
});
```

## API Reference

### Core MCP Functions

#### Repository Management
- `createRepository(options)` - Create a new repository
- `searchRepositories(query)` - Search for repositories
- `getFileContents(owner, repo, path)` - Get file or directory contents

#### File Operations
- `createOrUpdateFile(options)` - Create or update a single file
- `pushFiles(options)` - Push multiple files in one commit
- `deleteFile(options)` - Delete a file from repository

#### Issue Management
- `createIssue(options)` - Create a new issue
- `updateIssue(options)` - Update an existing issue
- `listIssues(options)` - List repository issues
- `addIssueComment(options)` - Add comment to issue

#### Pull Request Management
- `createPullRequest(options)` - Create a new pull request
- `updatePullRequest(options)` - Update an existing pull request
- `listPullRequests(options)` - List repository pull requests
- `mergePullRequest(options)` - Merge a pull request

### Error Handling

```javascript
try {
  const result = await mcp.createRepository({
    name: "my-repo",
    description: "My new repository"
  });
  console.log("Repository created:", result.html_url);
} catch (error) {
  if (error.message.includes("Authentication Failed")) {
    console.error("Please check your GitHub token");
  } else if (error.message.includes("Repository already exists")) {
    console.error("Repository name is already taken");
  } else {
    console.error("Unexpected error:", error.message);
  }
}
```

## Best Practices

### Security
- 🔐 **Never commit tokens** to version control
- 🔄 **Rotate tokens regularly** for security
- 🎯 **Use minimal required scopes** for tokens
- 🔒 **Store tokens in environment variables**

### Performance
- ⚡ **Batch operations** when possible using `pushFiles`
- 📊 **Use pagination** for large result sets
- 🚀 **Implement caching** for frequently accessed data
- ⏱️ **Set appropriate timeouts** for long-running operations

### Code Organization
- 📁 **Organize by functionality** (repos, issues, files)
- 🔄 **Use consistent error handling** patterns
- 📝 **Document your MCP integrations** thoroughly
- 🧪 **Write tests** for your MCP implementations

### Example Project Structure

```
my-mcp-project/
├── src/
│   ├── github/
│   │   ├── repository.js
│   │   ├── issues.js
│   │   └── files.js
│   ├── config/
│   │   └── mcp.js
│   └── utils/
│       └── error-handler.js
├── tests/
│   └── github.test.js
├── .env.example
├── mcp.json
└── README.md
```

## Troubleshooting

### Common Issues

#### Authentication Failed
```
Error: Authentication Failed: Requires authentication
```
**Solution**: Verify your GitHub token is valid and has the required scopes.

#### Repository Already Exists
```
Error: Repository already exists
```
**Solution**: Choose a different repository name or delete the existing repository.

#### Rate Limit Exceeded
```
Error: API rate limit exceeded
```
**Solution**: Wait for the rate limit to reset or implement exponential backoff.

#### Connection Timeout
```
Error: Connection timeout
```
**Solution**: Check your internet connection and increase timeout settings.

### Debug Mode

Enable debug logging by setting environment variables:

```bash
export DEBUG=mcp:*
export GITHUB_API_URL=https://api.github.com
```

### Logging

```javascript
// Enable detailed logging
const mcp = new MCPClient({
  debug: true,
  logger: console.log
});
```

## Contributing

We welcome contributions to this project! Please follow these steps:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit your changes**: `git commit -m 'Add amazing feature'`
4. **Push to the branch**: `git push origin feature/amazing-feature`
5. **Open a Pull Request**

### Development Setup

```bash
# Clone the repository
git clone https://github.com/your-username/GitHub-agent.git

# Install dependencies
npm install

# Run tests
npm test

# Start development server
npm run dev
```

### Code Style

- Use ESLint for JavaScript linting
- Follow the existing code style
- Write tests for new features
- Update documentation as needed

## Resources

- [Official MCP Documentation](https://modelcontextprotocol.io/)
- [GitHub API Documentation](https://docs.github.com/en/rest)
- [MCP GitHub Server](https://github.com/modelcontextprotocol/servers/tree/main/src/github)
- [MCP Specification](https://spec.modelcontextprotocol.io/)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

If you encounter any issues or have questions:

1. Check the [troubleshooting section](#troubleshooting)
2. Search existing [issues](https://github.com/your-username/GitHub-agent/issues)
3. Create a new issue with detailed information
4. Join our community discussions

---

**Happy coding with MCP! 🚀**

