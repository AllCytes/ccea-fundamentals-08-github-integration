# Claude Code: Commands, Hooks, Plugins, Skills, and MCPs Specification

This document explains the key extensibility mechanisms in Claude Code and how they differ from each other.

## Overview

Claude Code provides multiple ways to extend and customize its functionality:

1. **Commands** - Built-in CLI commands for direct interaction
2. **Hooks** - Event-driven shell scripts that execute automatically
3. **Plugins** - Packaged extensions that bundle functionality
4. **Skills** - AI-powered reusable task templates
5. **MCPs (Model Context Protocol)** - External tool integrations

---

## 1. Commands

### What They Are
Commands are built-in CLI directives that you invoke directly in the Claude Code interface, prefixed with a forward slash (`/`).

### Characteristics
- **Invocation**: Manual, user-initiated (e.g., `/help`, `/clear`, `/exit`)
- **Purpose**: Direct control over the CLI environment and session management
- **Scope**: System-level operations and session control
- **Execution**: Immediate, synchronous

### Examples
- `/help` - Display help information
- `/clear` - Clear conversation history
- `/exit` - Exit the session
- `/settings` - Open settings configuration

### When to Use
Use commands when you need to:
- Control the CLI session directly
- Access built-in utilities
- Manage conversation state
- Configure the environment

---

## 2. Hooks

### What They Are
Hooks are user-defined shell scripts that execute automatically in response to specific events during Claude Code's operation.

### Characteristics
- **Invocation**: Automatic, event-triggered
- **Purpose**: Extend behavior at specific lifecycle points
- **Scope**: Event-specific actions (before/after tool calls, on errors, etc.)
- **Execution**: Shell commands that run in the background
- **Configuration**: Defined in settings file

### Hook Types
- `user-prompt-submit-hook` - Runs when user submits a prompt
- `tool-call-hook` - Runs before/after tool execution
- `error-hook` - Runs when errors occur
- Custom event hooks

### Examples
```json
{
  "hooks": {
    "user-prompt-submit-hook": "echo 'Processing your request...'",
    "tool-call-hook": "notify-send 'Tool executed'"
  }
}
```

### When to Use
Use hooks when you need to:
- Run validation before operations
- Trigger notifications
- Log activity
- Enforce workflow policies
- Integrate with external systems automatically

---

## 3. Plugins

### What They Are
Plugins are packaged extensions that bundle together multiple features (commands, hooks, skills, MCPs) into a cohesive unit.

### Characteristics
- **Invocation**: Installed and enabled via configuration
- **Purpose**: Provide comprehensive feature sets
- **Scope**: Can include multiple extension types
- **Execution**: Various (depends on what the plugin includes)
- **Distribution**: Shareable packages

### Structure
A plugin typically includes:
- Configuration files
- Scripts and executables
- Documentation
- May bundle skills, hooks, or MCP servers

### Examples
- Authentication plugins
- Language-specific development environments
- Team workflow plugins
- Project template plugins

### When to Use
Use plugins when you need to:
- Deploy multiple related features together
- Share configurations across teams
- Package complex workflows
- Distribute reusable development environments

---

## 4. Skills

### What They Are
Skills are AI-powered, reusable task templates that Claude Code can invoke to perform specific operations. They're essentially prompt templates with associated logic.

### Characteristics
- **Invocation**: Via `/skill-name` or Skill tool
- **Purpose**: Reusable AI task patterns
- **Scope**: Specific workflows or task types
- **Execution**: Claude processes the skill's prompt and executes accordingly
- **Intelligence**: AI interprets and adapts the skill to context

### Skill Types
- **Built-in skills**: Provided by Claude Code (e.g., `/commit`, `/review-pr`)
- **User-defined skills**: Custom skills created by users
- **Plugin-provided skills**: Skills bundled in plugins

### Examples
- `/commit` - Create git commits with AI-generated messages
- `/review-pr` - Perform code reviews on pull requests
- `/pdf` - Process PDF documents
- Custom skills for domain-specific tasks

### When to Use
Use skills when you need to:
- Automate repetitive AI-assisted tasks
- Standardize workflows across teams
- Create reusable task patterns
- Combine multiple steps into single commands

---

## 5. MCPs (Model Context Protocol)

### What They Are
MCPs are standardized servers that provide Claude Code with access to external tools, data sources, and services through a defined protocol.

### Characteristics
- **Invocation**: Automatic when Claude needs the tool
- **Purpose**: Extend Claude's capabilities with external integrations
- **Scope**: Access to external systems and specialized tools
- **Execution**: Client-server communication via MCP protocol
- **Standardization**: Uses Model Context Protocol specification

### Components
- **MCP Server**: Provides tools/resources via the protocol
- **MCP Client**: Claude Code connects to servers
- **Tools**: Functions the server exposes
- **Resources**: Data the server provides
- **Prompts**: Templates the server offers

### Examples
- GitHub MCP server (gh CLI integration)
- Database MCP servers (PostgreSQL, MySQL)
- File system MCP servers
- API integration MCPs (Slack, Jira, etc.)
- Custom tool servers

### When to Use
Use MCPs when you need to:
- Integrate external services
- Access databases or APIs
- Provide specialized tools
- Connect to enterprise systems
- Share tools across multiple AI applications

---

## Comparison Matrix

| Feature | Commands | Hooks | Plugins | Skills | MCPs |
|---------|----------|-------|---------|--------|------|
| **Trigger** | Manual | Automatic | Varies | Manual/Auto | Automatic |
| **User Control** | Direct | Event-based | Configuration | Command-based | Transparent |
| **Complexity** | Simple | Medium | High | Medium | High |
| **Shareability** | N/A | Limited | High | High | High |
| **AI Integration** | No | No | Varies | Yes | Yes |
| **External Access** | No | Limited | Varies | No | Yes |
| **Configuration** | Built-in | Settings file | Plugin config | Skill definition | MCP config |

---

## How They Work Together

These mechanisms are complementary and often work together:

1. **Plugin bundles everything**: A plugin might include:
   - Custom skills for domain tasks
   - Hooks for validation
   - MCP servers for external access
   - Documentation and commands

2. **Skills use MCPs**: A skill can leverage MCP-provided tools to accomplish tasks

3. **Hooks validate operations**: Hooks can check conditions before skills execute

4. **Commands invoke skills**: User types `/skill-name` to activate a skill

### Example Workflow
```
User types: /deploy
↓
Command invokes the "deploy" skill
↓
Skill triggers pre-deployment hook
↓
Hook validates environment
↓
Skill uses MCP server to access deployment service
↓
MCP server executes deployment
↓
Post-deployment hook sends notification
```

---

## Decision Guide

**Choose Commands when:**
- You need immediate session control
- The action is built into Claude Code
- You want simple, direct interaction

**Choose Hooks when:**
- You need automatic responses to events
- You want to enforce policies
- You need integration with existing scripts

**Choose Plugins when:**
- You're packaging multiple features
- You want to share configurations
- You need comprehensive solutions

**Choose Skills when:**
- You have repetitive AI-assisted tasks
- You want intelligent, context-aware automation
- You need standardized workflows

**Choose MCPs when:**
- You need external system access
- You're building reusable tools
- You want standard integrations
- You need to share tools across AI apps

---

## Additional Resources

- Claude Code Documentation: https://github.com/anthropics/claude-code
- Model Context Protocol: https://modelcontextprotocol.io/
- Skills Documentation: Check Claude Code's built-in help
- Plugin Development Guide: Refer to Claude Code SDK

---

**Generated with Claude Code**
*Last updated: 2026-02-05*
