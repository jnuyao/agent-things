# Agent 那些事儿：关键事件时间线（2017–2026）

> 这份时间线不是附录，而是整个项目的叙事骨架。它记录了 LLM 应用工程从 2017 年到 2026 年的 67 个关键事件，帮助读者把章节、概念和产业演进放回同一条时间轴上理解。

## 它和 README、章节的关系

- [`README.md`](README.md) 负责回答：这个项目是什么，为什么值得读，应该怎么读。
- 这份 `TIMELINE.md` 负责回答：这些技术为什么会在那个时间点出现，它们之间的历史顺序是什么。
- 各章 `README.md` 负责回答：某个具体问题是如何一步步被解决的，以及为什么自然会走向下一章。

你可以把它们理解成三个层级：

- `README` 给你入口
- `TIMELINE` 给你历史骨架
- 章节正文给你问题和解法

如果你是第一次进入这个仓库，推荐阅读顺序是：

1. 先看 [`README.md`](README.md)
2. 再看这份时间线
3. 然后回到各章节 `README.md`

## 这份时间线怎么用

- 当你想知道“某个技术为什么会出现”时，先看它前后的事件。
- 当你在章节之间切换时，用它确认这一章在整条演进链里的位置。
- 带 `🏷️` 的事件代表关键概念或术语的诞生点，适合在写作时反复引用。

<a id="stage-1"></a>
## 第一阶段：基础奠基（2017 — 2022）

从 Transformer 架构到 ChatGPT 发布前夜。LLM 的核心技术基石在这一阶段逐一就位。

