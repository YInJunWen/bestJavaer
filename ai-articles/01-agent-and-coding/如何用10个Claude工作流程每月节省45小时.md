# 10 个贼爽的 workflow 工作流

> 日期：2026-04-14

## 引言：时间都去哪儿了？

每个月，你有大约45个小时凭空消失了。

不是被什么大事吞噬——没有紧急危机，没有重大决策，只是那些零碎的、重复的、本可以自动完成的工作。它们像沙漏里的细沙，一粒粒漏掉，等你回过神来，一个月又过去了。

这就是我看到 Zodchiii 的数据时的第一反应。他用两周时间追踪了自己使用 Claude 的每一个瞬间：时间戳、任务类型、手动 vs AI 处理时间。结果很直接——**每周节省 11.4 小时，每月 45+ 小时**，相当于 2 个完整工作日。

而他做到的这件事本身就很有意思：**没有 API、没有复杂集成、没有终端命令**。只是用 Claude 网页版 Pro 订阅，加上 10 个精心设计的工作流程。

这就是今天要聊的——不是怎么用 AI，而是怎么系统地用 AI。

---

## 为什么零散的提示词是个坑

先说个常见的误区。很多人用 AI 是这样的：想起一件事，扔一段话过去；再想起一件事，再扔一段。偶尔有惊喜，大多数时候在后悔——输出要么太泛，要么漏了关键信息，要么格式乱七八糟。

这不是 AI 的问题，是使用方式的问题。

零散的提示词有几个致命伤：

- **不可复制**：今天写的提示词，明天遇到类似任务就得重写一遍。脑子记不住那么多细节，版本一多就乱。
- **结果不稳定**：同一类型的任务，这次输出 A，下次输出 B，质量全凭运气。
- **每次都要校准**：你得花时间解释背景、格式要求、目标读者——这些摩擦累积起来，比"不用 AI"还累。

Zodchiii 的洞察很准确：**工作流程不是提示词，是一个系统**。固定输入格式、固定输出结构、可预测的结果、同类任务的通用解决方案。

换句话说：把"每次都要重新发明轮子"变成"一次构建，反复使用"。

---

## 10 个高 ROI 工作流程

下面这 10 个工作流程，是从实际使用中提炼出来的。每个都标明了时间节省幅度，都是 Zodchiii 实测过的。

### 1. 内容研究
**从 4 小时压缩到 1.5 小时，每周节省 2 小时 30 分钟**

这个流程解决的是写内容前的"调研地狱"——翻资料、找数据、辨真伪，一不小心就耗掉半天。

**English:**
```
Here are my sources on [TOPIC]:
[PASTE ALL RAW MATERIAL]

Extract:
1. The 5 facts that matter most for my audience (builders, not consumers)
2. Anything that contradicts the common narrative
3. Specific numbers: stars, users, funding, benchmarks
4. One angle nobody else is covering

If two sources disagree, show me both sides.
Don't summarize fluff. Only signal.
```

**中文版：**
```
以下是我收集的关于 [主题] 的原始材料：
[粘贴所有原始内容]

请提取：
1. 对我的受众最重要的 5 个事实（我的受众是实践者，不是旁观者）
2. 与主流叙事相矛盾的任何信息
3. 具体数字：GitHub star 数、用户量、融资金额、性能基准
4. 一个目前没人覆盖的角度

如果两个来源互相矛盾，同时展示双方观点。
不要总结空洞的套话，只提炼有价值的信息。
```

关键点：**直接粘贴原始材料，让 AI 做提炼，而不是让它"随便聊聊"**。模糊的要求产生模糊的结果，给定结构才能得到结构。

---

### 2. 快速研究
**从 2 小时砍到 15 分钟，每周节省 1 小时 45 分钟**

当你不了解某个领域、需要快速建立认知时，这个流程比 Google 搜索加阅读十篇文章高效得多。

**English:**
```
Research [TOPIC]. Structure:
1. Executive summary (3 sentences max)
2. Key findings (top 5, ranked by impact)
3. What's missing: gaps in available info
4. Sources with URLs

If data is insufficient for any claim, say so.
Don't speculate. Don't pad.
```

