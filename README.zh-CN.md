# Qwen Code Skill

> 阿里云 Qwen Code CLI 工具封装。为 OpenClaw 提供任务执行、代码审查、自动化脚本等功能。

**作者**: UserB1ank

---

## 概述

### 三句话概述

- **是什么**: 阿里云 Qwen Code CLI 的 OpenClaw 工具封装
- **解决什么**: 将 Qwen Code 集成到 OpenClaw 工作流，支持任务执行、代码审查、自动化脚本
- **30 秒上手**: `scripts/qwen-code.js status` 检查状态，`scripts/qwen-code.js run "任务描述"` 执行任务

### 关键词

Qwen Code, CLI 封装，代码审查，自动化，CI/CD, OpenClaw Skill

---

## 快速开始

### 1. 安装依赖

```bash
# 安装 Qwen Code CLI
npm install -g @qwen-code/qwen-code@latest

# 验证安装
qwen --version
```

### 2. 认证配置

```bash
# 方式一：OAuth 认证（推荐）
qwen auth login

# 方式二：API Key 认证
export DASHSCOPE_API_KEY="sk-xxx"
```

### 3. 使用 Skill

```bash
# 检查状态
scripts/qwen-code.js status

# 运行任务
scripts/qwen-code.js run "创建 Flask API"

# 代码审查
scripts/qwen-code.js review src/app.ts

# Headless 模式
scripts/qwen-code.js headless "分析代码" -o json
```

---

## 功能特性

### 核心命令

| 命令 | 说明 | 示例 |
|------|------|------|
| `status` | 检查 Qwen Code 状态和认证 | `scripts/qwen-code.js status` |
| `run <task>` | 执行编程任务 | `scripts/qwen-code.js run "创建 REST API"` |
| `review <file>` | 代码审查和分析 | `scripts/qwen-code.js review src/main.py` |
| `headless <task>` | Headless 模式（JSON 输出） | `scripts/qwen-code.js headless "分析代码" -o json` |
| `help` | 查看帮助信息 | `scripts/qwen-code.js help` |

### OpenClaw 集成

#### 后台模式运行任务

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

#### Headless 模式（自动化/CI/CD）

```bash
# JSON 输出
qwen -p "分析代码结构" --output-format json

# 管道操作
git diff | qwen -p "生成 commit message"

# 批量处理
find src -name "*.ts" | xargs -I {} qwen -p "审查 {}" 
```

---

## 使用场景

### 适用场景 (For)

- ✅ 需要使用 Qwen Code 完成编程任务的开发者
- ✅ 需要代码审查和分析的团队
- ✅ 需要自动化脚本和 CI/CD 集成的环境
- ✅ OpenClaw Sub-Agent 和 Skills 管理
- ✅ 批量代码分析和重构

### 不适用场景 (Not For)

- ❌ 未安装 Qwen Code CLI 的环境
- ❌ 需要图形界面交互的场景
- ❌ 非阿里云大模型用户
- ❌ 离线环境（需要网络连接）

---

## 目录结构

```
qwen-code/
├── SKILL.md                      # Skill 描述文件
├── README.md                     # 英文文档
├── README.zh-CN.md               # 中文文档（本文件）
├── _meta.json                    # 元数据
├── assets/
│   └── examples/                 # 示例代码
├── scripts/
│   └── qwen-code.js              # 主脚本
└── references/
    └── qwen-cli-commands.md      # 命令参考
```

---

## 边界说明

| 组件 | 行为 | 执行 Shell 命令？ |
|------|------|------------------|
| `scripts/qwen-code.js` | 封装 Qwen Code CLI 命令 | 是（通过 qwen 命令） |
| `references/qwen-cli-commands.md` | 命令参考文档 | 否（纯文本） |
| `assets/examples/` | 示例代码 | 否（静态文件） |

### 安全说明

- ⚠️ 本 Skill 不直接执行代码，仅调用 Qwen Code CLI
- ⚠️ 所有代码生成和修改需用户确认
- ⚠️ 生产环境建议使用 review 模式
- ⚠️ 敏感项目请关闭 YOLO 模式

---

## 前置条件

### 必需

- Node.js 20+
- Qwen Code CLI (`npm install -g @qwen-code/qwen-code@latest`)
- 阿里云账号（OAuth 或 API Key 认证）

### 可选

- Git（用于版本控制相关功能）
- Python/TypeScript 等目标语言环境

---

## 常见问题

### Q: 这个 Skill 会自动修改代码吗？

A: 不会。它生成代码建议和修改方案，但需要用户确认后执行。

### Q: 可以在生产环境使用吗？

A: 可以，但建议使用 review 模式，并在执行前进行代码审查。

### Q: 支持哪些模型？

A: 支持阿里云通义千问系列模型，包括 qwen3.5-plus、qwen3-coder-plus 等。

### Q: 如何查看完整命令列表？

A: 运行 `scripts/qwen-code.js help` 或查看 `references/qwen-cli-commands.md`。

---

## 参考资料

- [Qwen Code 官方文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [命令参考](references/qwen-cli-commands.md)
- [示例代码](assets/examples/)
- [OpenClaw 文档](https://openclaw.ai)

---

## 更新日志

### v1.1.0

- 改进 SKILL.md 结构（EvoMap/evolver 风格）
- 新增中英双语 README
- 添加示例代码目录
- 优化文档边界说明

### v1.0.0

- 初始版本
- 基础命令封装
- OpenClaw 集成

---

## License

MIT
