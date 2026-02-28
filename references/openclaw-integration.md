# OpenClaw Integration Guide

This document describes how to integrate and use the Qwen Code Skill in OpenClaw.

## Overview

Qwen Code Skill provides Alibaba Cloud Qwen LLM capabilities for OpenClaw, implemented through the following components:

```
OpenClaw Agent
    ↓
Call bash/exec tool
    ↓
Execute qwen command
    ↓
Alibaba Cloud Qwen LLM
```

---

## Installation Steps

### 1. Install Qwen Code CLI

```bash
npm install -g @qwen-code/qwen-code@latest
```

### 2. Install Skill

```bash
# Using ClawHub
clawhub install qwen-code

# Or manual clone
git clone https://github.com/UserB1ank/qwen-code-skill.git ~/.openclaw/workspace/skills/qwen-code
```

### 3. Authentication

```bash
# OAuth method (recommended)
qwen auth login

# Or API Key method
export DASHSCOPE_API_KEY="sk-xxx"
```

---

## Using in OpenClaw

### Basic Task Execution

```bash
# Background task execution
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Create Python Flask API'"
```

### Process Management

```bash
# View session list
process action:list

# View logs
process action:log sessionId:XXX

# Check completion status
process action:poll sessionId:XXX

# Send input
process action:write sessionId:XXX data:"y"

# Terminate session
process action:kill sessionId:XXX
```

---

## OpenClaw Configuration

### Tool Policy

Ensure OpenClaw configuration allows using `bash` and `process` tools:

```json5
{
  tools: {
    allow: ["bash", "process"]
  }
}
```

### Sandbox Configuration

If using sandbox mode, ensure Qwen Code CLI is also installed in the sandbox:

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        docker: {
          setupCommand: "npm install -g @qwen-code/qwen-code@latest"
        }
      }
    }
  }
}
```

---

## Use Cases

### 1. Code Generation

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Create REST API with user authentication and CRUD operations'"
```

### 2. Code Review

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Review code quality in src/ directory, identify potential issues'"
```

### 3. Batch Processing

```bash
# Parallel processing of multiple tasks
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Refactor module_a'"
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Refactor module_b'"
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Refactor module_c'"
```

### 4. CI/CD Integration

```bash
# Use in GitHub Actions
qwen -p "Analyze code changes" --output-format json
```

---

## Best Practices

### 1. Working Directory Isolation

Always specify `workdir` to avoid Agent operating in unrelated directories:

```bash
# ✅ Correct
bash workdir:~/my-project background:true command:"qwen -p '...'"

# ❌ Wrong
bash background:true command:"qwen -p '...'"
```

### 2. Appropriate Timeout Settings

Set appropriate `yieldMs` based on task complexity:

```bash
# Small tasks: 10-30 seconds
bash workdir:~/project background:true yieldMs:10000 command:"qwen -p 'Fix this bug'"

# Medium tasks: 30-60 seconds
bash workdir:~/project background:true yieldMs:30000 command:"qwen -p 'Create API module'"

# Large tasks: 60+ seconds
bash workdir:~/project background:true yieldMs:60000 command:"qwen -p 'Refactor entire project architecture'"
```

### 3. Monitoring and Notifications

Use OpenClaw's event notification feature:

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p 'Create API service' && openclaw system event --text 'Task completed'"
```

---

## Security Considerations

### 1. YOLO Mode Usage Restrictions

```bash
# ✅ Safe: Within workspace directory
bash workdir:~/.openclaw/workspace background:true command:"qwen -y -p '...'"

# ❌ Dangerous: In system directories
bash workdir:/etc background:true command:"qwen -y -p '...'"
```

### 2. Code Review

Production code should use review mode:

```bash
# Review mode (requires confirmation)
bash workdir:~/project command:"qwen -p '...'"

# Avoid YOLO mode
# bash workdir:~/project command:"qwen -y -p '...'"
```

### 3. Sensitive Information

Do not include sensitive information in prompts:

```bash
# ❌ Wrong
qwen -p "Connect to database with password admin123"

# ✅ Correct
qwen -p "Connect using database configuration from environment variables"
```

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| "qwen: command not found" | `npm install -g @qwen-code/qwen-code@latest` |
| "Authentication required" | `qwen auth login` or set `DASHSCOPE_API_KEY` |
| Session stuck | `process action:log sessionId:XXX` to check status |
| Insufficient permissions | Check OpenClaw tool policy configuration |

### Log Debugging

```bash
# Enable debug mode
qwen -p "task" -d

# View OpenClaw logs
openclaw logs --follow
```

---

## Related Documentation

- [Qwen CLI Command Reference](qwen-cli-commands.md)
- [Qwen Code Official Docs](https://qwenlm.github.io/qwen-code-docs/zh/)
- [OpenClaw Documentation](https://openclaw.ai)
