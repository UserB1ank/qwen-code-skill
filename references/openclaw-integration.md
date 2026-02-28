# OpenClaw 集成指南

本文档介绍如何在 OpenClaw 中集成和使用 Qwen Code Skill。

## 概述

Qwen Code Skill 为 OpenClaw 提供调用阿里云 Qwen 大模型的能力，通过以下组件实现：

```
OpenClaw Agent
    ↓
调用 bash/exec 工具
    ↓
执行 qwen 命令
    ↓
阿里云 Qwen 大模型
```

---

## 安装步骤

### 1. 安装 Qwen Code CLI

```bash
npm install -g @qwen-code/qwen-code@latest
```

### 2. 安装 Skill

```bash
# 使用 ClawHub
clawhub install qwen-code

# 或手动克隆
git clone https://github.com/UserB1ank/qwen-code-skill.git ~/.openclaw/workspace/skills/qwen-code
```

### 3. 认证

```bash
# OAuth 方式（推荐）
qwen auth login

# 或 API Key 方式
export DASHSCOPE_API_KEY="sk-xxx"
```

---

## 在 OpenClaw 中使用

### 基本任务执行

```bash
# 后台执行任务
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '创建 Python Flask API'"
```

### 进程管理

```bash
# 查看会话列表
process action:list

# 查看日志
process action:log sessionId:XXX

# 检查完成状态
process action:poll sessionId:XXX

# 发送输入
process action:write sessionId:XXX data:"y"

# 终止会话
process action:kill sessionId:XXX
```

---

## OpenClaw 配置

### 工具策略

确保 OpenClaw 配置允许使用 `bash` 和 `process` 工具：

```json5
{
  tools: {
    allow: ["bash", "process"]
  }
}
```

### 沙盒配置

如果使用沙盒模式，确保沙盒内也安装了 Qwen Code CLI：

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

## 使用场景

### 1. 代码生成

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '创建 REST API，包含用户认证和 CRUD 操作'"
```

### 2. 代码审查

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '审查 src/ 目录的代码质量，找出潜在问题'"
```

### 3. 批量处理

```bash
# 并行处理多个任务
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '重构 module_a'"
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '重构 module_b'"
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '重构 module_c'"
```

### 4. CI/CD 集成

```bash
# 在 GitHub Actions 中使用
qwen -p "分析代码变更" --output-format json
```

---

## 最佳实践

### 1. 工作目录隔离

始终指定 `workdir`，避免 Agent 在不相关的目录中操作：

```bash
# ✅ 正确
bash workdir:~/my-project background:true command:"qwen -p '...'"

# ❌ 错误
bash background:true command:"qwen -p '...'"
```

### 2. 适当的超时设置

根据任务复杂度设置合适的 `yieldMs`：

```bash
# 小任务：10-30 秒
bash workdir:~/project background:true yieldMs:10000 command:"qwen -p '修复这个 bug'"

# 中等任务：30-60 秒
bash workdir:~/project background:true yieldMs:30000 command:"qwen -p '创建 API 模块'"

# 大任务：60 秒以上
bash workdir:~/project background:true yieldMs:60000 command:"qwen -p '重构整个项目架构'"
```

### 3. 监控和通知

使用 OpenClaw 的事件通知功能：

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '创建 API 服务' && openclaw system event --text '任务完成'"
```

---

## 安全注意事项

### 1. YOLO 模式使用限制

```bash
# ✅ 安全：在工作目录内
bash workdir:~/.openclaw/workspace background:true command:"qwen -y -p '...'"

# ❌ 危险：在系统目录
bash workdir:/etc background:true command:"qwen -y -p '...'"
```

### 2. 代码审查

生产环境代码应使用审查模式：

```bash
# 审查模式（需要确认）
bash workdir:~/project command:"qwen -p '...'"

# 避免 YOLO 模式
# bash workdir:~/project command:"qwen -y -p '...'"
```

### 3. 敏感信息

不要在提示词中包含敏感信息：

```bash
# ❌ 错误
qwen -p "使用密码 admin123 连接数据库"

# ✅ 正确
qwen -p "使用环境变量中的数据库配置连接"
```

---

## 故障排除

### 常见问题

| 问题 | 解决方案 |
|------|----------|
| "qwen: command not found" | `npm install -g @qwen-code/qwen-code@latest` |
| "Authentication required" | `qwen auth login` 或设置 `DASHSCOPE_API_KEY` |
| 会话卡住 | `process action:log sessionId:XXX` 查看状态 |
| 权限不足 | 检查 OpenClaw 工具策略配置 |

### 日志调试

```bash
# 启用调试模式
qwen -p "任务" -d

# 查看 OpenClaw 日志
openclaw logs --follow
```

---

## 相关文档

- [Qwen CLI 命令参考](qwen-cli-commands.md)
- [Qwen Code 官方文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [OpenClaw 文档](https://openclaw.ai)
