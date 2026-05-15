# OpenClaw 凉了，Hermes 当立

> 日期：2026-04-09

## 写在前面

大家还记得 2、3 月份 OpenClaw 的盛世么？

那时候技术圈几乎人手一个，GitHub star 数量一路狂飙，媒体争相报道，自媒体疯狂刷屏。所有人都在说：开源 AI Agent 的时代来了，OpenClaw 就是那个标杆。

然后——

3 月 31 号之后，我在 TG 上用 OpenClaw 的次数基本归零了。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409050232135.png" alt="image-20260409050232135" style="zoom:50%;" />

这不是我一个人的感受。Twitter 上的风向、社区的讨论热度、GitHub 的 issue 数量——都在说明同一件事：OpenClaw 的增长曲线，开始 flat（平）了。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409085112401.png" alt="image-20260409085112401" style="zoom:50%;" />

这篇文章主要聊三件事情：**OpenClaw 为什么不行了，Hermes Agent 是什么，以及这场 AI Agent 的新战局会怎么走。**

废话不多说，先给大家把链接甩出来：

https://hermes-agent.nousresearch.com/docs/getting-started/quickstart/

---

## OpenClaw 为什么从神坛跌落？

首先要提到一点的是，OpenClaw 不是烂，它本质上是一个**控制平面优先**的优秀框架。

它可以直连操作系统——读写文件、控制浏览器、操作 Shell。可以接入各种 LLM ，也可以配合 Ollama 使用，数据从头到尾不出你的机器。Skill 插件系统靠人工编写，采用文件驱动的身份系统 Soul.md 。这套设计对程序员非常友好，一切皆文件的思路让调试和审计都很直观。

那它为什么不行了？

**版本迭代引入了稳定性问题。** 这是我在各个社区和群里听到提到最多的原因。3 月前后的几个大版本在功能上做了不少改动，但也顺手带入了新的 bug。用户量大了之后，各种边缘场景暴露出来，issue 堆得越来越多，响应速度跟不上。

**技术债的雪球越滚越大。** OpenClaw 的架构设计追求成年人啥都要——既要控制文件系统，又要管浏览器，又要支持 Skill 插件系统，还要保证安全沙盒——这些目标之间本身就有张力。在没有足够多资源维护的情况下，顾此失彼就成了常态。

**更深层的原因：OpenClaw 的设计哲学是「人必须在决策环里」。** 这在早期是优势，是卖点。但当用户量大了之后，大多数普通用户并不想在每一步操作前都停下来确认。他们想要的是：说一声，事情办了。OpenClaw 给不了这个。

**最后是竞品的冲击。** Hermes Agent 这类新选手在开箱即用和智能进化上打了差异化的牌，把 OpenClaw 的目标用户分流了一部分。

---

## Hermes Agent 是什么？

Hermes Agent 是一个开源、自托管的持久型 AI Agent。

它和 OpenClaw 一样：开源、自托管、能接入 Telegram / Discord / Slack / WhatsApp 等聊天软件执行实际任务、数据不离你的机器。但两者的设计哲学，在根子上就不一样。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409050524628.png" alt="image-20260409050524628" style="zoom:50%;" />

**OpenClaw 的核心逻辑是：Agent = 你给工具 + 你给 Skill + 你全程盯着。**

**Hermes 的核心逻辑是：Agent = 你给目标 + 它自己学 + 用久了比你自己还懂你。**

这个差异听起来有点虚，举个例子你就明白了。

你用 OpenClaw 跑一个任务：你要告诉它用什么 Skill、文件放哪、Shell 怎么跑。下次做同类任务，这些你还得再说一遍。它是工具，很好用，但不会「长记性」。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409085248718.png" alt="image-20260409085248718" style="zoom:50%;" />

你用 Hermes 跑同一个任务：你只要说目标，它自己决定用什么方法。任务完成后，它会**把这次成功的执行路径提炼成一个 Skill**，存进技能库里。下次做同类任务，你不用说，它自己调用。而且如果你用新方法把效果做得更好，它会自动更新那个 Skill——旧的不如新的，存新的。

这就是 Hermes 最核心的卖点：**从经验中学习，在使用中进化。**

---

## 核心架构拆解