**中文版：**
```
研究 [主题]，结构如下：
1. 执行摘要（最多 3 句话）
2. 关键发现（Top 5，按影响力排序）
3. 缺失信息：现有资料中的空白
4. 来源（带 URL）

如果数据不足以支撑某个结论，如实说明。
不要猜测，不要填充废话。
```

这个模板的价值在于**强制简洁**。3 句话说完核心观点，然后才是细节。如果你发现 AI 在"填充废话"，说明你问的问题本身就太空泛。

---

### 3. 内容重构
**从 2 小时压缩到 20 分钟，每周节省 1 小时 40 分钟**

写完一篇文章后，最痛苦的不是写本身，而是改写成不同格式发不同平台。这个工作流程让 AI 帮你一次性搞定 Twitter/X、LinkedIn、Newsletter 等多个渠道。

**English:**
```
Here's my article: [PASTE OR UPLOAD]

Create:
1. A 2-sentence hook for X (include a specific number or claim from the article)
2. A 4-paragraph TG post with the key insight
3. A provocative quote-tweet caption (1 sentence)
4. 3 standalone insights that work as separate tweets throughout the week

Each piece must work independently.
Someone who never read the article should still get value.
```

**中文版：**
```
以下是我的文章：[粘贴或上传]

请创建：
1. 一条 X 推文的两句话钩子（必须包含文章中的一个具体数字或观点）
2. 一篇 4 段 Telegram 帖子，突出核心洞察
3. 一条引发讨论的引用推文配文（1 句话）
4. 3 个独立的洞察，可作为本周不同时段分别发布的推文

每个内容都必须独立可读。
哪怕对方没读过原文，也要能从这条内容里获得价值。
```

**一鱼多吃**。好的内容值得多平台分发，但手动改写是时间黑洞。这个流程保证每个版本都是独立可读的，不是简单的"复制粘贴换格式"。

这个流程还能**反向操作**——把零散的帖子整合成长文章：

**English:**
```
Here are 5-6 short TG posts from this week: [PASTE ALL]

Find the connecting thread and draft a long-form article outline.
```

**中文版：**
```
以下是本周的 5-6 条短帖：[粘贴所有内容]

找出它们之间的连接线，起草一篇长文的提纲。
```

---

### 4. 数据洞察
**从 2 小时砍到 20 分钟，每周节省 1 小时 40 分钟**

面对一份数据集，大部分人要么不知道从何下手，要么看完只记得"数字挺多"。这个流程让 AI 先做初步分析，你再决定深挖哪里。

**English:**
```
Analyze this data. I need:
1. Top 3 trends over time
2. Anything unusual or unexpected
3. Correlations between [COLUMN A] and [COLUMN B]

Table first, then a 2-paragraph summary explaining
what this means in plain English.
If the dataset is too small for a conclusion, say so.
```

**中文版：**
```
分析这份数据，我需要：
1. 时间维度的 Top 3 趋势
2. 任何异常或出乎意料的发现
3. [A 列] 和 [B 列] 之间的相关性

先出表格，再出两段总结，用通俗语言解释这意味着什么。
如果数据集太小无法得出结论，如实说明。
```

**先出表格，再给解读**——这个顺序很重要。表格是原始信息，解读是认知结论。混在一起写很容易变成"看似有道理但不可验证"的废话。

---

### 5. 代码审查
**从 2 小时压缩到 15 分钟，每周节省 1 小时 45 分钟**

代码审查是个技术活，但大部分审查时间其实花在"找常规问题"上——安全漏洞、代码风格、性能隐患。AI 擅长干这个，而且不会累。

**English:**
```
Review this code for:
- Security issues (exposed keys, injection, XSS)
- Logic errors and edge cases I might have missed
- Performance problems
- Anything that would make a senior dev uncomfortable

For each issue: severity (Critical/High/Medium/Low),
exact location, why it matters, and the corrected code.

Be harsh. "Looks good overall" is not helpful.

[PASTE CODE]
```

**中文版：**
```
审查这段代码，重点关注：
- 安全问题（泄露的密钥、注入漏洞、XSS 等）
- 逻辑错误和我可能遗漏的边界情况
- 性能问题
- 任何让高级工程师看了不舒服的地方

每个问题请注明：严重程度（Critical/High/Medium/Low）、
具体位置、为什么重要，以及修正后的代码。

要严厉。"整体看起来还行"这种反馈没有帮助。

[粘贴代码]
```

