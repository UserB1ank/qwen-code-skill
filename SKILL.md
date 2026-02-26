# Qwen Code Skill - 官方 Qwen Code CLI 集成

## 技能描述

集成阿里云官方 **Qwen Code CLI**，提供 AI 编程辅助能力。Qwen Code 是运行在终端中的 AI 编码助手，帮助你快速将想法转化为代码。

**核心模式：**
- ✅ **后台模式** - 非交互式任务（推荐）
- ✅ **交互模式** - 复杂编码会话（使用 tmux）

官方文档：https://qwenlm.github.io/qwen-code-docs/zh/

## 前置条件

✅ 已安装 Qwen Code CLI (`qwen` 命令)  
✅ Node.js 20+ (`node -v` 检查)  
✅ 完成认证（OAuth 或 API Key，二选一）

## 安装方式

### NPM（推荐）
```bash
npm install -g @qwen-code/qwen-code@latest
```

### Homebrew（macOS, Linux）
```bash
brew install qwen-code
```

### VS Code 扩展（测试版）
安装 [Qwen Code Companion](https://marketplace.visualstudio.com/items?itemName=qwenlm.qwen-code-vscode-ide-companion) 获得图形界面体验。

## 认证方式（二选一）

### 方式 1: OAuth 认证（免费，推荐）

```bash
cd your-project
qwen
# 选择 "Qwen OAuth (免费)" 并按照提示登录
```

### 方式 2: 百炼 API Key（付费，适合企业/自动化）

编辑 `~/.qwen/settings.json`：

```json
{
  "env": {
    "BAILIAN_CODING_PLAN_API_KEY": "sk-sp-xxxxxxxx"
  }
}
```

**获取 API Key**: https://bailian.console.aliyun.com/

---

首次使用后会保存认证信息，无需重复配置。

## OpenClaw 集成工具

本技能包含一个辅助工具脚本 `qwen-code.js`，提供便捷的 CLI 封装：

```bash
# 检查状态
~/.openclaw/skills/qwen-code/qwen-code.js status

# 运行任务
~/.openclaw/skills/qwen-code/qwen-code.js run "创建 Flask API"

# 代码审查
~/.openclaw/skills/qwen-code/qwen-code.js review src/app.ts

# Headless 模式
~/.openclaw/skills/qwen-code/qwen-code.js headless "分析代码" -o json

# 查看帮助
~/.openclaw/skills/qwen-code/qwen-code.js help
```

**注意**: 这些命令底层调用官方 `qwen` CLI，需要先安装 Qwen Code。

---

## 核心功能

### 1. 后台模式（推荐用于非交互式任务）

使用 `exec` 工具的 `background` 和 `yieldMs` 参数运行 Qwen Code：

```bash
# 创建临时工作目录（避免污染主项目）
SCRATCH=$(mktemp -d)

# 后台运行 Qwen Code 任务
bash workdir:$SCRATCH background:true yieldMs:30000 command:"qwen -p '创建 Python Flask API'"

# 或指定模型和输出格式
bash workdir:~/my-project background:true yieldMs:60000 command:"qwen -p '分析代码结构' -o json"

# 返回 sessionId 用于跟踪
```

**监控进度：**
```bash
# 查看日志
process action:log sessionId:XXX

# 检查是否完成
process action:poll sessionId:XXX

# 发送输入（如果 Qwen Code 提问）
process action:write sessionId:XXX data:"y"

# 必要时终止
process action:kill sessionId:XXX
```

### 2. Headless 模式（脚本化/自动化）

适合 CI/CD、管道操作：

```bash
# 直接提示
qwen --prompt "什么是机器学习？"
qwen -p "创建 Python Flask API"

# 标准输入（Unix 哲学：可组合）
echo "解释这段代码" | qwen
cat README.md | qwen -p "总结此文档"
tail -f app.log | qwen -p "如果出现异常，通知我"

# 恢复会话
qwen --continue -p "再次运行测试并总结失败项"
qwen --resume <session-id> -p "应用后续重构"

# JSON 输出（结构化数据）
qwen -p "分析代码结构" --output-format json

# 流式 JSON（实时处理）
qwen -p "处理大文件" --output-format stream-json
```

### 3. 交互式编程（使用 tmux）

复杂任务使用 tmux 技能：

```bash
# 启动交互式会话
qwen

# 指定模型
qwen -m qwen3-coder-plus

# YOLO 模式（自动批准）
qwen -y
```

### 4. Sub-Agents（子代理）

Qwen Code 可以创建子代理并行处理任务：

```bash
# 在会话中创建子代理
qwen "/agent spawn code-reviewer 请审查这个模块"

# 子代理并行处理
qwen "/agents 请分别分析前端和后端代码"

# 列出子代理
qwen "/agents list"
```

### 5. Skills（技能扩展）

扩展 Qwen Code 的能力：

```bash
# Skills 目录：~/.qwen/skills/
ls ~/.qwen/skills/

# 安装 skill
qwen extensions install <git-url>

# 创建 skill
qwen extensions new ~/.qwen/skills/my-skill

# 列出扩展
qwen extensions list
```

### 6. MCP（Model Context Protocol）

连接外部数据源（Google Drive、Figma、Slack、Jira 等）：

```bash
# 列出 MCP 服务器
qwen mcp list

# 添加 MCP 服务器
qwen mcp add <server-name> <config>

# 移除 MCP 服务器
qwen mcp remove <server-name>
```

### 7. 输出格式

| 格式 | 命令 | 用途 |
|------|------|------|
| 文本（默认） | `qwen -p "任务"` | 人类可读 |
| JSON | `qwen -p "任务" --output-format json` | 程序化处理 |
| Stream JSON | `qwen -p "任务" --output-format stream-json` | 实时流式处理 |

### 8. 审批模式

| 模式 | 命令 | 说明 |
|------|------|------|
| 仅计划 | `--approval-mode plan` | 只显示计划，不执行 |
| 默认 | `--approval-mode default` | 询问后执行 |
| 自动编辑 | `--approval-mode auto-edit` | 自动批准编辑操作 |
| YOLO | `-y` | 自动批准所有操作 |

---

## 自动化用例

### 1. 生成提交信息

```bash
git diff | qwen -p "生成 commit message"
```

### 2. 批量代码分析

```bash
find src -name "*.py" | xargs cat | qwen -p "分析代码质量" --output-format json
```

### 3. PR 代码审查

```bash
gh pr diff | qwen -p "审查此 PR"
```

### 4. 日志分析

```bash
cat logs/app.log | qwen -p "分析错误原因"
tail -f app.log | qwen -p "如果发现异常，Slack 通知我"
```

### 5. 发布说明生成

```bash
git log v1.0..HEAD | qwen -p "生成发布说明"
```

### 6. CI/CD 自动化

```bash
# 在 CI 中自动翻译
qwen -p "如果有新的文本字符串，将它们翻译成法语并为 @lang-fr-team 创建 PR"
```

### 7. 调试和修复

```bash
qwen -p "修复这个 TypeError: Cannot read property 'map' of undefined"
```

### 8. 代码库导航

```bash
qwen -p "这个项目的认证流程是如何工作的？"
```

### 9. 并行任务（后台模式）

```bash
# 同时运行多个分析任务
bash workdir:~/project background:true command:"qwen -p '分析前端代码'"
bash workdir:~/project background:true command:"qwen -p '分析后端代码'"
bash workdir:~/project background:true command:"qwen -p '分析数据库结构'"

# 监控所有任务
process action:list
```

---

## 会话数据

会话数据存储在：
```
~/.qwen/projects/<sanitized-cwd>/chats
```

## 配置文件

```
~/.qwen/settings.json
```

---

## ⚠️ 规则

1. **尊重工具选择** — 如果用户要求 Qwen Code，使用 Qwen Code
2. **后台模式优先** — 非交互式任务使用 `background:true`
3. **tmux 用于交互** — 复杂编码会话使用 tmux 技能
4. **工作目录隔离** — 使用 `workdir` 避免污染主项目
5. **耐心监控** — 使用 `process:log` 检查进度，不要过早终止
6. **不要读取无关文件** — Qwen Code 只应访问项目相关文件

---

## 可用模型

| 模型 | 用途 |
|------|------|
| qwen3.5-plus | 通用编程 |
| qwen3-coder-plus | 复杂代码任务 |
| qwen3-coder-next | 轻量代码生成 |
| qwen3-max | 最强能力 |

---

## 参考资料

- [官方文档](https://qwenlm.github.io/qwen-code-docs/zh/)
- [快速入门](https://qwenlm.github.io/qwen-code-docs/zh/users/quickstart/)
- [无头模式](https://qwenlm.github.io/qwen-code-docs/zh/users/features/headless/)
- [子代理](https://qwenlm.github.io/qwen-code-docs/zh/users/features/sub-agents/)
- [Skills](https://qwenlm.github.io/qwen-code-docs/zh/users/features/skills/)
- [MCP](https://qwenlm.github.io/qwen-code-docs/zh/users/features/mcp/)
- [NPM 包](https://www.npmjs.com/package/@qwen-code/qwen-code)
- [VS Code 扩展](https://marketplace.visualstudio.com/items?itemName=qwenlm.qwen-code-vscode-ide-companion)