咱们这次不讲虚的，直接按官网文档把 Hermes 的架构拆成几层。你会发现它其实不是“一个会聊天的命令行工具”，而是一套把模型、工具、记忆、技能、自动化和安全边界拼到一起的 Agent runtime。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409085456767.png" alt="image-20260409085456767" style="zoom:50%;" />

### 第一层：运行时入口层

从官方 Quickstart 和 Features Overview 来看，Hermes 最外层其实是一个统一入口层。

这个入口层不只是一套 CLI 命令，它至少覆盖了三类交互面：

- CLI 会话：直接跑 `hermes` 进入交互式终端，支持多行输入、任务中断、`hermes --continue` 恢复最近会话
- 消息平台入口：通过 `hermes gateway setup` 接 Telegram、Discord、Slack、WhatsApp，把同一个 Agent 暴露到不同消息渠道
- 自动化入口：可以直接用自然语言创建定时任务，让 Hermes 通过 gateway 或 cron 在后台周期性执行

也就是说，Hermes 不是“先做个命令行，后面再补 Bot”，而是从一开始就把 CLI、消息平台和自动化视为同一套 runtime 的不同入口。这一点很关键，因为它决定了 Hermes 的状态、记忆、技能和权限控制都得跨入口复用。

### 第二层：模型与工具执行层

官方 Quickstart 里给出的供应商选择其实已经暴露了它的底层结构。Hermes 把“模型调用”和“工具调用”分成了两件事：

- 模型侧支持 Nous Portal、OpenAI Codex、OpenRouter，以及任何 OpenAI 兼容端点
- 执行侧则是工具系统，官网 Overview 里明确列了 web search、terminal execution、file editing、browser automation、memory、delegation 这些能力。

这意味着 Hermes 的内核不是绑定某一家模型，而是把模型当“推理大脑”，把工具当“行动器官”。

你可以把这一层理解成：

1. 模型负责理解目标、规划步骤、判断是否要调用工具  
2. 工具负责真的去搜网页、改文件、跑终端、开浏览器  
3. 结果再回流给模型，进入下一轮推理

这也是为什么它能一边接 OpenAI Codex，一边又强调自带 web search、terminal、browser 这些执行能力。Hermes 的重点不是“聊天模型接得多”，而是“模型和工具之间的闭环完整”。

### 第三层：上下文装配层

这一层是很多人会忽略，但其实很像“控制中枢”的地方。

官网 Features Overview 里提到 Hermes 会自动发现并加载项目里的上下文文件，比如 `.hermes.md`、`AGENTS.md`、`CLAUDE.md`、`SOUL.md`、`.cursorrules`。它还支持用 `@` 引用文件、文件夹、git diff 和 URL，把这些内容动态注入当前消息。

这说明 Hermes 在正式推理前，会先做一轮上下文装配：

- 先吃系统级配置和项目规则
- 再吃用户当前显式引用的文件和变更
- 再把记忆、技能、外部 provider 上下文拼进去
- 最后才把完整上下文交给模型

这一步非常像一个“prompt assembly pipeline”，不是单纯地把用户一句话丢给模型。

也是因为有这一层，Hermes 才能做到既像编程助手，又像自动化助手，还像聊天 Bot。不同任务的差异，很多不是靠换模型，而是靠换上下文装配结果。

### 第四层：持久记忆层

这是 Harmes 区别于 OpenClaw 最大的点。

这部分官网写得最清楚，也最值得细讲。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409085611925.png" alt="image-20260409085611925" style="zoom:50%;" />

Hermes 的内建记忆不是一个模糊概念，而是两份固定文件加一套历史检索机制：

- `MEMORY.md`：Agent 的环境记忆，存项目结构、工具坑点、工作流经验、已经验证过的方法
- `USER.md`：用户画像，存偏好、沟通方式、习惯、雷区、技术水平

两份文件默认都在 `~/.hermes/memories/`，而且不是实时乱改的“内存”，而是**在会话开始时作为 frozen snapshot 注入 system prompt**。这点很重要，因为它意味着 Hermes 对持久记忆的使用是受控的、稳定的，不会一边推理一边把 prompt 基础设施搞乱。

官网还给了明确的容量设计：

- `MEMORY.md` 默认 2200 字符，约 800 tokens
- `USER.md` 默认 1375 字符，约 500 tokens

