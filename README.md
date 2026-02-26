# Qwen Code Skill for OpenClaw

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org/)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-Skill-blue)](https://github.com/openclaw/openclaw)

集成阿里云官方 **Qwen Code CLI** 的 OpenClaw Skill，提供 AI 编程辅助能力。

## ✨ 特性

- 🤖 **官方集成** - 基于 Qwen Code CLI 官方工具
- 🎯 **双模式支持** - 后台模式（非交互式）+ tmux（交互式）
- 🔐 **灵活认证** - OAuth（免费）或 百炼 API Key（企业）
- 📦 **丰富功能** - Headless 模式、Sub-Agents、MCP、Skills 扩展
- 🚀 **自动化友好** - 支持 CI/CD、管道操作、并行任务
- 📊 **多模型支持** - Qwen3.5/GLM/MiniMax/Kimi 等 8+ 模型

## 📦 安装

### 前置要求

- Node.js 20+
- Qwen Code CLI (`npm install -g @qwen-code/qwen-code@latest`)
- OpenClaw 运行环境

### 安装 Skill

```bash
# 克隆到 OpenClaw skills 目录
git clone https://github.com/YOUR_USERNAME/qwen-code.git ~/.openclaw/skills/qwen-code

# 或使用 clawhub（如果已配置）
clawhub install qwen-code
```

### 认证配置（二选一）

**方式 1: OAuth（免费，推荐）**
```bash
qwen
# 选择 "Qwen OAuth (免费)" 并按提示登录
```

**方式 2: 百炼 API Key**
```json
// ~/.qwen/settings.json
{
  "env": {
    "BAILIAN_CODING_PLAN_API_KEY": "sk-sp-xxxxxxxx"
  }
}
```

## 🚀 快速开始

### 基本用法

```bash
# 检查状态
~/.openclaw/skills/qwen-code/qwen-code.js status

# 运行任务
~/.openclaw/skills/qwen-code/qwen-code.js run "创建 Python Flask API"

# 代码审查
~/.openclaw/skills/qwen-code/qwen-code.js review src/app.ts

# 查看帮助
~/.openclaw/skills/qwen-code/qwen-code.js help
```

### OpenClaw 会话中使用

**后台模式（推荐用于非交互式任务）：**
```bash
# 在 OpenClaw 中执行
bash workdir:/tmp/my-project background:true yieldMs:60000 \
  command:"qwen -p '创建 Flask API'"
```

**交互模式（复杂任务使用 tmux）：**
```bash
# 使用 tmux 技能启动交互式会话
qwen
```

### Headless 模式（自动化/CI/CD）

```bash
# 生成 commit message
git diff | qwen -p "生成 commit message"

# PR 审查
gh pr diff | qwen -p "审查此 PR"

# 日志分析
tail -f app.log | qwen -p "如果发现异常，Slack 通知我"

# JSON 输出（程序化处理）
qwen -p "分析代码结构" --output-format json
```

## 📖 功能概览

### 核心功能

| 功能 | 命令 | 说明 |
|------|------|------|
| 状态检查 | `qwen-code status` | 检查安装、认证、模型 |
| 任务执行 | `qwen-code run <task>` | 运行 AI 编程任务 |
| 代码审查 | `qwen-code review <file>` | 审查代码文件 |
| Headless | `qwen-code headless <task>` | 脚本化/自动化模式 |
| 子代理 | `qwen-code agent <action>` | 管理 Sub-Agents |
| Skills | `qwen-code skill <action>` | 管理技能扩展 |
| MCP | `qwen-code mcp <cmd>` | 连接外部数据源 |

### 可用模型

- `qwen3.5-plus` - 通用编程
- `qwen3-coder-plus` - 复杂代码任务
- `qwen3-coder-next` - 轻量代码生成
- `qwen3-max` - 最强能力
- `glm-4.7` / `glm-5` - 智谱 AI
- `MiniMax-M2.5` - MiniMax
- `kimi-k2.5` - 月之暗面

### 审批模式

| 模式 | 命令 | 说明 |
|------|------|------|
| 仅计划 | `--approval-mode plan` | 只显示计划，不执行 |
| 默认 | `--approval-mode default` | 询问后执行 |
| 自动编辑 | `--approval-mode auto-edit` | 自动批准编辑 |
| YOLO | `-y` | 自动批准所有 |

## 🔧 命令行选项

```bash
qwen-code <command> [options]

命令:
  status              检查状态和配置
  run <task>          运行 Qwen Code 任务
  review <file>       代码审查
  headless <task>     Headless 模式（脚本化/自动化）
  agent <action>      Sub-Agent 管理
  skill <action>      Skills 管理
  mcp <cmd>           MCP 服务器管理
  extensions <cmd>    扩展管理
  help                显示帮助

选项:
  -m, --model         指定模型
  -y, --yolo          YOLO 模式（自动批准）
  -s, --sandbox       沙盒模式
  --approval-mode     审批模式 (plan|default|auto-edit|yolo)
  -o, --output-format 输出格式 (text|json|stream-json)
  -d, --debug         调试模式
  --continue          恢复当前项目的最近会话
  --resume <id>       恢复指定会话 ID
```

## 📚 文档

- [官方 Qwen Code 文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [快速入门](https://qwenlm.github.io/qwen-code-docs/zh/users/quickstart/)
- [无头模式](https://qwenlm.github.io/qwen-code-docs/zh/users/features/headless/)
- [子代理](https://qwenlm.github.io/qwen-code-docs/zh/users/features/sub-agents/)
- [MCP](https://qwenlm.github.io/qwen-code-docs/zh/users/features/mcp/)
- [OpenClaw 文档](https://docs.openclaw.ai)

## ⚠️ 注意事项

1. **工作目录隔离** - 使用 `workdir` 避免访问无关文件
2. **后台模式优先** - 非交互式任务使用 `background:true`
3. **耐心监控** - 使用 `process:log` 检查进度
4. **不要读取敏感文件** - Qwen Code 只应访问项目相关文件

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

## 👥 作者

- pc01

---

**🦌 由 OpenClaw 社区驱动**
