---
name: qwen-code
description: 为 OpenClaw 提供调用阿里云 Qwen 大模型的能力。支持任务执行、代码审查、自动化脚本等场景。
metadata: {"clawdbot":{"emoji":"🦌","requires":{"anyBins":["qwen"]}}}
author: UserB1ank
---

# Qwen Code Skill

本 Skill 为 OpenClaw 提供调用阿里云 Qwen 大模型的能力，通过封装 Qwen Code CLI 实现编程任务执行、代码审查、自动化脚本等功能。

## 快速开始

### 前置条件

```bash
# 安装 Qwen Code CLI
npm install -g @qwen-code/qwen-code@latest

# 验证安装
qwen --version

# 认证（方式 1：OAuth）
qwen auth login

# 认证（方式 2：API Key）
export DASHSCOPE_API_KEY="sk-xxx"
```

### 基本用法

```bash
# 后台执行任务
bash workdir:~/project background:true yieldMs:30000 command:"qwen -p '创建 Flask API'"

# 监控进度
process action:log sessionId:XXX

# 检查完成状态
process action:poll sessionId:XXX
```

---

## 核心能力

### 1. 任务执行

```bash
# 基本任务
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '创建 Python Flask API'"

# 指定模型
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '分析代码结构' -m qwen3-coder-plus"

# YOLO 模式（自动批准）
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '重构这个函数' -y"
```

### 2. 代码审查

```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '审查 src/app.ts 的代码质量'"
```

### 3. Headless 模式（自动化/CI/CD）

```bash
# JSON 输出
qwen -p "分析代码结构" --output-format json

# 管道操作
git diff | qwen -p "生成 commit message"
gh pr diff | qwen -p "审查此 PR"
```

---

## 命令参考

| 命令 | 描述 | 示例 |
|------|------|------|
| `status` | 检查状态和认证 | `scripts/qwen-code.js status` |
| `run <task>` | 执行编程任务 | `scripts/qwen-code.js run "创建 REST API"` |
| `review <file>` | 代码审查 | `scripts/qwen-code.js review src/main.py` |
| `headless <task>` | 无头模式（JSON 输出） | `scripts/qwen-code.js headless "分析" -o json` |
| `help` | 显示帮助 | `scripts/qwen-code.js help` |

详细命令参考：[references/qwen-cli-commands.md](references/qwen-cli-commands.md)

---

## 支持模型

| 模型 | 用途 |
|------|------|
| qwen3.5-plus | 通用编程（默认） |
| qwen3-coder-plus | 复杂代码任务 |
| qwen3-coder-next | 轻量代码生成 |
| qwen3-max | 最强能力 |

**指定模型：**
```bash
bash workdir:~/project background:true yieldMs:30000 \
  command:"qwen -p '重构代码' -m qwen3-coder-plus"
```

---

## 进程管理

```bash
# 查看日志
process action:log sessionId:XXX

# 检查完成状态
process action:poll sessionId:XXX

# 发送输入（如果 Qwen 询问）
process action:write sessionId:XXX data:"y"

# 终止会话
process action:kill sessionId:XXX
```

---

## 使用规则

1. **尊重工具选择** — 用户要求用 Qwen 就用 Qwen，不要自己实现
2. **保持耐心** — 不要因为"慢"就终止会话
3. **用 process:log 监控** — 检查进度但不干扰
4. **YOLO 模式用于开发** — `--yolo` 自动批准（仅在工作区使用）
5. **生产代码用审查模式** — 确保安全
6. **可以并行** — 同时运行多个 Qwen 进程处理批量任务
7. **不要在 ~/clawd/ 中运行** — 使用目标项目目录或 /tmp
8. **工作区安全** — YOLO 模式仅在 `agents.defaults.workspace` 中安全

---

## 适用场景

✅ **推荐使用：**
- OpenClaw 调用 Qwen 大模型执行编程任务
- 代码审查和质量分析
- 自动化脚本和 CI/CD 集成
- 批量代码分析和重构
- Sub-Agent 任务委派

❌ **不推荐使用：**
- 未安装 Qwen Code CLI 的环境
- 需要 GUI 交互的场景
- 非阿里云大模型用户
- 离线环境（需要网络连接）

---

## 安全说明

| 组件 | 行为 | 执行 Shell 命令？ |
|------|------|------------------|
| `scripts/qwen-code.js` | 封装 Qwen Code CLI 命令 | 是（通过 `qwen` 命令） |
| `references/*.md` | 命令参考文档 | 否（纯文本） |
| `assets/examples/` | 示例代码文件 | 否（静态文件） |

**安全注意：**
- 本 Skill 不直接执行代码，仅调用 Qwen Code CLI
- 所有代码生成和修改需要用户确认
- 生产环境使用审查模式
- 敏感项目禁用 YOLO 模式

---

## 示例

查看 [`assets/examples/`](assets/examples/) 获取完整示例：

| 示例 | 描述 |
|------|------|
| `basic-task.example.sh` | 基本任务执行 |
| `code-review.example.sh` | 代码审查流程 |
| `ci-cd.example.yml` | GitHub Actions 集成 |
| `headless-mode.example.js` | Node.js 自动化示例 |

---

## 参考文档

- [📖 Qwen Code 官方文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [📝 命令参考](references/qwen-cli-commands.md)
- [📝 OpenClaw 集成指南](references/openclaw-integration.md)
- [📦 示例代码](assets/examples/)
- [🦌 OpenClaw 文档](https://openclaw.ai)

---

## 故障排除

### "qwen: command not found"
```bash
npm install -g @qwen-code/qwen-code@latest
```

### "Authentication required"
```bash
qwen auth login
# 或设置 API Key
export DASHSCOPE_API_KEY="sk-xxx"
```

### 会话卡住/等待输入
```bash
# 查看 Qwen 在问什么
process action:log sessionId:XXX
# 发送确认
process action:write sessionId:XXX data:"y"
```

### 终止卡住的会话
```bash
process action:kill sessionId:XXX
```

---

*Qwen Code Skill 🦌 — 为 OpenClaw 提供阿里云 Qwen 大模型调用能力*
