# Qwen CLI Command Reference

This document provides a complete command reference for Qwen Code CLI, helping OpenClaw users efficiently call the Qwen LLM.

## Basic Commands

### Status Check
```bash
scripts/qwen-code.js status
```
Check Node.js version, CLI installation status, authentication status, configured models, and recent sessions.

### Version Information
```bash
scripts/qwen-code.js version
scripts/qwen-code.js -v
```

### Help
```bash
scripts/qwen-code.js help
scripts/qwen-code.js --help
scripts/qwen-code.js -h
```

---

## Task Execution

### Run Task
```bash
scripts/qwen-code.js run "Task description"
scripts/qwen-code.js run "Create Python Flask API" -m qwen3-coder-plus
scripts/qwen-code.js run "Refactor this function" -y
```

**Options:**
| Option | Description |
|--------|-------------|
| `-m, --model <model>` | Specify model |
| `-y, --yolo` | YOLO mode (auto-approve all actions) |
| `-s, --sandbox` | Sandbox mode |
| `--approval-mode <mode>` | Approval mode (plan\|default\|auto-edit\|yolo) |
| `-o, --output-format <format>` | Output format (text\|json\|stream-json) |
| `-d, --debug` | Debug mode |
| `--continue` | Resume most recent session in current project |
| `--resume <id>` | Resume specific session ID |

---

## Code Review

```bash
scripts/qwen-code.js review <file_path>
scripts/qwen-code.js review src/app.ts
scripts/qwen-code.js review src/app.ts -m qwen3-coder-plus
```

**Review Content:**
- Potential bugs
- Performance issues
- Code style issues
- Security vulnerabilities
- Improvement suggestions

---

## Headless Mode

For scripting, automation, and CI/CD integration:

```bash
scripts/qwen-code.js headless "Task description"
scripts/qwen-code.js headless "Analyze code" -o json
scripts/qwen-code.js headless "Generate commit message" --continue
```

**Pipeline Examples:**
```bash
git diff | qwen -p "Generate commit message"
gh pr diff | qwen -p "Review this PR"
cat logs/app.log | qwen -p "Analyze error cause"
```

---

## Sub-Agent Management

```bash
# Spawn sub-agent
scripts/qwen-code.js agent spawn "Code Reviewer" Please review this module

# List sub-agents
scripts/qwen-code.js agent list

# Other agent commands
scripts/qwen-code.js agent <action> [args]
```

---

## Skills Management

```bash
# List installed skills
scripts/qwen-code.js skill list

# Create new skill
scripts/qwen-code.js skill create "python-expert"

# Open skill directory
scripts/qwen-code.js skill open <skill-name>
```

---

## MCP Server Management

```bash
# List MCP servers
scripts/qwen-code.js mcp list

# Add MCP server
scripts/qwen-code.js mcp add google-drive

# Other MCP commands
scripts/qwen-code.js mcp <command> [args]
```

---

## Extensions Management

```bash
# List extensions
scripts/qwen-code.js extensions list

# Install extension
scripts/qwen-code.js extensions install <git-url>
```

---

## Available Models

| Model | Use Case |
|-------|----------|
| qwen3.5-plus | General programming |
| qwen3-coder-plus | Complex code tasks |
| qwen3-coder-next | Lightweight code generation |
| qwen3-max | Maximum capability |

---

## Configuration Files

| Item | Path |
|------|------|
| Config Directory | `~/.qwen/` |
| Config File | `~/.qwen/settings.json` |
| Session Data | `~/.qwen/projects/<cwd>/chats` |

---

## Authentication Methods

### OAuth Authentication (Recommended)
```bash
qwen
# Follow prompts to complete OAuth login
```

### API Key Authentication
Configure in `~/.qwen/settings.json`:
```json
{
  "env": {
    "BAILIAN_CODING_PLAN_API_KEY": "sk-xxx"
  }
}
```

---

## Automation Use Cases

```bash
# Code review automation
git diff | qwen -p "Generate commit message"

# Log analysis
tail -f app.log | qwen -p "Notify me if anomalies are detected"

# PR review
gh pr diff | qwen -p "Review this PR"

# OpenClaw background task
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Create API service'"
```

---

## VS Code Extension

- Extension Marketplace: https://marketplace.visualstudio.com/items?itemName=qwenlm.qwen-code-vscode-ide-companion
