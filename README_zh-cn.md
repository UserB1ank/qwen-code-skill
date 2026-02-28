<div align="center">

# 🦌 Qwen Code Skill

> 🚀 Alibaba Cloud Qwen Code CLI 的 OpenClaw 封装器。通过 AI 驱动的助手执行任务、审查代码并自动化工作流。

**作者**: [@UserB1ank](https://github.com/UserB1ank)
**版本**: v1.4.0
**许可证**: MIT

[📝 更新日志](CHANGELOG.md) | [📦 示例](assets/examples/) | [📖 English README](README.md)

</div>

---

## 📖 概述

| | |
|---|---|
| **是什么** | 阿里巴巴云 Qwen Code CLI 的 OpenClaw 工具封装器 |
| **解决的痛点** | 将 Qwen Code 集成到 OpenClaw 工作流中，用于任务执行、代码审查和自动化 |
| **30 秒上手** | `scripts/qwen-code.js status` 检查状态，`scripts/qwen-code.js run "任务"` 执行任务 |

### ✨ 功能特性

---

## 🎯 触发关键词

当提到以下关键词时，此技能将被激活：

| 关键词 | 示例 |
|---------|---------|
| `qwen` | "用 qwen 审查这段代码" |
| `qwencode` | "在任务上运行 qwencode" |
| `qwen-code` | "执行 qwen-code 进行重构" |
| `qwen code` | "让 qwen code 分析这个" |
| `aliyun code` | "用 aliyun code 执行这个任务" |
| `dashscope` | "用 dashscope 模型运行" |

**触发示例：**
- "用 **qwen** 创建一个 Flask API"
- "运行 **qwencode** 审查 src/app.ts"
- "执行 **qwen-code** 进行重构任务"


- 🎯 **任务执行** - 使用自然语言运行编程任务
- 🔍 **代码审查** - 自动化代码分析和建议
- 🤖 **Headless 模式** - 用于自动化和 CI/CD 的 JSON 输出
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

### 认证

```bash
# 方式 1：OAuth（推荐）
qwen auth login

# 方式 2：API Key
export DASHSCOPE_API_KEY="sk-xxx"
```

### 基本用法

```bash
# 检查状态
scripts/qwen-code.js status

# 运行任务
scripts/qwen-code.js run "创建一个 Flask API"

# 代码审查
scripts/qwen-code.js review src/app.ts

# Headless 模式（JSON 输出）
scripts/qwen-code.js headless "分析代码" -o json
```

---

## 📋 命令

| 命令 | 描述 | 示例 |
|---------|-------------|---------|
| `status` | 检查 Qwen Code 状态和认证 | `scripts/qwen-code.js status` |
| `run <task>` | 执行编程任务 | `scripts/qwen-code.js run "创建 REST API"` |
| `review <file>` | 代码审查和分析 | `scripts/qwen-code.js review src/main.py` |
| `headless <task>` | Headless 模式（JSON 输出） | `scripts/qwen-code.js headless "分析" -o json` |
| `help` | 显示帮助信息 | `scripts/qwen-code.js help` |

---

## 🔌 OpenClaw 集成

### 后台执行

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

# 检查完成状态
process action:poll sessionId:XXX

# 发送输入（如果 Qwen 询问）
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
├── SKILL.md                      # Skill 定义（coding-agent 格式）
├── README.md                     # 英文文档
├── README_zh-cn.md               # 中文文档
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
    ├── qwen-cli-commands.md      # 命令参考
    └── openclaw-integration.md   # OpenClaw 集成指南
```

---

## ✅ 适用 / ❌ 不适用

### ✅ 适用

- 使用 Qwen Code 执行编程任务的开发者
- 需要代码审查和分析的团队
- 自动化脚本和 CI/CD 集成
- OpenClaw Sub-Agent 和 Skills 管理
- 批量代码分析和重构

### ❌ 不适用

- 未安装 Qwen Code CLI 的环境
- 需要 GUI 交互的场景
- 非阿里云大模型用户
- 离线环境（需要网络连接）

---

## 🛡️ 安全与边界

| 组件 | 行为 | 执行 Shell 命令？ |
|-----------|----------|-------------------------|
| `scripts/qwen-code.js` | 封装 Qwen Code CLI 命令 | 是（通过 `qwen` 命令） |
| `references/qwen-cli-commands.md` | 命令参考文档 | 否（纯文本） |
| `assets/examples/` | 示例代码文件 | 否（静态文件） |

### ⚠️ 安全说明

- 本 Skill 不直接执行代码，仅调用 Qwen Code CLI
- 所有代码生成和修改需要用户确认
- 生产环境使用审查模式
- 敏感项目禁用 YOLO 模式

---

## 📦 示例

查看 [`assets/examples/`](assets/examples/) 获取完整示例：

| 示例 | 描述 |
|---------|-------------|
| `basic-task.example.sh` | 基本任务执行 |
| `code-review.example.sh` | 代码审查工作流 |
| `ci-cd.example.yml` | GitHub Actions 集成 |
| `headless-mode.example.js` | Node.js 自动化示例 |

---

## 🔗 参考文档

- [📖 Qwen Code 官方文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [📝 命令参考](references/qwen-cli-commands.md)
- [📝 OpenClaw 集成指南](references/openclaw-integration.md)
- [📦 示例代码](assets/examples/)
- [🦌 OpenClaw 文档](https://openclaw.ai)

---

## 📝 更新日志

查看 [CHANGELOG.md](CHANGELOG.md) 获取版本历史和发布说明。

### 最新：v1.4.0 (2026-02-28)

🌍 **国际化版本**
- SKILL.md 转换为英文版本
- 所有参考文档改为英文
- 新增中文 README (README_zh-cn.md)
- 更新元数据和包描述为英文

### v1.3.0 (2026-02-27)

✨ **英文重构**
- SKILL.md 重新格式化为 coding-agent 风格
- 所有文档改为英文
- 简化结构以提高清晰度
- 添加故障排除部分

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件。

---

<div align="center">

**为 OpenClaw 社区构建 ❤️**

[🔝 返回顶部](#-qwen-code-skill)

</div>
