# Visual Studio Project Template with Comprehensive Git Configuration

This repository provides a pre-configured Visual Studio project template with an extensive Git configuration setup. 
It helps developers maintain clean repositories by automatically excluding common Visual Studio-generated files and build artifacts from version control.

The template implements industry best practices for .NET development environments by providing a comprehensive .gitignore configuration.
It covers all major Visual Studio components including build outputs, user-specific files, NuGet packages, and various IDE-generated artifacts. This template is particularly suited for Agentic AI development and MCP (Multi-agent Cooperative Protocol) experimentation projects utilizing Vibe coding methodologies.
This setup ensures consistent version control practices across team environments and prevents unnecessary files from bloating the repository.

## Repository Structure
```
.
├── .github/                     # GitHub specific configurations and documentation
│   └── copilot-instructions.md  # GitHub Copilot setup instructions
├── mcp-config-used-amazonQ/     # MCP configuration examples
│   ├── config.json             # PostgreSQL MCP server configuration
│   └── mcp.json                # Multi-database MCP server configuration
├── .gitignore                   # Comprehensive VS-specific Git ignore rules
└── prd.md                       # Product requirements documentation
```

## MCP Configuration Examples

The repository includes working examples of MCP server configurations that demonstrate integration with Oracle and PostgreSQL databases using Amazon Q CLI.

### PostgreSQL Configuration (config.json)
```json
{
  "mcpServers": {
    "postgres": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "DATABASE_URI",
        "crystaldba/postgres-mcp",
        "--access-mode=unrestricted"
      ],
      "env": {
        "DATABASE_URI": "postgresql://admin:root@localhost:5432/test_db"
      }
    }
  }
}
```

### Multi-Database Configuration (mcp.json)
```json
{
  "mcpServers": {
    "postgres": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "DATABASE_URI",
        "crystaldba/postgres-mcp",
        "--access-mode=unrestricted"
      ],
      "env": {
        "DATABASE_URI": "postgresql://admin:root@localhost:5432/test_db"
      }
    },
    "oracle": {
      "command": "docker",
      "args": [
        "run", 
        "-i", 
        "--rm", 
        "-e",
        "ORACLE_USER=system",
        "-e",
        "ORACLE_PASSWORD=root",
        "mochoa/mcp-oracle", 
        "host.docker.internal:1521/XEPDB1"]
    }
  }
}
```

These configurations demonstrate:
- Docker-based deployment of MCP servers
- Connection settings for PostgreSQL and Oracle databases
- Environment variable configuration for secure credential management
- Unrestricted access mode for development environments
- Docker networking setup for local database connections

## Usage Instructions

### Prerequisites
- Git (version 2.x or higher)
- Visual Studio 2015 or later
- .NET development environment

### Installation

1. Clone the template repository:
```bash
git clone <repository-url> my-project
cd my-project
```

2. Initialize your project:
```bash
# Remove existing Git history
rm -rf .git
# Initialize new repository
git init
# Initial commit with template
git add .
git commit -m "Initial commit from template"
```

### Quick Start

1. Open the solution in Visual Studio
2. Verify the .gitignore is working:
```bash
# Should show only tracked files
git status
```

3. Start developing your project with the correct Git ignore rules in place

### More Detailed Examples

#### Verifying Ignored Files
```bash
# Create some common Visual Studio artifacts
touch bin/Debug/output.exe
touch obj/Debug/temporary.cache
# Check git status - these files should not appear
git status
```

#### Adding Custom Rules
```bash
# Add custom rules to .gitignore
echo "# Custom rules" >> .gitignore
echo "my-custom-folder/" >> .gitignore
```

### Troubleshooting

#### Common Issues

1. **Files being tracked despite being in .gitignore**
   - Problem: Files were committed before being added to .gitignore
   - Solution:
   ```bash
   git rm --cached <file>
   git commit -m "Remove tracked file that should be ignored"
   ```

2. **IDE-specific files appearing in Git status**
   - Problem: Incomplete .gitignore configuration
   - Solution: Verify your Visual Studio version and update .gitignore accordingly
   ```bash
   # Check current tracked files
   git ls-files
   # Update .gitignore with missing patterns
   ```

#### Debugging .gitignore Rules
To check why a file is being ignored:
```bash
git check-ignore -v path/to/file
```

To test .gitignore patterns:
```bash
git status --ignored
```

## Data Flow

The .gitignore configuration filters files based on pattern matching before Git operations.

```ascii
[Working Directory] --> [.gitignore Filter] --> [Git Index] --> [Repository]
     |                         |
     |                         |
     v                         v
[Tracked Files]        [Ignored Files]
```

Key interactions:
- Git checks file paths against .gitignore patterns
- Matching files are excluded from Git operations
- Patterns are evaluated in order of appearance
- More specific patterns take precedence over general ones
- Nested .gitignore files can provide additional rules
- Directory-specific patterns use forward slashes
- Pattern matching supports wildcards and negation