这里有个心态要调整：代码审查不是"证明我很仔细"，而是"发现真正的问题"。AI 帮你过滤掉 80% 的常规问题，你就可以专注在架构层面的判断。

---

### 6. GitHub 仓库分析
**从 1.5 小时压缩到 20 分钟，每周节省 1 小时 10 分钟**

为了写一篇关于 AI 工具的文章，需要从 1000 多个仓库中筛选出真正值得提及的项目。手动检查需要 50+ 小时，AI 帮你大幅缩减这个过程。

**English:**
```
Here's a list of GitHub repos with descriptions:
[PASTE BATCH]

For each repo, evaluate:
1. What it actually does (1 sentence, no marketing speak)
2. Traction signals: stars, recent commit activity, contributor count
3. Category: agent framework / dev tool / MCP / infrastructure / other
4. Worth featuring? Yes/No with one reason

Skip anything that's just a wrapper, a tutorial repo,
or has no commits in 30+ days.
Sort the "Yes" picks by most interesting first.
```

**中文版：**
```
以下是 GitHub 仓库列表及描述：[粘贴批次]

对每个仓库评估：
1. 它实际做什么（1 句话，不许用营销语言）
2. 增长信号：star 数、最近提交活跃度、贡献者数量
3. 分类：Agent 框架 / 开发工具 / MCP / 基础设施 / 其他
4. 值得推荐吗？是/否，附一个理由

跳过那些只是包装层、教程仓库、或 30 天以上无提交的项目。
把"值得推荐"的选项按价值从高到低排序。
```

分批处理 50-100 个仓库，自动过滤废弃项目（30 天无提交）。这个流程把需要手动检查的数量从 1000 个减少到 80 个。

---

### 7. 竞争对手分析
**从 1 小时压缩到 15 分钟，每周节省 45 分钟**

分析竞争对手的内容、定位和受众通常需要 1 小时，AI 帮你快速建立结构化认知。

**English:**
```
I'm analyzing [COMPETITOR/ACCOUNT].

Based on what you know + the data I'm providing:
1. Top 3 things they're doing well (be specific)
2. Gaps or weaknesses in their approach
3. What I can learn from them
4. How my positioning is different

About me: I write about AI tools, vibe coding, and crypto
for builders. TG + X.

Don't say "they have a strong brand."
Tell me WHY and what specifically makes it work.
```

**中文版：**
```
我在分析 [竞争对手/账号]。

基于我所知道的信息 + 我提供的资料：
1. 他们做得最好的 3 件事（要具体）
2. 他们方法中的漏洞或薄弱环节
3. 我可以从他们身上学到什么
4. 我的定位有何不同

关于我：我的受众是 AI 工具实践者、Vibe Coder、
Crypto 建设者。发布渠道是 Telegram + X。

不要说"他们有很强大的品牌"这种废话。
告诉我是**为什么**，是什么具体因素让他们的做法有效。
```

必须提供"我是谁"的上下文。没有这个，你会得到一份通用的 SWOT 分析，而不是有针对性的洞察。

---

### 8. 晨间简报
**从 45 分钟压缩到 5 分钟，每周节省 40 分钟**

每天早上刷 X"保持更新"需要 45 分钟，还会被各种噪声干扰。AI 帮你过滤掉 95% 的噪声。

**English:**
```
3-minute briefing:
1. Top 3 AI news from last 24 hours (one sentence each)
2. Crypto: major moves, liquidations, new narratives
3. Anything I should know before posting content today

Be specific: names, numbers, links.
Skip anything that isn't genuinely important.
3 real updates > 10 filler items.
```

**中文版：**
```
给我一个 3 分钟简报：
1. 过去 24 小时 Top 3 AI 新闻（每条一句话）
2. Crypto 动态：主要行情、清算情况、新兴叙事
3. 今天发内容前需要知道的事

要具体：人名、数字、链接。只选真正重要的信息。
3 条真实更新 > 10 条凑数的废话。
```

为什么比直接刷社交媒体更好：消除了噪声和分心，只提供真正重要的信息，用结构化的方式呈现，每天节省 40 分钟。

---

### 9. 邮件起草
**从 30 分钟压缩到 5 分钟，每周节省 25 分钟**

