# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.1.0] - 2026-02-27

### ✨ Added

- 中英双语 README 文档 (`README.md` / `README.zh-CN.md`)
- 示例代码目录 (`assets/examples/`)
  - 基本任务执行示例 (`basic-task.example.sh`)
  - 代码审查示例 (`code-review.example.sh`)
  - CI/CD 集成示例 (`ci-cd.example.yml`)
  - Headless 模式示例 (`headless-mode.example.js`)
- 完整命令参考文档 (`references/qwen-cli-commands.md`)
- 项目元数据文件 (`_meta.json`)

### 🔧 Changed

- **重构 SKILL.md**: 采用 EvoMap/evolver 风格
  - 3 句话概述（What it is / Pain it solves / Use in 30 seconds）
  - For / Not For 清单
  - 快速开始命令
  - 边界说明表格
- 目录结构调整: `skill/` → `scripts/`
- 文档风格优化：简洁直接、技术导向、代码块展示命令

### 📝 Documentation

- 新增 README.md（英文精简版）
- 新增 README.zh-CN.md（中文完整版）
- 添加中英文切换链接
- 增加 Emoji 图标提升可读性

### 🏷️ Release

- Tag: `v1.1.0`
- Commit: `HEAD`

---

## [1.0.0] - 2026-02-26

### ✨ Added

- 初始版本发布
- 基础命令封装 (`scripts/qwen-code.js`)
  - `status` - 状态检查
  - `run` - 任务执行
  - `review` - 代码审查
  - `headless` - 无头模式
  - `help` - 帮助信息
- OpenClaw 集成支持
- 基础 Skill 定义文件 (`SKILL.md`)

### 🔧 Changed

- 项目更名为 `qwen-code-skill`

### 📝 Documentation

- 初始 README 文档
- 基础使用说明

### 🏷️ Release

- Tag: `v1.0.0`
- Commit: `6f6fff1`

---

## Version History

| Version | Date | Description |
|---------|------|-------------|
| 1.1.0 | 2026-02-27 | ✨ EvoMap 风格重构，中英双语支持 |
| 1.0.0 | 2026-02-26 | 🚀 初始版本发布 |

---

## Upcoming

### [Unreleased]

- [ ] 添加更多示例代码
- [ ] 支持 MCP 服务器管理
- [ ] 支持 Sub-Agent 管理
- [ ] 添加单元测试

---

## Release Notes

### Version 1.1.0 Highlights

🎯 **核心改进**: 采用 EvoMap/evolver 项目风格，全面提升文档质量和用户体验。

📚 **文档升级**:
- 中英双语支持，满足国际化需求
- 清晰的 For / Not For 边界说明
- 丰富的示例代码（Shell / YAML / JavaScript）
- 完整的命令参考文档

🔧 **结构优化**:
- 目录结构更清晰（scripts/ / references/ / assets/）
- SKILL.md 符合 skill-creator 最佳实践
- 元数据完善（author / version / description）

### Version 1.0.0 Highlights

🚀 **从零到一**: 完成 Qwen Code CLI 到 OpenClaw 的基础集成。

✅ **核心功能**:
- 状态检查
- 任务执行
- 代码审查
- Headless 自动化

🔗 **OpenClaw 集成**: 支持后台模式、进程管理、模型指定等功能。