| # | 时间 | 事件 | 意义 | 参考链接 |
| --- | --- | --- | --- | --- |
| 1 | 2017.06.12 | 🏷️ Transformer / "Attention Is All You Need" (Vaswani et al.) | 提出 Transformer 架构，奠定整个 LLM 时代的技术基石，引用超 17 万次 | [arXiv](https://arxiv.org/abs/1706.03762) |
| 2 | 2020.05 | 🏷️ RAG 概念诞生 (Lewis et al., Meta/Facebook AI) | "Retrieval-Augmented Generation" 首次被提出，将检索与生成结合，NeurIPS 2020 | [arXiv](https://arxiv.org/abs/2005.11401) |
| 3 | 2022.01.28 | 🏷️ Chain-of-Thought Prompting (Wei et al., Google) | 提出 CoT 思维链提示，让 LLM 通过逐步推理显著提升复杂问题表现，NeurIPS 2022 | [arXiv](https://arxiv.org/abs/2201.11903) |
| 4 | 2022.03 | 🏷️ RLHF / InstructGPT (Ouyang et al., OpenAI) | 确立“人类反馈强化学习”三阶段训练范式（SFT → RM → PPO），直接催生 ChatGPT | [arXiv](https://arxiv.org/abs/2203.02155) |
| 5 | 2022.10 | LangChain 首次发布 (Harrison Chase) | 首个 LLM 应用编排框架，定义了 Chain / Agent / Tool 范式 | [GitHub](https://github.com/langchain-ai/langchain) |
| 6 | 2022.10.06 | 🏷️ ReAct 论文 (Yao et al., Princeton / Google) | 提出 Reasoning + Acting 交替范式，成为后续所有 Agent Loop 的理论基础 | [arXiv](https://arxiv.org/abs/2210.03629) |
| 7 | 2022.11.30 | ChatGPT 发布 | LLM 进入大众视野，“能聊天但不能做事”的起点 | [OpenAI Blog](https://openai.com/blog/chatgpt) |

<a id="stage-2"></a>
## 第二阶段：工具与框架涌现（2023.01 — 2023.12）

ChatGPT 引爆行业后，Function Calling、Agent 原型、多 Agent 框架密集出现。这一年更像“概念验证之年”。

| # | 时间 | 事件 | 意义 | 参考链接 |
| --- | --- | --- | --- | --- |
| 8 | 2023.03.17 | Microsoft Semantic Kernel 公开 | 微软的 LLM 集成 SDK，C# / Python 双语言支持 | [GitHub](https://github.com/microsoft/semantic-kernel) |
| 9 | 2023.03.23 | ChatGPT Plugins 发布 | ChatGPT 的 “App Store 时刻”，第三方工具首次接入 LLM | [OpenAI Blog](https://openai.com/blog/chatgpt-plugins) |
| 10 | 2023.03.28 | BabyAGI 发布 | 最小化自主 Agent 原型，Task Creation + Prioritization + Execution 循环 | [GitHub](https://github.com/yoheinakajima/babyagi) |
| 11 | 2023.03.30 | AutoGPT 发布 (Toran Bruce Richards) | 首个全自主 GPT-4 Agent，21 天达 100K stars，引爆第一波 Agent 热潮 | [GitHub](https://github.com/Significant-Gravitas/AutoGPT) |
| 12 | 2023.04.07 | Stanford Generative Agents (Smallville) | 25 个 AI 居民在模拟小镇自主生活，奠定 Memory + Reflection + Planning 架构 | [arXiv](https://arxiv.org/abs/2304.03442) |
| 13 | 2023.05 | “Prompt Engineer” 成为年度热门职业 | WEF 将其列为 2023 年 #1 新兴职业，薪资高达 33.5 万美元 | [TIME](https://time.com/6272103/ai-prompt-engineer-job/) |
| 14 | 2023.06.13 | OpenAI 发布 Function Calling | 模型第一次能结构化地调用外部工具 | [OpenAI Blog](https://openai.com/blog/function-calling-and-other-api-updates) |
| 15 | 2023.06.30 | MetaGPT 发布 (DeepWisdom) | 多 Agent 框架，用 SOP 编码软件公司工作流，ICLR 2024 oral | [GitHub](https://github.com/geekan/MetaGPT) |
| 16 | 2023.09 | Microsoft AutoGen 发布 | 微软的多 Agent 对话框架，支持 Agent 间结构化协作 | [GitHub](https://github.com/microsoft/autogen) |
| 17 | 2023.11.06 | OpenAI DevDay：GPTs + Assistants API 发布 | 线程 + 工具 + 文件 + Code Interpreter，首次有状态 Agent 尝试 | [OpenAI Blog](https://openai.com/blog/new-models-and-developer-products-announced-at-devday) |
| 18 | 2023.12 | RAG 论文与实践主流化 | Naive RAG 成为 LLM 接入私有知识的标配方案 | 行业趋势 |
| 19 | 2023.12 | CrewAI 首版发布 | 基于角色的多 Agent 框架，简化 Agent 团队编排 | [GitHub](https://github.com/joaomdmoura/crewAI) |
| 20 | 2023.12.29 | GitHub Copilot Chat GA | AI Pair Programming 主流化，IDE 内对话式编码 | [GitHub Blog](https://github.blog/2023-12-29-github-copilot-chat-now-generally-available-for-organizations-and-individuals/) |

<a id="stage-3"></a>
## 第三阶段：能力跃升与协议萌芽（2024.01 — 2024.12）

模型能力大幅提升，长上下文、多模态和推理能力快速增强；与此同时，MCP 诞生，Agent 开始从概念走向产品。

| # | 时间 | 事件 | 意义 | 参考链接 |
| --- | --- | --- | --- | --- |
| 21 | 2024.01.10 | GPT Store 上线 | GPTs 应用商店，验证了“Agent 分发”模式 | [OpenAI Blog](https://openai.com/blog/introducing-the-gpt-store) |
| 22 | 2024.01.17 | LangGraph 发布 (LangChain) | 基于图的 Agent 编排框架，解决 LangChain 在复杂工作流中的局限 | [LangChain Blog](https://blog.langchain.dev/langgraph/) |
| 23 | 2024.02.15 | Gemini 1.5 Pro 预览版 | 100 万 token 上下文窗口，改变了 RAG 的必要性边界 | [Google Blog](https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/) |
| 24 | 2024.03.04 | Claude 3 系列发布 | Opus / Sonnet / Haiku 三档模型，Opus 首次在多项基准超越 GPT-4 | [Anthropic](https://www.anthropic.com/news/claude-3-family) |
| 25 | 2024.03 | Devin（首个 AI 软件工程师）发布 | 验证了 Agent 自主完成复杂编码任务的可能性 | [Cognition Blog](https://www.cognition-labs.com/blog) |
| 26 | 2024.03 | Andrew Ng 推广 “Agentic AI” 概念 | 提出 Agentic Workflows 四大模式（Reflection / Tool Use / Planning / Multi-Agent） | [X/Twitter](https://x.com/AndrewYNg) |
| 27 | 2024.05.13 | GPT-4o 发布 | 多模态原生模型（文本 + 视觉 + 语音），Agent 感知能力跃升 | [OpenAI Blog](https://openai.com/index/hello-gpt-4o/) |
| 28 | 2024.06 | Microsoft GraphRAG 开源 | RAG 从纯向量检索进化到图谱推理 | [GitHub](https://github.com/microsoft/graphrag) |
| 29 | 2024.06.20 | Claude 3.5 Sonnet 发布 | 性能超越 Opus，成为 2024 年编码能力最强模型之一 | [Anthropic](https://www.anthropic.com/news/claude-3-5-sonnet) |
| 30 | 2024.08.06 | OpenAI Structured Outputs | Function Calling 进化：保证 100% schema 合规的 JSON 输出 | [OpenAI Blog](https://openai.com/index/introducing-structured-outputs-in-the-api/) |
| 31 | 2024.08 | Cursor 完成 6000 万美元 Series A（估值 4 亿美元） | IDE-native AI 编码工具崛起，验证了“Agent 嵌入开发者工作流”的商业模式 | [TechCrunch](https://techcrunch.com/2024/08/22/cursor-an-ai-code-editor-raises-60m/) |
| 32 | 2024.09.12 | OpenAI o1 推理模型预览 | 首个 Chain-of-Thought 推理模型，开启“思考后再答”新范式 | [OpenAI Blog](https://openai.com/index/learning-to-reason-with-llms/) |
| 33 | 2024.10.22 | 升级版 Claude 3.5 Sonnet + Computer Use | 模型直接操控桌面 GUI，Agent 从 API 调用扩展到“像人类一样操作电脑” | [Anthropic](https://www.anthropic.com/news/3-5-models-and-computer-use) |
| 34 | 2024.11 | Windsurf Editor 发布 (Codeium) | 首个 Agentic IDE，结合 Copilot 协作与自主 Agent 能力 | [Windsurf](https://codeium.com/windsurf) |
| 35 | 2024.11.25 | 🏷️ Anthropic 发布 MCP（Model Context Protocol） | 模型接入外部世界的第一个开放标准协议 | [Anthropic](https://www.anthropic.com/news/model-context-protocol) |
| 36 | 2024.12.20 | Anthropic《Building Effective Agents》博文 | Agent 系统设计的权威指南，定义了 workflows vs agents 的关键区分 | [Anthropic](https://www.anthropic.com/research/building-effective-agents) |
| 37 | 2024.12.26 | DeepSeek V3 发布 | 开源模型（MIT），训练成本极低但性能逼近前沿，震动行业 | [GitHub](https://github.com/deepseek-ai/DeepSeek-V3) |

<a id="stage-4"></a>
## 第四阶段：Agent 产品化与协议栈成型（2025.01 — 2025.12）

Agent 从实验走向产品，协议开始分层，开源 Agent 生态迅速繁荣。

| # | 时间 | 事件 | 意义 | 参考链接 |
| --- | --- | --- | --- | --- |
| 38 | 2025.01.20 | DeepSeek R1 与推理模型浪潮 | 强化学习 + 长链推理成为模型端的新趋势 | [GitHub](https://github.com/deepseek-ai/DeepSeek-R1) |
| 39 | 2025.02.06 | 🏷️ Vibe Coding 概念诞生 (Andrej Karpathy) | “完全放弃手写代码，接受 AI 所有 diff”，后被选为 2025 Collins 年度词汇 | [X/Twitter](https://x.com/karpathy) |
| 40 | 2025.02.24 | Claude Code 发布（研究预览版） | 终端里的 Agentic 编码工具，Agent 产品化里程碑 | [Anthropic](https://www.anthropic.com/products/claude-code) |
| 41 | 2025.02 | GitHub Copilot Agent Mode 发布 | VS Code 中的 Agent 模式，迭代式多文件编辑 + 错误自修复 | [GitHub Blog](https://github.blog) |
| 42 | 2025.03.06 | Manus 发布 (Butterfly Effect) | 首个通用自主 AI Agent，GAIA 基准 86.5%，CodeAct 架构 | [manus.im](https://manus.im) |
| 43 | 2025.03 | Manus 系统提示词泄露 | 多 Agent 架构细节曝光，Prompt Injection 安全问题引发行业关注 | [MIT Technology Review](https://www.technologyreview.com/) |
| 44 | 2025.03.11 | OpenAI Responses API + Agents SDK | OpenAI 的 Agent 框架正式入局 | [OpenAI Blog](https://openai.com/blog) |
| 45 | 2025.04 | MCP Tool Poisoning Attacks (Invariant Labs) | 首个 MCP 安全漏洞类别：工具描述中嵌入恶意指令可劫持 Agent 行为 | [Invariant Labs](https://invariantlabs.ai) |
| 46 | 2025.04.09 | 🏷️ Google 发布 A2A（Agent-to-Agent Protocol） | 50+ 合作伙伴加入，Agent 间协作的开放标准开始形成 | [Google Blog](https://blog.google) |
| 47 | 2025.04.16 | OpenAI o3 / o4-mini + Codex CLI 开源 | 推理模型迭代 + 终端编码工具开源，Agent 推理能力再升级 | [OpenAI Blog](https://openai.com/blog) |
| 48 | 2025.05 | 🏷️ IBM 发布 ACP（Agent Communication Protocol） | 强调结构化对话和异构系统协调 | [GitHub](https://github.com/ibm/acp) |
| 49 | 2025.05.22 | Claude Opus 4 / Sonnet 4 发布 | Anthropic 新一代模型，Agent 编码能力大幅提升 | [Anthropic](https://www.anthropic.com) |
| 50 | 2025.05 | OpenAI 收购 Windsurf（约 30 亿美元） | AI 编码工具并购潮的标志性事件 | [Bloomberg](https://www.bloomberg.com) |
| 51 | 2025.06.18 | 🏷️ Context Engineering 概念流行 | Tobi Lutke 与 Karpathy 推动，“设计 LLM 完整信息环境”取代“提示词工程” | [X/Twitter](https://x.com) |
| 52 | 2025.08 | GPT-5 发布 | OpenAI 新一代旗舰模型 | [OpenAI Blog](https://openai.com/blog) |
| 53 | 2025.08 | 🏷️ Zed + Google 发布 ACP（Agent Client Protocol） | IDE ↔ Agent 的标准协议，AI 编码工具的 “LSP 时刻” | [Zed Blog](https://zed.dev/blog) |
| 54 | 2025.08 | IBM ACP 并入 A2A（Linux Foundation） | Agent 间通信协议整合，A2A 逐步成为统一标准 | [LF AI & Data](https://lfaidata.foundation) |
| 55 | 2025.10 | JetBrains 加入 ACP 协议 | ACP 覆盖 JetBrains 全系 IDE，成为 IDE-Agent 集成的事实标准 | [JetBrains Blog](https://blog.jetbrains.com) |
| 56 | 2025.11 | OpenClaw 创建 (Peter Steinberger) | 开源 Agent 系统，3 个月达 200K+ stars，历史最快增长 GitHub 项目之一 | [GitHub](https://github.com) |
| 57 | 2025.11 | AI System Prompt 泄露仓库达 134K stars | 28+ AI 编码工具的提示词被收集，成为 Agent 设计的学习资料 | [GitHub](https://github.com) |
| 58 | 2025.12.09 | MCP 捐赠给 Linux 基金会 / AAIF 成立 | Anthropic + Block + OpenAI 共同发起，MCP 月下载超 1 亿次 | [Anthropic](https://www.anthropic.com) |
| 59 | 2025.12.29 | Meta 收购 Manus（约 20 亿美元） | 通用自主 Agent 能力被大厂整合 | [CNBC](https://www.cnbc.com) |

<a id="stage-5"></a>
## 第五阶段：Agent OS 与多 Agent 时代（2026.01 — 2026.03）

Agent 系统从单体走向操作系统化，多 Agent 协作成为默认能力，产品级架构通过一系列公开事件被行业学习。

| # | 时间 | 事件 | 意义 | 参考链接 |
| --- | --- | --- | --- | --- |
| 60 | 2026.01 | OpenClaw 作为开源 Agent 系统崛起 | 开源 Claude Code 替代品，集成 Skills / MCP / 多 Agent | [Wikipedia](https://en.wikipedia.org) |
| 61 | 2026.01.16 | Cursor Agent Mode (CLI) 发布 | Cursor 进入终端 Agent 赛道，云端 Agent + 本地 CLI 并行发展 | [Cursor Changelog](https://changelog.cursor.com) |
| 62 | 2026.02 | Multi-Agent Week | 两周内 Grok Build、Windsurf、Claude Code Agent Teams、Codex CLI、Devin 同时发布多 Agent 功能 | 行业综合事件 |
| 63 | 2026.02.05 | Claude Opus 4.6 发布 | Anthropic 模型持续迭代，Agent Teams 功能上线 | [Anthropic](https://www.anthropic.com) |
| 64 | 2026.02.05 | 🏷️ Mitchell Hashimoto《Engineer the Harness》博文 | 命名 Harness Engineering 概念：为 Agent 建立工程化约束和反馈闭环 | [Blog](https://mitchellh.com) |
| 65 | 2026.02.11 | OpenAI 发布 Harness Engineering 报告 | 内部项目 100 万行代码零人工编写，确立 Harness Engineering 为 Agent 时代的新工程范式 | [OpenAI Blog](https://openai.com/blog) |
| 66 | 2026.03.25 | MCP 生态里程碑 | 97M 月安装量，5800+ MCP Server，生态全面成熟 | [Anthropic](https://www.anthropic.com) |
| 67 | 2026.03.31 | Claude Code 源码泄露事件 | npm 包意外包含 59.8MB source map，架构细节全曝光 | [The Register](https://www.theregister.com) |

<a id="chapter-map"></a>
## 时间线与章节映射

这张表用于回答一个实际写作问题：某个事件更适合放进哪一章，它在那一章里承担什么作用。

| 时间阶段 | 主要对应章节 | 核心主题 |
| --- | --- | --- |
| 2017 — 2022 | Ch0 被困在房间里的天才 | LLM 的能力边界与“只会聊天”的起点 |
| 2023.03 — 2024.08 | Ch1 Function Calling | 工具调用从提示词技巧走向结构化接口 |
| 2020.05 — 2024.06 | Ch2 RAG | 外部知识接入从基线方案走向高级检索 |
| 2024.11 — 2026.03 | Ch3 MCP | 工具与数据接入的标准化，以及 MCP 生态形成 |
| 2022.10 — 2025.06 | Ch4 / Ch5 / Ch6 Agent、Skill、Memory | 多步执行、能力封装与长期记忆逐步成形 |
| 2024.06 — 2025.04 | Ch7 Agentic RAG | 检索从静态管线进入 Agent 驱动阶段 |
| 2023.06 — 2026.02 | Ch8 Multi-Agent 与协议全景 | 多 Agent 编排与协议分层逐步成型 |
| 2025.02 — 2026.03 | Ch9 Agent OS | 从零件堆叠到产品级 Agent 系统 |
| 2025.04 — 2026.03 | Ch10 工程实战 | 安全、可观测、成本和恢复能力的系统化 |

<a id="protocol-stack"></a>
## 四层协议栈速查

这是全书里最值得反复回看的结构之一。它帮助读者理解：协议不是在竞争“谁取代谁”，而是在不同层解决不同问题。

| 层级 | 协议 | 方向 | 解决的问题 | 代表时间 |
| --- | --- | --- | --- | --- |
| Layer 1 | Function Calling | LLM → Function | 模型如何调用单个函数 | 2023.06 OpenAI |
| Layer 2 | MCP | Agent → Tool / Data | 如何标准化工具和数据接入 | 2024.11 Anthropic |
| Layer 3 | A2A | Agent ↔ Agent | 多 Agent 如何发现彼此并协作 | 2025.04 Google |
| Layer 4 | ACP | Human(IDE) ↔ Agent | 人类如何在 IDE 或客户端里使用 Agent | 2025.08 Zed + Google |
