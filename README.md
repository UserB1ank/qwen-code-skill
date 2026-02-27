# Qwen Code Skill

> Qwen Code CLI wrapper for OpenClaw. Execute tasks, review code, and automate workflows.

**Author**: UserB1ank

---

## Overview

- **What it is**: OpenClaw tool wrapper for Alibaba Cloud Qwen Code CLI
- **Pain it solves**: Integrates Qwen Code into OpenClaw workflows for task execution, code review, and automation
- **Use in 30 seconds**: `scripts/qwen-code.js status` to check status, `scripts/qwen-code.js run "task"` to execute

---

## Quick Start

```bash
# Install Qwen Code CLI
npm install -g @qwen-code/qwen-code@latest

# Check status
scripts/qwen-code.js status

# Run a task
scripts/qwen-code.js run "Create a Flask API"

# Code review
scripts/qwen-code.js review src/app.ts
```

---

## For

- Developers using Qwen Code for programming tasks
- Teams needing code review and analysis
- Automation scripts and CI/CD integration
- OpenClaw Sub-Agent and Skills management

## Not For

- Environments without Qwen Code CLI installed
- GUI-based interaction requirements
- Non-Alibaba Cloud LLM users

---

## Commands

| Command | Description |
|---------|-------------|
| `status` | Check Qwen Code status and authentication |
| `run <task>` | Execute programming task |
| `review <file>` | Code review and analysis |
| `headless <task>` | Headless mode (JSON output) |
| `help` | Show help |

---

## OpenClaw Integration

```bash
# Run task in background mode
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Create Python Flask API'"

# Specify model
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Analyze code structure' -m qwen3-coder-plus"

# YOLO mode (auto-approve)
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Refactor this function' -y"
```

---

## Prerequisites

- Qwen Code CLI installed (`qwen` command)
- Node.js 20+
- Authentication completed (OAuth or API Key)

---

## References

- Official Docs: https://qwenlm.github.io/qwen-code-docs/zh/
- CLI Commands: `references/qwen-cli-commands.md`
- Examples: `assets/examples/`

---

## License

MIT
