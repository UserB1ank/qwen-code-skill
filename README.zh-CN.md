<div align="center">

# 🦌 Qwen Code Skill

> 🚀 阿里云 Qwen Code CLI 工具封装。为 OpenClaw 提供任务执行、代码审查、自动化脚本等 AI 驱动的开发能力。

**作者**: [@UserB1ank](https://github.com/UserB1ank)  
**版本**: v1.1.0  
**许可证**: MIT

[📖 English Docs](README.md) | [📝 更新日志](CHANGELOG.md) | [📦 示例代码](assets/examples/)

</div>

---

## 📖 概述

| | |
|---|---|
| **是什么** | 阿里云 Qwen Code CLI 的 OpenClaw 工具封装 |
| **解决什么** | 将 Qwen Code 集成到 OpenClaw 工作流，支持任务执行、代码审查、自动化脚本 |
| **30 秒上手** | `scripts/qwen-code.js status` 检查状态，`scripts/qwen-code.js run "任务描述"` 执行任务 |

### ✨ 核心特性

- 🎯 **任务执行** - 使用自然语言运行编程任务
- 🔍 **代码审查** - 自动化代码分析和建议
- 🤖 **Headless 模式** - JSON 输出支持自动化和 CI/CD
- 🔌 **OpenClaw 集成** - 后台执行、进程管理、模型选择

---

## 🚀 快速开始

### 前置条件

```bash
# Node.js 20+
node --version

# 安装 Qwen Code CLI
npm install -g @qwen-code/qwen-code@latest

# 验证安装
qwen --version
```

### 认证配置

```bash
# 方式一：OAuth 认证（推荐）
qwen auth login

# 方式二：API Key 认证
export DASHSCOPE_API_KEY="sk-xxx"
```

### 基本用法

```bash
# 检查状态
scripts/qwen-code.js status

# 运行任务
scripts/qwen-code.js run "创建 Flask API"

# 代码审查
scripts/qwen-code.js review src/app.ts

# Headless 模式（JSON 输出）
scripts/qwen-code.js headless "分析代码" -o json
```

---

## 📋 命令列表

| 命令 | 说明 | 示例 |
|------|------|------|
| `status` | 检查 Qwen Code 状态和认证 | `scripts/qwen-code.js status` |
| `run <task>` | 执行编程任务 | `scripts/qwen-code.js run "创建 REST API"` |
| `review <file>` | 代码审查和分析 | `scripts/qwen-code.js review src/main.py` |
| `headless <task>` | Headless 模式（JSON 输出） | `scripts/qwen-code.js headless "分析" -o json` |
| `help` | 查看帮助信息 | `scripts/qwen-code.js help` |

---

## 🔌 OpenClaw 集成

### 后台模式运行

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

### 进程管理

```bash
# 查看日志
process action:log sessionId:XXX

# 检查是否完成
process action:poll sessionId:XXX

# 发送输入（如果 Qwen 提问）
process action:write sessionId:XXX data:"y"
```

### Headless 模式（自动化/CI/CD）

```bash
# JSON 输出
qwen -p "分析代码结构" --output-format json

# 管道操作
git diff | qwen -p "生成 commit message"

# 批量处理
find src -name "*.ts" | xargs -I {} qwen -p "审查 {}"
```

---

## 📁 项目结构

```
qwen-code-skill/
├── SKILL.md                      # Skill 定义文件
├── README.md                     # 英文文档
├── README.zh-CN.md               # 中文文档（本文件）
├── CHANGELOG.md                  # 版本历史
├── _meta.json                    # 元数据
├── assets/
│   └── examples/                 # 示例代码
│       ├── basic-task.example.sh
│       ├── code-review.example.sh
│       ├── ci-cd.example.yml
│       └── headless-mode.example.js
├── scripts/
│   └── qwen-code.js              # 主脚本
└── references/
    └── qwen-cli-commands.md      # 命令参考
```

---

## ✅ 适用场景 / ❌ 不适用场景

### ✅ 适用场景 (For)

- 需要使用 Qwen Code 完成编程任务的开发者
- 需要代码审查和分析的团队
- 需要自动化脚本和 CI/CD 集成的环境
- OpenClaw Sub-Agent 和 Skills 管理
- 批量代码分析和重构

### ❌ 不适用场景 (Not For)

- 未安装 Qwen Code CLI 的环境
- 需要图形界面交互的场景
- 非阿里云大模型用户
- 离线环境（需要网络连接）

---

## 🛡️ 安全与边界说明

| 组件 | 行为 | 执行 Shell 命令？ |
|------|------|------------------|
| `scripts/qwen-code.js` | 封装 Qwen Code CLI 命令 | 是（通过 `qwen` 命令） |
| `references/qwen-cli-commands.md` | 命令参考文档 | 否（纯文本） |
| `assets/examples/` | 示例代码 | 否（静态文件） |

### ⚠️ 安全说明

- 本 Skill 不直接执行代码，仅调用 Qwen Code CLI
- 所有代码生成和修改需用户确认
- 生产环境建议使用 review 模式
- 敏感项目请关闭 YOLO 模式

---

## 📦 示例代码

查看 [`assets/examples/`](assets/examples/) 获取完整示例：

| 示例 | 说明 |
|------|------|
| `basic-task.example.sh` | 基本任务执行示例 |
| `code-review.example.sh` | 代码审查工作流 |
| `ci-cd.example.yml` | GitHub Actions 集成 |
| `headless-mode.example.js` | Node.js 自动化示例 |

---

## 🔗 参考资料

- [📖 Qwen Code 官方文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [📝 命令参考](references/qwen-cli-commands.md)
- [📦 示例代码](assets/examples/)
- [🦌 OpenClaw 文档](https://openclaw.ai)

---

## 📝 更新日志

查看 [CHANGELOG.md](CHANGELOG.md) 获取完整版本历史和发布说明。

### 最新：v1.1.0 (2026-02-27)

✨ **EvoMap 风格重构**
- 中英双语 README 支持
- 示例代码目录（4 个示例文件）
- 完整命令参考文档
- 优化的 SKILL.md 结构

---

## ❓ 常见问题

<details>
<summary><b>Q: 这个 Skill 会自动修改代码吗？</b></summary>

A: 不会。它生成代码建议和修改方案，但需要用户确认后执行。

</details>

<details>
<summary><b>Q: 可以在生产环境使用吗？</b></summary>

A: 可以，但建议使用 review 模式，并在执行前进行代码审查。

</details>

<details>
<summary><b>Q: 支持哪些模型？</b></summary>

A: 支持阿里云通义千问系列模型，包括 qwen3.5-plus、qwen3-coder-plus 等。

</details>

<details>
<summary><b>Q: 如何查看完整命令列表？</b></summary>

A: 运行 `scripts/qwen-code.js help` 或查看 `references/qwen-cli-commands.md`。

</details>

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件。

---

<div align="center">

**Built with ❤️ for the OpenClaw Community**

[🔝 返回顶部](#-qwen-code-skill)

</div>
