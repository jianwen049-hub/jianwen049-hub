# 马健文 / Evans Owen

**AI / Agent Engineer** · LangChain · RAG · Tool-calling · Java & Python

AI Agent 与 RAG 方向工程师，Java 后端出身（支付 / 风控 / 航天工具链），现专注构建可运行的 Agent 系统并公开分享：问题是什么、方案怎么设计、为什么这样选、代码怎么拆。

*AI/Agent engineer building runnable agent systems and sharing the whole process in the open — the problem, the design, the trade-offs, and the code.*

## Featured Labs

| Repo | What |
|------|------|
| [rag-agent-lab](https://github.com/jianwen049-hub/rag-agent-lab) | 可运行的 LangChain RAG + Tool-calling Agent 实验场，附中文深度技术博客 · A runnable RAG + tool-calling agent lab with deep-dive technical write-ups |
| [ai-eval-lab](https://github.com/jianwen049-hub/ai-eval-lab) | AI 评测：golden sets、回归评测、badcase 收集 · Evals: golden sets, regression suites, and bad-case mining |
| [agent-observability](https://github.com/jianwen049-hub/agent-observability) | Agent 可观测性：traces/spans、脱敏、成本×延迟 · Observability: traces/spans, PII scrubbing, cost × latency |
| [llm-cost-latency](https://github.com/jianwen049-hub/llm-cost-latency) | LLM 成本与延迟：token 记账、缓存、路由、压缩 · Token accounting, caching, routing, and compression |
| [context-engineering](https://github.com/jianwen049-hub/context-engineering) | Context engineering：窗口预算、消息打包、记忆摘要 · Window budgeting, message packing, memory summarization |
| [hybrid-retrieval](https://github.com/jianwen049-hub/hybrid-retrieval) | 混合检索：稀疏 + 稠密 + RRF/线性融合重排 · Hybrid retrieval: sparse + dense + RRF/linear fusion |
| [multi-agent-orchestration](https://github.com/jianwen049-hub/multi-agent-orchestration) | 多 Agent 编排：planner/worker/critic、总线、黑板、重试 · Multi-agent orchestration: roles, bus, blackboard, retry |
| [spring-ai-lab](https://github.com/jianwen049-hub/spring-ai-lab) | Spring AI：Java 后端 ChatClient / Advisors / Tools / RAG / SSE · Spring AI for Java backends |
| [mcp-lab](https://github.com/jianwen049-hub/mcp-lab) | MCP：Tools/Resources、传输、权限边界与失败演练 · Model Context Protocol lab |
| [ai-stack-landscape](https://github.com/jianwen049-hub/ai-stack-landscape) | AI 技术栈全景对比：框架/Agent/检索/MCP/服务观测成本 · Stack landscape & trade-offs |

## Stack

按问题分层，记录我实际在用的东西：什么时候选它、代价是什么、想深挖去哪个 lab。不是清单，是踩过坑之后的取舍笔记。

### Application Frameworks · 应用框架

- **LangChain / LangGraph** — 搭 RAG + Agent 原型的默认起点，迭代最快；抽象层多、调试栈深，图状态机复杂到一定程度就该迁到 LangGraph → 深挖见 [rag-agent-lab](https://github.com/jianwen049-hub/rag-agent-lab)
- **Spring AI** — 存量 Java 服务里嵌 ChatClient / Advisors / Tools 的最省事路径；Python 侧生态更厚，它适合"服务本来就在 Spring 里"，而不是"为 AI 另起新栈" → [spring-ai-lab](https://github.com/jianwen049-hub/spring-ai-lab)
- **Bare SDK** — demo 一过、框架抽象开始挡路，就退回裸调用：可控、可调试，代价是 retry、结构化输出这些都得自己造一遍 → 对照实现见 [rag-agent-lab](https://github.com/jianwen049-hub/rag-agent-lab)

### Retrieval · 检索

- **RAG** — 私有知识问答的基本盘；难点从来不在"接上"，而在切块、元数据过滤和坏例挖掘 → [rag-agent-lab](https://github.com/jianwen049-hub/rag-agent-lab)
- **Hybrid（sparse + dense + RRF）** — 术语/关键词靠 sparse、语义靠 dense，RRF 融合几乎白拿一段精度；代价是两路索引都要养 → [hybrid-retrieval](https://github.com/jianwen049-hub/hybrid-retrieval)
- **Chroma / FAISS / pgvector** — 实验期 Chroma 起步最快，FAISS 顶得住规模但没有元数据过滤，pgvector 赢在"向量与业务数据同库"；没有万能选，看运维边界在哪 → [hybrid-retrieval](https://github.com/jianwen049-hub/hybrid-retrieval)

### Agents & Tools · 智能体与工具

- **Tool-calling** — 让模型动手的最低成本路径；真正花时间的不是接通，是 schema 设计和失败重试策略 → [rag-agent-lab](https://github.com/jianwen049-hub/rag-agent-lab)
- **Multi-agent（planner / worker / critic）** — 单 Agent 提示词堆不动时才上多角色编排；收益是职责隔离，代价是延迟 ×N、调试面变宽 → [multi-agent-orchestration](https://github.com/jianwen049-hub/multi-agent-orchestration)
- **MCP vs OpenAI tools** — 工具要跨模型、跨应用复用就选 MCP，锁单一供应商图省事就选 OpenAI tools；MCP 的传输、权限边界和失败模式值得单独演练一遍 → [mcp-lab](https://github.com/jianwen049-hub/mcp-lab)

### Evals & Observability · 评测与可观测性

- **Golden sets / 回归评测** — 没有 golden set 之前，一切"变好了"都只是感觉；维护成本持续存在，但不付这个钱就没有迭代资格 → [ai-eval-lab](https://github.com/jianwen049-hub/ai-eval-lab)
- **Traces / Spans** — Agent 出了问题不看 trace 等于盲调；把一次请求拆成 span，才分得清是检索烂还是提示词烂 → [agent-observability](https://github.com/jianwen049-hub/agent-observability)
- **Cost × Latency** — token 记账、缓存、模型路由三件事一起做，才谈得上"这个功能能不能上线"；只压延迟，常常把成本压没了 → [llm-cost-latency](https://github.com/jianwen049-hub/llm-cost-latency)

### Context & Serving · 上下文与服务化

- **窗口预算 / 消息打包 / 记忆摘要** — 上下文不是塞得越多越好：预算化 + 打包策略 + 滚动摘要，是长会话不崩的前提 → [context-engineering](https://github.com/jianwen049-hub/context-engineering)
- **Streaming / SSE** — 生成式接口的默认形态，首 token 体感延迟决定产品生死；Java 侧 SSE 管道的坑记在 → [spring-ai-lab](https://github.com/jianwen049-hub/spring-ai-lab)

### Languages / Runtime · 语言与运行时

- **Python** — 实验与图编排的主场，AI 生态最新最全，代价是工程化纪律要自己补齐。
- **Java / Spring Boot** — 生产服务的主场，稳、生态成熟、运维顺手，代价是 AI 侧生态总慢 Python 半拍。

完整的选型对比矩阵（框架 / Agent / 检索 / MCP / 观测成本）另开一仓慢慢养：**[ai-stack-landscape](https://github.com/jianwen049-hub/ai-stack-landscape)**。

扫一眼就够的话：`LangChain` · `RAG` · `Hybrid Retrieval` · `Chroma` / `FAISS` · `Tool-calling` · `Agent Orchestration` · `Multi-agent` · `MCP` · `Evals` / `Golden Sets` · `Traces` / `Spans` · `Cost × Latency` · `Python` · `Java` / `Spring Boot` · `Spring AI`

---
Start here → **[rag-agent-lab](https://github.com/jianwen049-hub/rag-agent-lab)**，其余 labs 按上面表格从评测 → 可观测性 → 成本延迟 → 上下文工程 → 混合检索 → 多 Agent → Spring AI → MCP → 技术栈全景一路读下去。
