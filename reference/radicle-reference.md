# Radicle Reference

## Overview

Radicle is a decentralized, peer-to-peer code collaboration platform built on Git with enhanced collaboration features. It emphasizes user sovereignty, privacy, and offline-first workflows without centralized control.

## Core Concepts

### Technical Foundation
- **Peer-to-peer networking**: Direct collaboration without central servers
- **Local-first storage**: All data stored locally with selective sharing
- **Cryptographic identities**: Secure, verifiable user authentication
- **Collaborative Objects (COBs)**: Structured data for issues and patches
- **Git-based**: Built on top of Git with additional collaboration layers

### Key Principles
- User sovereignty and data ownership
- Privacy-first design with optional anonymity
- Decentralized collaboration without single points of failure
- Cryptographically signed interactions for integrity

## Installation & Setup

### Requirements
- Linux or Unix-based operating system
- Git version 2.34.0 or later
- OpenSSH version 9.1 or later

### Identity Management
```bash
# Create new Radicle identity
rad auth

# View identity details
rad self
```

## Repository Operations

### Basic Repository Management
```bash
# Initialize a new Radicle repository
rad init

# Clone an existing repository
rad clone <repository-id>

# Seed a repository (make it available to others)
rad seed <repository-id>

# Publish repository (make it public)
rad publish
```

### Remote Management
```bash
# Manage repository remotes
rad remote
rad remote add <name> <repository-id>
rad remote remove <name>
```

## Collaboration Features

### Issue Management
```bash
# Create new issue (opens editor for title and description)
rad issue open

# Create issue with explicit title and description
rad issue open --title "Issue Title" --description "Detailed description of the issue"

# Create issue from template or file
cat issue_template.md | rad issue open

# List issues
rad issue list

# View specific issue
rad issue show <issue-id>

# Comment on issue
rad issue comment <issue-id>

# Close issue
rad issue close <issue-id>
```

#### Issue Creation Tips
- **Title and Description Required**: Issues need both a title and description to be created
- **Markdown Support**: Issue descriptions support full Markdown formatting
- **Long Descriptions**: For complex issues, use `--description` flag with multi-line strings
- **Template Approach**: Create issue templates and pipe them to `rad issue open`
- **Interactive Mode**: `rad issue open` without parameters opens an editor

#### Shell Parsing Limitations
- **Description Length Limits**: Very long descriptions can hit shell parsing limits
- **Special Character Issues**: Avoid complex nested quotes, backticks, and special characters in descriptions
- **Multi-line String Problems**: Large multi-line strings may cause parsing errors
- **Best Practice**: Keep descriptions concise and focused on essential information

#### Troubleshooting Issue Creation
```bash
# If issue creation fails with parsing errors:

# Option 1: Simplify the description
rad issue open --title "Short Title" --description "Brief, focused description without complex formatting"

# Option 2: Use interactive mode for complex issues
rad issue open
# This opens an editor where you can safely use complex formatting

# Option 3: Create from file template
echo "# Title\nDescription content" > issue.md
cat issue.md | rad issue open

# Check for common problematic characters:
# - Nested quotes: "text with 'inner quotes'"
# - Backticks in code blocks: ```code```
# - Special shell characters: $, `, \, |
```

### Patch Management
```bash
# Create patch from current branch
rad patch open

# List patches
rad patch list

# View specific patch
rad patch show <patch-id>

# Review patch
rad patch review <patch-id>

# Merge patch
rad patch merge <patch-id>
```

## Privacy & Security

### Tor Integration
Radicle supports multiple Tor configurations for enhanced privacy:

1. **Mixed Mode**: Selective Tor routing for specific operations
2. **Full Proxy Mode**: Route all Radicle traffic through Tor
3. **Transparent Proxy Mode**: Network-level Tor proxy configuration

### Access Control
- Private repositories with granular access permissions
- Cryptographic verification of all interactions
- Encrypted peer-to-peer connections
- Optional anonymous participation

## Workflow Patterns

### Basic Collaboration Workflow
1. Initialize or clone repository with `rad init` or `rad clone`
2. Create and manage issues with `rad issue open --title "Title" --description "Description"`
3. Develop features on branches
4. Submit patches with `rad patch open`
5. Review and merge patches collaboratively
6. Seed repository for availability

### Issue Management Workflow
1. **Create Detailed Issues**: Use `--title` and `--description` flags for structured issues
2. **Reference Issues**: Link patches to issues using issue IDs in commit messages
3. **Track Progress**: Update issues as work progresses
4. **Close Completed Issues**: Use `rad issue close <issue-id>` when work is finished

### Issue Creation Best Practices
1. **Start Simple**: Begin with concise descriptions, expand in comments if needed
2. **Test Length**: If creation fails, try shorter descriptions first
3. **Use Templates**: For complex issues, create templates in separate files
4. **Iterate**: Create basic issue first, then add details via comments
5. **Avoid Shell Conflicts**: Be cautious with special characters in command-line descriptions

### Decentralized Development
1. Fork repositories through cloning
2. Maintain independent development tracks
3. Share changes through patches
4. Coordinate through issues and discussions
5. Merge contributions without central authority

## Best Practices

### Repository Management
- Seed important repositories for high availability
- Use descriptive commit messages and patch descriptions
- Regularly sync with collaborator repositories
- Maintain clear issue tracking and documentation

### Security Considerations
- Verify cryptographic signatures on important changes
- Use Tor integration when privacy is required
- Regularly backup identity keys and repository data
- Review patches thoroughly before merging

### Collaboration Guidelines
- Communicate clearly through issues and patch descriptions
- Establish project conventions for patch submission
- Use semantic commit messages for clarity
- Coordinate major changes through discussion

## Troubleshooting

### Common Issues
- **Network connectivity**: Ensure proper peer discovery and connectivity
- **Identity verification**: Check cryptographic key integrity
- **Repository synchronization**: Verify seeding and remote configuration
- **Tor configuration**: Validate proxy settings for privacy modes

### Debugging Commands
```bash
# Check identity status
rad self

# Verify repository status
rad inspect

# List active connections
rad node status

# Check seeding status
rad seed list
```

## Related Resources

- [Radicle Official Documentation](https://radicle.xyz/guides/user)
- [Git Reference](jj-reference.md) (for Git workflow integration)
- [Commit Preparation Methodology](commit-preparation-methodology.md)