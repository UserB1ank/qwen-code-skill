---
name: qwen-code
description: Provides Alibaba Cloud Qwen LLM capabilities for OpenClaw. Supports task execution, code review, automation scripts, and more.
aliases: ["qwen", "qwencode", "qwen-code", "qwen code", "aliyun code", "dashscope"]
keywords: ["qwen", "qwencode", "qwen-code", "alibaba", "aliyun", "dashscope", "coding", "code review", "task execution"]
metadata: {"clawdbot":{"emoji":"🦌","requires":{"anyBins":["qwen"]}}}
author: UserB1ank
---

# Qwen Code Skill

This Skill provides Alibaba Cloud Qwen LLM capabilities for OpenClaw, wrapping Qwen Code CLI to enable programming task execution, code review, automation scripts, and more.

## 🎯 Trigger Keywords

This skill will be activated when you mention:

| Keyword | Example |
|---------|---------|
| `qwen` | "Use qwen to review this code" |
| `qwencode` | "Run qwencode on this task" |
| `qwen-code` | "Execute qwen-code for refactoring" |
| `qwen code` | "Let qwen code analyze this" |
| `aliyun code` | "Use aliyun code for this task" |
| `dashscope` | "Run with dashscope model" |

**Example triggers:**
- "Use **qwen** to create a Flask API"
- "Run **qwencode** to review src/app.ts"
- "Execute **qwen-code** for this refactoring task"
- "Let **qwen code** analyze the performance issues"

---

## Quick Start

### Prerequisites

```bash
# Install Qwen Code CLI
npm install -g @qwen-code/qwen-code@latest

# Verify installation
qwen --version

# Authenticate (Option 1: OAuth)
qwen auth login

# Authenticate (Option 2: API Key)
export DASHSCOPE_API_KEY="sk-xxx"
```

### Basic Usage

```bash
# Background task execution
bash workdir:~/project background:true yieldMs:30000 command:"qwen -p 'Create Flask API'"

# Monitor progress
process action:log sessionId:XXX

# Check completion status
process action:poll sessionId:XXX
```

---

## Core Capabilities

### 1. Task Execution

```bash
# Basic task
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Create Python Flask API'"

# Specify model
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Analyze code structure' -m qwen3-coder-plus"

# YOLO mode (auto-approve)
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Refactor this function' -y"
```

### 2. Code Review

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Review code quality in src/app.ts'"
```

### 3. Headless Mode (Automation/CI/CD)

```bash
# JSON output
qwen -p "Analyze code structure" --output-format json

# Pipeline operations
git diff | qwen -p "Generate commit message"
gh pr diff | qwen -p "Review this PR"
```

---

## Command Reference

| Command | Description | Example |
|---------|-------------|---------|
| `status` | Check status and authentication | `scripts/qwen-code.js status` |
| `run <task>` | Execute programming task | `scripts/qwen-code.js run "Create REST API"` |
| `review <file>` | Code review | `scripts/qwen-code.js review src/main.py` |
| `headless <task>` | Headless mode (JSON output) | `scripts/qwen-code.js headless "Analyze" -o json` |
| `help` | Show help | `scripts/qwen-code.js help` |

Detailed commands: [references/qwen-cli-commands.md](references/qwen-cli-commands.md)

---

## Supported Models

| Model | Use Case |
|-------|----------|
| qwen3.5-plus | General programming (default) |
| qwen3-coder-plus | Complex coding tasks |
| qwen3-coder-next | Lightweight code generation |
| qwen3-max | Most capable |

**Specify model:**
```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Refactor code' -m qwen3-coder-plus"
```

---

## Process Management

```bash
# View logs
process action:log sessionId:XXX

# Check completion
process action:poll sessionId:XXX

# Send input (if Qwen asks)
process action:write sessionId:XXX data:"y"

# Kill session
process action:kill sessionId:XXX
```

---

## Usage Rules

1. **Respect tool choice** — if user asks for Qwen, use Qwen
2. **Be patient** — don't kill sessions for being "slow"
3. **Monitor with process:log** — check progress without interfering
4. **YOLO mode for development** — `--yolo` auto-approves (workspace only)
5. **Review mode for production** — ensure safety
6. **Parallel is OK** — run multiple Qwen processes for batch tasks
7. **Never run in ~/clawd/** — use target project dir or /tmp
8. **Workspace safety** — YOLO mode only safe in `agents.defaults.workspace`

---

## Use Cases

✅ **Recommended:**
- OpenClaw calling Qwen LLM for programming tasks
- Code review and quality analysis
- Automation scripts and CI/CD integration
- Batch code analysis and refactoring
- Sub-Agent task delegation

❌ **Not Recommended:**
- Environments without Qwen Code CLI installed
- GUI interaction requirements
- Non-Alibaba Cloud LLM users
- Offline environments (requires network)

---

## Security Notes

| Component | Behavior | Executes Shell Commands? |
|-----------|----------|-------------------------|
| `scripts/qwen-code.js` | Wraps Qwen Code CLI commands | Yes (via `qwen` command) |
| `references/*.md` | Command reference docs | No (plain text) |
| `assets/examples/` | Example code files | No (static files) |

**Security considerations:**
- This Skill only calls Qwen Code CLI, doesn't execute code directly
- All code generation/modifications require user confirmation
- Use review mode in production environments
- Disable YOLO mode for sensitive projects

---

## Examples

See [`assets/examples/`](assets/examples/) for complete examples:

| Example | Description |
|---------|-------------|
| `basic-task.example.sh` | Basic task execution |
| `code-review.example.sh` | Code review workflow |
| `ci-cd.example.yml` | GitHub Actions integration |
| `headless-mode.example.js` | Node.js automation example |

---

## References

- [📖 Qwen Code Official Docs](https://qwenlm.github.io/qwen-code-docs/zh/)
- [📝 Command Reference](references/qwen-cli-commands.md)
- [📝 OpenClaw Integration Guide](references/openclaw-integration.md)
- [📦 Example Code](assets/examples/)
- [🦌 OpenClaw Docs](https://openclaw.ai)

---

## Troubleshooting

### "qwen: command not found"
```bash
npm install -g @qwen-code/qwen-code@latest
```

### "Authentication required"
```bash
qwen auth login
# Or set API Key
export DASHSCOPE_API_KEY="sk-xxx"
```

### Session stuck/waiting for input
```bash
# Check what Qwen is asking
process action:log sessionId:XXX
# Send confirmation
process action:write sessionId:XXX data:"y"
```

### Kill stuck session
```bash
process action:kill sessionId:XXX
```

---

*Qwen Code Skill 🦌 — Provides Alibaba Cloud Qwen LLM capabilities for OpenClaw*