超了怎么办？不是无限追加，而是让 Agent 自己 consolidate 或替换旧条目。也就是说，Hermes 官方对记忆的理解不是越多越好，而是要把常驻上下文压缩成真正重要的那一小块。

除此之外，还有第二条线：历史会话搜索。

官方说明 Hermes 会把所有会话存进 SQLite 的 `state.db`，并用 FTS5 做全文检索。真正需要“我们上周是不是讨论过数据库索引策略”这种问题时，它走的是 `session_search` 这条路径，再让模型做摘要，而不是把所有旧对话提前塞进 prompt。

所以 Hermes 的记忆层其实是双轨的：

- 常驻记忆：小而稳定，始终在上下文里
- 历史检索：大而按需，只有需要时才拉进来

这一层的工程味很重，也很像真正能长期运行的 Agent 系统。

### 第五层：外部记忆 Provider 层

如果你以为 Hermes 的记忆就只有两个 markdown 文件，那就低估它了。

官网 Memory Providers 页面写得很明确：内建的 `MEMORY.md` / `USER.md` 会一直开着，同时还可以外挂一个外部 memory provider。当前文档里列了 8 个 provider，Honcho 只是其中之一。

当外部 provider 启用时，Hermes 会自动做六件事：

1. 把 provider 知道的上下文注入 system prompt  
2. 在每轮对话前预取相关记忆  
3. 在每次响应后把对话同步给 provider  
4. 会话结束时抽取长期记忆  
5. 把内建记忆写入镜像到 provider  
6. 给 Agent 暴露 provider 专属工具

这一层的意义在于：Hermes 没把记忆架构写死，而是做成了插件化接口。

尤其是 Honcho，官方把它定位成“built-in memory 之上的深层用户建模层”。它能做的是：

- 通过 dialectic reasoning 在会后归纳用户习惯和目标
- 做跨会话、跨平台甚至多 Agent 的用户画像
- 用 `honcho_search`、`honcho_context`、`honcho_profile`、`honcho_conclude` 等工具进行语义级记忆检索和结论管理

这意味着 Hermes 的长期方向不是“把本地记忆做大”，而是“把内建轻记忆和外部深记忆分层”。这个架构比很多一体化的 Agent 方案要成熟。

### 第六层：技能与程序性记忆层

如果说持久记忆存的是“你是谁、环境是什么、哪些事实重要”，那技能层存的就是“事情到底怎么做”。

官网对 Skills 的定义很直接：它是按需加载的知识文档，遵循 progressive disclosure，默认都放在 `~/.hermes/skills/` 下。这里最关键的不是“技能是 markdown 文件”，而是 Hermes 把技能明确当成了**procedural memory**。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409085819605.png" alt="image-20260409085819605" style="zoom:50%;" />

官方文档里说得非常直白：

- 完成复杂任务后，Agent 可以创建 Skill
- 试错后找到正确路径，可以保存成 Skill
- 用户纠正了做法，也可以更新 Skill
- Agent 甚至可以删除过时 Skill

也就是说，技能层其实是 Hermes 对经验沉淀的正式建模。

记忆层回答的是：

- 用户是谁
- 项目是什么
- 过去讨论过什么

技能层回答的是：

- 这类任务该怎么做
- 哪条流程以前走通过
- 哪些坑已经踩过

这个分层很漂亮。因为如果你把“用户喜欢简洁回答”存成 Skill 就太重了；反过来，如果你把“如何部署 Kubernetes 服务”塞进 MEMORY.md 又太脏。Hermes 官方把这两类东西拆开，实际上就是在区分**事实记忆**和**程序记忆**。

### 第七层：自动化与并行执行层

Hermes 还有一层经常被忽略，就是自动化层。

官网 Features Overview 里明确列了这些能力：

- Scheduled Tasks：通过自然语言或 cron 表达式创建定时任务
- Subagent Delegation：用 `delegate_task` 启动子 Agent，隔离上下文并限制工具集
- Code Execution：让 Agent 写 Python 脚本，通过沙箱化 RPC 一次完成多步工具调用
- Batch Processing：批量并行跑大量 prompt

这说明 Hermes 并不满足于“单轮问答”。它已经把 Agent 进一步做成了一个可调度系统：

