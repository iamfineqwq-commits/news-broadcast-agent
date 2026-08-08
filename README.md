# news-broadcast-agent
AI智能新闻助理Agent
# 📰 新闻报通 · AI 资讯与日程助理

[![n8n](https://img.shields.io/badge/n8n-2.31.4-1A1A1A?logo=n8n)](https://n8n.io/)
[![DeepSeek](https://img.shields.io/badge/DeepSeek-API-4A6CF7?logo=deepseek)](https://deepseek.com/)
[![Feishu](https://img.shields.io/badge/飞书-API-00C4B4?logo=lark)](https://open.feishu.cn/)

## 🚀 项目简介

**“新闻报通”** 是一个基于 n8n 构建的自动化 AI Agent。它能帮你追踪最新的 AI 动态，并结合你的飞书日程，智能地为你推荐感兴趣的内容和安排行程。当没有重大新闻时，它还会贴心地建议你放松身心。

**核心价值**：
- 📈 **信息聚合**：自动从飞书多维表格抓取过去 2 天的 AI 新闻。
- 📅 **日程感知**：同步你未来 7 天的飞书日历，了解你的忙碌程度。
- 🤖 **智能决策**：利用 DeepSeek 大模型分析新闻和日程，提供个性化建议。
- 💬 **即时推送**：将生成的每日简报以精美的 Markdown 卡片形式，通过飞书 Bot 推送给你。

## ✨ 功能特性

- **定时触发**：工作流可按设定时间（如每日上午 9 点）自动运行。
- **双源查询**：
    - 查询飞书多维表格中存储的 AI 新闻。
    - 查询飞书日历中近期的日程安排。
- **智能摘要**：由 DeepSeek 模型驱动，生成结构清晰、有洞察力的新闻摘要和行动建议。
- **数据标准化**：自动处理日期格式，确保所有数据源格式统一。
- **无代码/低代码编排**：整个逻辑通过 n8n 的可视化界面搭建，易于修改和扩展。

## 🛠️ 技术栈

- **[n8n](https://n8n.io/)** (v2.31.4)：工作流自动化编排的核心引擎。
- **[DeepSeek API](https://platform.deepseek.com/)**：提供大语言模型能力，用于生成智能建议。
- **[飞书开放平台](https://open.feishu.cn/)**：
    - **多维表格 (Bitable)**：作为新闻数据的存储和查询源。
    - **日历 API (Calendar API)**：用于获取用户的日程信息。
    - **飞书 Bot**：用于将最终结果推送给用户。

## 📋 前置要求

在导入和使用此工作流前，请确保你具备以下条件：

1.  **n8n 环境**：一个可用的 n8n 实例（推荐版本 `2.31.4` 或更高），可以是自托管或云版本。
2.  **飞书开发者权限**：
    - 一个飞书企业自建应用，并已获取 **App ID** 和 **App Secret**。
    - 该应用需启用并授权以下 API 权限：
        - `bitable:record:read` (读取多维表格记录)
        - `calendar:event:read` (读取日历事件)
    - 一个用于存储新闻的飞书多维表格（需包含 `标题`、`日期`、`媒体`、`内容`、`链接` 等字段）。
    - （可选）一个飞书 Bot，用于接收消息推送。
3.  **DeepSeek API 密钥**：一个有效的 DeepSeek API Key，并确保账户有足够余额。

## 🚀 快速开始：如何导入与使用

请按照以下步骤在你的 n8n 环境中部署此 Agent：

### 第一步：获取工作流文件
1.  在本仓库的根目录下，找到 `自动获取新闻流.json` `My workflow.json`文件。
2.  点击该文件，然后点击右上角的 “Raw” 按钮，或直接下载整个仓库的压缩包，以获取 JSON 文件内容。

### 第二步：导入工作流到 n8n
1.  登录你的 n8n 实例，进入工作流仪表盘。
2.  点击右上角的 **“Import from File”** (从文件导入) 按钮。
3.  选择你刚下载的 `news-broadcast-agent.json` 文件并导入。此时，工作流的节点和连接关系将被创建。

### 第三步：配置凭证 (Credentials)
这是最关键的一步，需要将你的实际 API 密钥填入 n8n：

1.  **飞书凭证**：
    - 在工作流中，找到调用飞书 API 的节点（如 `SearchRecords base in Lark`）。
    - 点击节点，在 **“Credential”** 下拉菜单中，选择 **“Create New”** (新建)。
    - 选择凭证类型为 **“Lark Tenant Token”**。
    - 填入你的飞书应用 **App ID** 和 **App Secret**，并保存。
2.  **DeepSeek 凭证**：
    - 找到 `AI Agent` 节点，在其 **“Credential”** 处新建凭证。
    - 选择类型为 **“Generic API”**，填入你的 DeepSeek API Key。
    - 确保节点中的 **“Model”** 名称（如 `deepseek-v3`）与你 API 密钥可用的模型一致。

### 第四步：配置参数 (Parameters)
根据你的实际情况调整节点参数：

1.  **飞书多维表格 ID**：在 `SearchRecords base in Lark` 节点中，将 **“多维表格 Token”** 和 **“数据表 ID”** 替换为你存储新闻的表格的对应值。
2.  **飞书日历 ID**：在 `GetEventList calendar in Lark` 节点中，填入你想要查询的飞书日历的 ID。
3.  **时间范围**：工作流默认查询“今天”的新闻和“未来7天”的日程。你可以根据需要调整 Code 节点中的日期计算逻辑。

### 第五步：设置触发器 (Schedule Trigger)
1.  找到工作流的起点节点，通常是 **“Schedule Trigger”**。
2.  双击该节点，设置你希望工作流自动运行的时间（例如，每天早上 8:30）。
3.  如果你只想手动测试，可以将触发器临时替换为 **“Manual Trigger”** (手动触发) 节点。

### 第六步：运行与测试
1.  保存工作流的所有更改。
2.  点击界面右上角的 **“Execute Workflow”** (执行工作流) 按钮。
3.  观察各节点的执行状态。如果一切配置正确，你最终会看到 `AI Agent` 节点输出一段结构化的 Markdown 文本。
4.  （可选）在最后一个节点后，你可以连接一个飞书 Bot 节点，将此 Markdown 内容发送到你的飞书聊天中。

## 📁 项目结构
news-broadcast-agent/
├── README.md # 项目说明文档（即本文件）
└── news-broadcast-agent.json # n8n 工作流导出文件


## 🤝 贡献与反馈

如果你有任何改进建议、发现了 Bug，或者想分享自己的扩展，欢迎通过以下方式参与：
- 在仓库中提交 **Issue**。
- Fork 本仓库，修改后提交 **Pull Request**。

## 📄 许可证

本项目仅供学习和个人使用参考。使用 n8n、DeepSeek 和飞书等服务时，请遵守各平台的相应服务条款。

## 🙏 致谢

- 感谢 **[n8n](https://n8n.io/)** 提供了强大的自动化编排平台。
- 感谢 **[DeepSeek](https://deepseek.com/)** 提供了出色的 AI 模型能力。
- 感谢 **[飞书](https://www.feishu.cn/)** 提供了开放的企业协作 API。
