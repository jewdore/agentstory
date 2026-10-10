# AgentStory

> 本地优先的 AI 编程会话管理工具，让 Codex 与 Claude Code 的历史记录真正成为可检索、可整理、可复用的个人知识库。

[下载最新版本](https://github.com/jewdore/agentstory/releases/latest) · [English](#english)

## AgentStory 是什么？

使用 AI 编程工具时，大量有价值的排查过程、技术决策和解决方案会散落在不同会话中。AgentStory 自动发现本机的 Codex 与 Claude Code 历史记录，将它们整理到统一的桌面应用中，方便你随时搜索、阅读和沉淀知识。

AgentStory 采用本地优先设计。浏览、搜索、标记和笔记默认都在你的设备上完成，不会自动上传会话内容。

## 主要功能

- **自动发现会话**：读取本机 Codex 和 Claude Code 的历史记录，无需手动导入。
- **统一阅读体验**：集中查看提问、回答、思考过程、工具调用和代码变更。
- **快速检索整理**：按关键词、项目、Agent、时间和标签查找历史会话。
- **笔记与标签**：摘录重要内容，添加备注和标签，逐步建立个人知识库。
- **本地 MCP 接口**：让支持 MCP 的工具按需检索你保存的 Notes。
- **安全分享**：由你主动选择内容，在检查敏感信息后创建只读分享快照。

## 下载与安装

前往 [Releases](https://github.com/jewdore/agentstory/releases/latest) 下载最新版本。

| 系统 | 推荐文件 | 说明 |
| --- | --- | --- |
| Windows 10/11 x64 | `AgentStory_*_x64-setup.exe` | 推荐使用的安装程序 |
| Windows 10/11 x64 | `AgentStory_*_x64_en-US.msi` | 适合需要 MSI 的用户或批量安装 |
| macOS（Apple Silicon / Intel） | `AgentStory_*_universal.dmg` | 通用安装镜像 |
| macOS（Apple Silicon / Intel） | `AgentStory_universal.app.tar.gz` | 压缩版应用程序 |

安装包目前可能尚未进行商业代码签名。如果 Windows SmartScreen 或 macOS Gatekeeper 显示安全提示，请确认文件来自本仓库的 Releases 页面后再决定是否继续。

## 开始使用

1. 下载并安装适合你系统的版本。
2. 启动 AgentStory，应用会自动扫描本机支持的 Agent 历史记录。
3. 通过项目、标签、收藏和搜索整理会话。
4. 在重要内容上创建 Notes，沉淀可复用的经验。

## 隐私说明

- 原始 Codex 与 Claude Code 历史文件仅以只读方式访问。
- 本地索引、标签和笔记保存在你的设备上。
- AgentStory 不会自动上传你的会话。
- 只有你主动发起分享时，所选内容才会经过敏感信息检查并创建分享快照。

## 反馈

如果遇到问题或有功能建议，请在本仓库提交 [Issue](https://github.com/jewdore/agentstory/issues)。提交问题时，请尽量附上操作系统、AgentStory 版本和复现步骤，但不要上传包含密钥、账号或私人会话的日志。

## 加入交流群

欢迎加入 AgentStory 用户交流群，交流使用经验、反馈问题和了解最新进展。

请添加以下任一微信，并备注 **“AgentStory 进群”**：

- `huobanbengkui`
- `jewdores`

## 支持 AgentStory

如果 AgentStory 对你有帮助，欢迎通过[爱发电](https://afdian.com/a/jerry)支持项目持续开发。你的支持将用于产品维护、功能改进和后续版本发布。

---

<a id="english"></a>

## English

AgentStory is a local-first desktop app that turns your Codex and Claude Code history into a searchable, organized, and reusable personal knowledge base.

[Download the latest release](https://github.com/jewdore/agentstory/releases/latest)

### What is AgentStory?

Valuable debugging trails, technical decisions, and solutions are often buried across AI coding sessions. AgentStory automatically discovers supported session history on your computer and brings it into one desktop app for reading, searching, and knowledge capture.

AgentStory is local-first. Browsing, searching, tagging, and note-taking happen on your device by default. Your sessions are never uploaded automatically.

### Highlights

- **Automatic discovery** of local Codex and Claude Code sessions.
- **Unified session reader** for prompts, responses, reasoning, tool calls, and code changes.
- **Fast search and organization** by keyword, project, agent, date, and tag.
- **Notes and tags** for capturing reusable knowledge from long conversations.
- **Local MCP access** so compatible tools can search your saved Notes when requested.
- **Controlled sharing** with a review and sensitive-information check before publishing a read-only snapshot.

### Download and install

Open the [Releases page](https://github.com/jewdore/agentstory/releases/latest) and choose the file for your platform:

| Platform | Recommended file | Notes |
| --- | --- | --- |
| Windows 10/11 x64 | `AgentStory_*_x64-setup.exe` | Recommended installer |
| Windows 10/11 x64 | `AgentStory_*_x64_en-US.msi` | MSI package for managed installation |
| macOS, Apple Silicon or Intel | `AgentStory_*_universal.dmg` | Universal disk image |
| macOS, Apple Silicon or Intel | `AgentStory_universal.app.tar.gz` | Compressed application bundle |

The installers may not yet carry a commercial code-signing certificate. If Windows SmartScreen or macOS Gatekeeper displays a warning, verify that the file came from this repository's Releases page before deciding whether to continue.

### Getting started

1. Download and install the package for your platform.
2. Launch AgentStory; supported local agent history is discovered automatically.
3. Organize sessions with projects, tags, favorites, and search.
4. Create Notes from important content to build reusable knowledge.

### Privacy

- Original Codex and Claude Code history files are accessed read-only.
- Local indexes, tags, and notes remain on your device.
- AgentStory never uploads sessions automatically.
- A share snapshot is created only when you explicitly choose to share selected content.

### Feedback

Please open an [Issue](https://github.com/jewdore/agentstory/issues) for bug reports or feature requests. Include your operating system, AgentStory version, and reproduction steps where possible, but never attach logs containing credentials or private conversations.

### Join the community

To join the AgentStory WeChat group, add either account below and include **“AgentStory 进群”** in your friend request:

- `huobanbengkui`
- `jewdores`

### Support AgentStory

If AgentStory is useful to you, you can support its continued development on [Afdian](https://afdian.com/a/jerry). Your support helps fund maintenance, improvements, and future releases.