- 可以定时跑
- 可以拆子任务并行跑
- 可以把多轮工具调用折叠成程序执行
- 可以批处理

从架构上看，这一层就是把 Hermes 从“交互式助手”推向“可运营的任务系统”的关键。

### 第八层：安全与隔离层

最后一层是安全边界，官网 Security 页面把这件事讲得很明白：Hermes 采用的是 defense-in-depth，而不是单点防护。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260409085849194.png" alt="image-20260409085849194" style="zoom:50%;" />

它把安全分成五层：

1. 用户授权  
2. 危险命令审批  
3. 容器隔离  
4. MCP 凭证过滤  
5. 上下文文件扫描

展开看，每一层都不是装饰：

- 用户授权控制的是谁能跟 Agent 说话，支持 allowlist 和 DM pairing
- 危险命令审批控制的是哪些操作必须人在环内确认
- 容器隔离控制的是命令到底跑在本机、Docker、Singularity 还是远程 SSH
- MCP 凭证过滤控制的是外部 MCP 子进程不该看到哪些环境变量
- 上下文文件扫描控制的是项目里的恶意提示注入

这套设计说明 Hermes 并没有因为“自治”就放弃“边界”。恰恰相反，它把最危险的几个边界都显式化了。

### 一句话总结这套架构

如果非要用一句工程语言总结 Hermes，我会这么说：

**Hermes 是一个多入口的 Agent runtime，上面挂着工具执行层、上下文装配层、双轨记忆层、可进化技能层、自动化调度层，底下再用纵深防御安全模型兜底。**

所以它和 OpenClaw 的真正差别，不只是“一个更聪明，一个更啰嗦”，而是 Hermes 在官方架构上已经把“长期运行、长期学习、长期复用”这三件事当成了第一性原则。

---

## OpenClaw vs Hermes：全维度对比

### 核心设计哲学

**OpenClaw**：控制平面优先，人在决策链中心。所有操作需要显式授权，文件驱动的身份系统（SOUL.md）。人定义规则，Agent 执行规则。

**Hermes Agent**：学习循环优先，Agent 自动进化。用目标驱动，用结果反馈，用经验优化。人定义目标，Agent 自己找最优路径。

### Skill 系统

**OpenClaw**：人工编写 Skill，靠文件驱动。开发者写模板，Agent 按模板跑。灵活但依赖人工维护。

**Hermes**：从成功经验中自动生成 Skill，持续优化。Skill 会随使用进化，不依赖人工维护。

### 记忆能力

**OpenClaw**：显式文件记录，需要人工整理维护。没有跨会话学习能力。

**Hermes**：分层记忆系统，跨会话保留偏好和经验。越用越懂你。

### 安全模型

**OpenClaw**：高控制，高权限。文件层面可审计所有行为，但依赖人工配置。

**Hermes**：安全沙盒 + 审批检查，默认配置更保守。对非专业用户更友好。

### 部署难度

**OpenClaw**：有一定门槛。需要折腾配置，理解 SOUL.md 机制。

**Hermes**：更轻量，开箱即用，内置 cron 等实用功能。

### 适用人群

| | OpenClaw | Hermes Agent |
|--|---------|-------------|
| 目标用户 | 硬核开发者、隐私敏感场景 | 想省事的团队、个人用户 |
| 技术背景 | 需要一定折腾能力 | 半技术、小白都友好 |
| 维护成本 | Skill 靠人工写，长期维护成本高 | 自动进化，维护成本低 |
| 透明度 | 高，所有操作文件层面可见 | 中，Skill 生成过程是黑盒 |
| 灵活性 | 极高，SOUL.md 可完全定制 | 中，依赖内置进化机制 |

---

## Hermes 的局限

Hermes 也有它的问题，不能盲目吹。

**Skill 进化的黑盒问题。** 它的 Skill 生成和优化机制是自动的，这意味着你很难完全理解「为什么这个 Skill 是这样写的」。当 Hermes 生成了一个你不认可的 Skill 时，你没有像 OpenClaw 那样直接改文件的能力。

**「成长」需要时间。** Hermes 用久了确实会更懂你，但这个`久`是真的需要一段时间积累。如果你的使用场景是一次性的、或者用几次就不用了，Hermes 的学习能力体现不出来。

