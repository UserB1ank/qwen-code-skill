---
name: qwen-code
description: 阿里云 Qwen Code CLI 工具封装。提供状态检查、任务执行、代码审查、Headless 自动化等功能
author: UserB1ank
---

# Qwen Code Skill

三句话概述：

- **是什么**: 阿里云 Qwen Code CLI 的 OpenClaw 工具封装
- **解决什么**: 将 Qwen Code 集成到 OpenClaw 工作流，支持任务执行、代码审查、自动化脚本
- **30 秒上手**: `scripts/qwen-code.js status` 检查状态，`scripts/qwen-code.js run "任务描述"` 执行任务

**关键词**: Qwen Code, CLI 封装，代码审查，自动化，CI/CD

---

## For

- 需要使用 Qwen Code 完成编程任务的开发者
- 需要代码审查和分析的团队
- 需要自动化脚本和 CI/CD 集成的环境
- OpenClaw Sub-Agent 和 Skills 管理

## Not For

- 未安装 Qwen Code CLI 的环境
- 需要图形界面交互的场景
- 非阿里云大模型用户

---

## 快速开始

```bash
# 1. 安装 Qwen Code CLI
npm install -g @qwen-code/qwen-code@latest

# 2. 检查状态
scripts/qwen-code.js status

# 3. 运行任务
scripts/qwen-code.js run "创建 Flask API"

# 4. 代码审查
scripts/qwen-code.js review src/app.ts
```

---

## 核心功能

### 命令列表

| 命令 | 说明 |
|------|------|
| `status` | 检查 Qwen Code 状态和认证 |
| `run <task>` | 执行编程任务 |
| `review <file>` | 代码审查和分析 |
| `headless <task>` | Headless 模式（JSON 输出） |
| `help` | 查看帮助 |

### OpenClaw 集成示例

```bash
# 后台模式运行任务
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '创建 Python Flask API'"

# 指定模型
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '分析代码结构' -m qwen3-coder-plus"

# YOLO 模式（自动批准）
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '重构这个函数' -y"
```

### Headless 模式（自动化/CI/CD）

```bash
# JSON 输出
qwen -p "分析代码结构" --output-format json

# 管道操作
git diff | qwen -p "生成 commit message"
```

---

## 边界说明

| 组件 | 行为 | 执行 Shell 命令？ |
|------|------|------------------|
| `scripts/qwen-code.js` | 封装 Qwen Code CLI 命令 | 是（通过 qwen 命令） |
| `references/qwen-cli-commands.md` | 命令参考文档 | 否（纯文本） |

**安全说明**:
- 本 Skill 不直接执行代码，仅调用 Qwen Code CLI
- 所有代码生成和修改需用户确认
- 生产环境建议使用 review 模式

---

## 前置条件

- 已安装 Qwen Code CLI (`qwen` 命令)
- Node.js 20+
- 完成认证（OAuth 或 API Key）

---

## 参考资料

- 官方文档：https://qwenlm.github.io/qwen-code-docs/zh/
- 命令参考：`references/qwen-cli-commands.md`
- 示例代码：`assets/examples/`