写 4 句话的邮件需要 15 分钟思考语气——太正式像机器人，太随意不专业。AI 帮你处理措辞。

**English:**
```
Draft an email.
To: [NAME + how I know them]
Goal: [WHAT I WANT THEM TO DO]
Tone: professional but sounds like a real person
Max: 5 sentences
Context: [THE SITUATION]

Does not sound like: a cold pitch template, corporate speak,
or something ChatGPT would write.
No "I hope this email finds you well."
```

**中文版：**
```
起草一封邮件。
收件人：[姓名 + 我是怎么认识他们的]
目标：[我希望他们做什么]
语气：专业但听起来像真人
长度：最多 5 句话
背景：[具体情况]

不要写成：冷冰冰的模板套话、企业黑话、
或 ChatGPT 会写的那种。
不要用"希望此邮件您一切安好"这种开头。
```

成功的关键是**说清楚你要什么，而不是让 AI 猜**。"写封邮件"是模糊指令，"写封措辞坚定但不失礼貌、要求对方在周五前确认方案的邮件"才是可执行的指令。

---

### 10. 每周回顾
**从 1 小时压缩到 30 分钟，每周节省 30 分钟**

整理一周的笔记和想法需要 1 小时，还容易遗漏重要模式。AI 帮你从上帝视角看自己。

**English:**
```
Here are my notes and ideas from this week: [PASTE EVERYTHING]

Help me:
1. Find patterns: what topics am I gravitating toward?
2. Which 3 ideas have the most content potential?
3. What am I ignoring that I shouldn't be?
4. Content plan for next week: 3 TG posts + 1 article topic

Be honest. If an idea is weak, say so.
Don't tell me everything is great.
```

**中文版：**
```
以下是我本周的笔记和想法：[粘贴所有内容]

帮我：
1. 找到模式：我正在被哪些话题吸引？
2. 哪 3 个想法的内容潜力最大？
3. 我在忽略什么不该忽略的东西？
4. 下周内容计划：3 篇 Telegram 帖子 + 1 个文章选题

要诚实。如果某个想法很弱，直接说。
不要告诉我一切都很棒。
```

AI 作为思考伙伴的价值：识别你可能忽略的模式和连接，帮助你看到更大的画面，提供诚实的反馈而不是无意义的赞美，提前规划下周的内容，避免"写什么"的焦虑。

---

## 时间节省数据：每分钟都花在刀刃上

| 工作流程 | 手动所需时间 | AI 处理时间 | 每周节省 |
|---------|------------|------------|---------|
| 内容研究 | 4 小时 | 1.5 小时 | 2 小时 30 分钟 |
| 快速研究 | 2 小时 | 15 分钟 | 1 小时 45 分钟 |
| 数据洞察 | 2 小时 | 20 分钟 | 1 小时 40 分钟 |
| 代码审查 | 2 小时 | 15 分钟 | 1 小时 45 分钟 |
| GitHub 仓库分析 | 1.5 小时 | 20 分钟 | 1 小时 10 分钟 |
| 内容重构 | 2 小时 | 20 分钟 | 1 小时 40 分钟 |
| 竞争对手分析 | 1 小时 | 15 分钟 | 45 分钟 |
| 晨间简报 | 45 分钟 | 5 分钟 | 40 分钟 |
| 邮件起草 | 30 分钟 | 5 分钟 | 25 分钟 |
| 每周回顾 | 1 小时 | 30 分钟 | 30 分钟 |
| **总计** | **~16 小时** | **~3 小时** | **约 13 小时/月** |

> 注：以上数字为 Zodchiii 个人实测数据。你的结果取决于工作性质和当前流程的摩擦程度。但即使只采用 3-4 个工作流程，每月节省 5+ 小时也很现实。

---

## 深刻的洞察：时间的真相

从这次追踪中，Zodchiii 得出了一个比节省时间更深刻的结论：**大多数"工作"其实是摩擦。**

我们的时间都花在哪里了？

- **上下文切换**：打开标签页、重新阅读、重新定位——每次切换消耗 15-20 分钟
- **重复性整理**：同样的信息整理工作一遍又一遍——每次都是"重新发明轮子"
- **噪声过滤**：从大量无关信息中找出有价值的那一条——大海捞针
- **格式化劳动**：把同一份内容改写成不同格式——技术含量低但极其耗时