**对极度隐私敏感的场景仍有局限。** 虽然 Hermes 支持本地部署，但它的 Skill 自动生成机制涉及将执行路径数据写进技能库——这意味着你的使用习惯本身会被结构化存储。对于极度保守的安全要求，这个存储本身也需要保护。

---

## 怎么选

说到底，两者的差异是**「人治」和「自治」的差异**。

选 OpenClaw 的情况：
- 你对 Agent 每一步操作需要有完全的可见性和控制权
- 有特殊的安全合规要求，需要在文件层面审计所有行为
- 想深度定制 Agent 的能力，开发自己的 Skill 系统
- 你的场景需要和操作系统、浏览器深度耦合
- 你有时间和精力折腾配置

选 Hermes Agent 的情况：
- 你希望 Agent 能随着使用越来越懂你的工作方式
- 不想折腾，想快速部署
- 需要定时自动化任务，内置 cron 调度器很实用
- 你需要一个更安全的默认配置，减少人工审核负担
- 团队规模不大，追求轻量灵活

**务实建议：可以混用。**

用 OpenClaw 做主控层，它适合处理需要精确控制的高风险任务。用 Hermes Agent 做轻量执行节点，负责那些重复性的、可以自动学习优化的日常任务。实验性功能隔离到 Hermes 节点上，方便回滚而不影响核心业务。

---

## 我的看法

OpenClaw 代表了一种非常合理的思路：**把控制权还给人类**。它的所有设计——SOUL.md、显式授权、文件审计——都围绕同一个命题：人应该知道 Agent 在做什么，也应该能够干预 Agent 的每一步。

这是对的。尤其对技术出身的用户来说，这种控制感是信任的基础。

Hermes 代表另一种思路：**让 Agent 真正有用**。它承认了一个现实——大多数人没有时间精力去精细化管理一个 Agent，也不想每次都要停下来审批。所以它把「自主学习」做成了内置能力，把重复性的决策自动化，把人从「监控者」变成「监督者」。

两种思路都有市场，也都有局限。

OpenClaw 的问题是：它假设了用户愿意且能够持续介入。实际上，大多数人不愿意。开个电脑还要审批一堆东西，时间久了就烦了，要么关掉安全机制，要么干脆不用。

Hermes 的问题是：它把「自动进化」做得很漂亮，但这个进化过程对用户是不透明的。Agent 学了什么、为什么这么学、某次决策的依据是什么——这些都是黑盒。用户要么选择完全信任，要么选择不用。中间地带很小。

短期看，OpenClaw 在硬核开发者社区还会是主流，尤其在隐私合规要求高的场景里。长期看，能「进化」的 Agent 范式会逐渐成为主流——问题是，这个「进化」机制的透明度和可控性，是它能不能真正普及的关键。

Hermes 已经走出了第一步。OpenClaw 不会死，但它需要解决的不只是技术债，还有产品思路上的转型。

技术选型没有标准答案。先搞清楚你的场景到底需要什么，再决定用哪个。

对了这里要多说一句话，把 OpenClaw 带火的都是中国区用户，那这次的 Hermes 呢？

---

## 参考来源

1. [2026 年AI Agent 双雄对决：OpenClaw vs Hermes Agent 怎么选？](https://cloud.tencent.com/developer/news/3700465) - 腾讯云开发者社区
2. [狂揽28.4k Star！OpenClaw最新平替Hermes Agent开源](https://zhuanlan.zhihu.com/p/2024913544848623338) - 知乎专栏
3. [OpenClaw vs Hermes Agent: A Thorough Comparison for Your Use](https://www.youtube.com/watch?v=eJzQuC9Dy3w) - YouTube
4. [OpenClaw vs. Hermes Agent: The race to build AI assistants](https://thenewstack.io/persistent-ai-agents-compared/) - The New Stack
5. [Hermes Agent – OpenClaw's Rival? Differences and Best Use Cases](https://www.turingpost.com/p/hermes) - Turing Post
6. [Hermes Agent 的逆袭：轻量、跨平台、可定制](https://www.sohu.com/a/1004153546_122413774) - 搜狐
7. [What Is Hermes Agent? The OpenClaw Alternative with a Built-In Brain](https://www.mindstudio.ai/blog/what-is-hermes-agent-openclaw-alternative/) - MindStudio