**AI 改变了什么？**

AI 并没有替代人类的工作，它替代了围绕工作的摩擦。真正需要人类的部分——思考、决策、编辑——仍然保留，但这些部分的效率大幅提升。

**核心纠正**：很多人说"AI 不能做我的工作"，这通常是对的。但 AI 可以做围绕你工作的 60% 的摩擦性任务，而这 60% 正是时间浪费最严重的地方。

---

## 建立工作流程的实践指南

### 步骤 1：识别高摩擦任务

查看你的工作日，问自己：

- 哪些任务我每周至少做 2 次？
- 哪些任务需要重复相同的步骤？
- 哪些任务让我感到无聊或烦躁？

这些都是建立工作流程的理想候选。

### 步骤 2：分析当前流程

记录你现在完成该任务的步骤：

- 需要什么输入？
- 需要什么输出？
- 有哪些可预测的部分？
- 哪里最容易出错？

### 步骤 3：创建结构化提示词

基于分析结果，创建一个包含以下元素的提示词：

- 明确的任务描述
- 输入格式要求
- 输出结构模板
- 质量控制标准（如"要严厉""不要猜测"）
- 成功的关键要素

### 步骤 4：测试和优化

使用这个提示词完成几次任务，然后问自己：

- 结果是否符合预期？
- 还需要什么调整？
- 如何让它更精确？

### 步骤 5：记录和标准化

一旦工作流程稳定下来，记录：

- 任务类型
- 输入要求
- 提示词模板
- 预期输出格式
- 常见问题和解决方案

---

## 常见误区和避免方法

**误区 1：追求完美** ：工作流程不需要完美，只需要比手动更快。追求完美会让你永远无法开始。

**误区 2：一次建立太多** ：从 1-2 个高价值工作流程开始，验证有效后再扩展。贪多嚼不烂。

**误区 3：忽视质量控制** ：在提示词中加入质量要求（如"要严厉""只提炼信号"），定期验证结果，不要让 AI 输出成为新的噪声来源。

**误区 4：建立后不再更新** ：定期回顾工作流程，根据使用反馈持续迭代。一次性工作流程不如持续优化的系统。

**误区 5：忽视人机边界** ：AI 擅长执行和整理，不擅长判断和决策。工作流程应该把"需要判断的事"留给人类，把"重复性执行"交给 AI。

---

## 未来趋势：接下来会发生什么

**短期（6-12 个月）**

1. **工作流程智能化**： AI 将更好地理解个人工作风格，主动推荐优化空间，而不只是被动响应
2. **跨平台连接**： 工作流程将能与更多工具（Notion、Linear、Slack）直接集成，减少手动复制粘贴
3. **结构化提示词市场**： 会出现经过验证的高质量提示词模板市场，降低"从零构建"的门槛

**中期（1-2 年）**

1. **个性化工作流程引擎**： 基于你的历史行为数据，自动生成和优化工作流程，人类从"构建者"变成"审核者"
2. **多 Agent 协作**： 复杂任务将由多个 AI Agent 分工完成，每个 Agent 专注一个工作流程，形成流水线

**更重要的趋势**

AI 工作流程的普及将重新定义"专业能力"的含义：

- **会提问比会执行更重要**： 未来最稀缺的能力是清晰定义问题，而不是熟练操作工具
- **判断力成为核心竞争力**： AI 能做执行，但判断"做什么"、判断"结果对不对"仍然需要人类
- **系统化思维成为必备技能**： 能把重复性任务抽象为流程的人，和只会一个个完成任务的人，效率差距会越拉越大

---

## 写在最后

45 小时听起来很多，分摊到每个月其实也就是每天多出 1.5 小时。

但这 1.5 小时不是从天上掉下来的——它是把那些本该花在"操作"上的精力，转移到"思考"上的结果。

AI 不会替你做决定，但可以替你做执行。把时间从执行层抽出来，放到真正需要判断力的地方，这才是使用 AI 的正确姿势。

剩下的，就是选一个工作流程，今天就开始。

---

> 注：本文基于 Zodchiii 在 Twitter/X 上的分享。更多日常笔记和 AI 实践心得，可以在她的 Telegram 频道（t.me/zodchixquant）中找到。
