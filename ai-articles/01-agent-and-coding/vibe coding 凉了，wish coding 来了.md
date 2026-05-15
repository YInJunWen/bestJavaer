# vibe coding 凉了，wish coding 来了

> 日期：2026-04-21

2025 年，vibe coding 是最火的词。

Karpathy 提出来之后，全行业都在讨论：你描述需求，AI 写代码，方向你来判断，结果来验收。这个模式听起来很美好，也确实解决了很多问题。

但到了 2026 年，问题开始暴露了。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260421152607242.png" alt="image-20260421152607242" style="zoom:50%;" />

vibe coding 的本质是什么？你给 AI 一个大致需求，AI 自己猜你要什么。你说「做一个感觉像高端产品的登录页面」，AI 给你一堆深色渐变加玻璃态。你说「帮我搭一个后端」，AI 给你一个 REST API架子，你也不知道为什么选这个结构。

这种凭借直观感受的方式，快的时候是真快。但出来的代码质量取决于 AI 猜得对不对。猜对了是惊喜，猜错了是惊吓。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260421152641388.png" alt="image-20260421152641388" style="zoom:50%;" />

vibe coding 暴露的根本问题是：它还是在让你**大概描述**而不是**精确表达**。你给 AI 的信息是模糊的，AI 的输出自然也是模糊的。

---

## 新的词出来了：intent is the new syntax

McKinsey 有一句话我觉得说得很准：

> Over the next decade, AI-assisted programming will let developers specify intent through a mix of formal programming languages and natural language

翻译过来就是：未来十年，AI 辅助编程会让开发者通过正式编程语言和自然语言的混合来表达意图。

注意这里的核心词是 **intent**——意图。

Everest Group 的说法更直接：Intent is the new syntax 意图就是新的语法。

这两句话合在一起，说的是同一件事：编程的最小单元，正在从**语法**变成**意图**。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260421152722254.png" alt="image-20260421152722254" style="zoom:50%;" />

以前你学编程，是学怎么用正确的语法表达一个指令。现在你学编程，是学怎么用精确的语言描述你要什么。

这不是工具变了，是人会编程这件事的定义变了。

---

## wish coding 的正经名字

有人在给这个现象起了个正经名字：intent-oriented programming，意图导向编程。

它的核心主张是：编程语言应该无限接近人的思考方式，而不是机器的执行方式。你想的是「用户登录之后看到首页」，而不是「调用 /auth/login 接口然后重定向到 /home」。

这个转变的滑梯很长：

- 最早期：写机器码
- 然后：写汇编
- 然后：写高级语言
- 现在：用自然语言描述意图，AI 转换成代码

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260421152820950.png" alt="image-20260421152820950" style="zoom:50%;" />

每一步都是在用更接近人思考方式的方式编程。

所以 `wish coding` 这个名字虽然听起来有点调侃，但它描述的现象是真实的：你不是写代码，你是在许愿。你说「我要一个能用的登录系统」，AI 给你一个。

许愿的问题是：你得把你的愿望说清楚。

vibe coding 阶段，你大概说说就行，AI 会猜。intent-oriented 阶段，你说模糊了，AI 就给你模糊的结果。

区别在于：vibe coding 是感受驱动，intent-oriented 是规格驱动。

---

## 为什么 vibe coding 先火，intent-oriented 后到

这跟 AI 能力的进化阶段有关。

vibe coding 能跑起来，是因为 2024-2025 年的 AI 已经能理解**大概要什么**，并且能生成看起来像样的代码。这个阶段 AI 的能力是我猜你大概要这个，对模糊指令有一定的容忍度。

但问题是大概像样不等于能用。出来的代码往往有这些问题：边界情况没处理、安全隐患存在、架构不适合长期维护。测试能跑，一上线就出问题。

intent-oriented 需要的是：AI 能精确理解你说的每一句话，并把它转化成严格对应的代码实现。你说处理并发的时候要考虑 token 过期，AI 不会漏掉这个细节。你说错误信息要包含原始请求 ID，AI 不会随便写一句请求失败。

这种精确度，需要更强的 AI 能力，也需要更规范的表达方式。

所以 vibe coding 是 AI 早期的产物，intent-oriented 是 AI 成熟一点的产物。这不是谁取代谁，是 AI 能力上了一个台阶，许愿的方式也需要升级。

---

## Augment Code 做的事

有个产品叫 Augment Code https://www.augmentcode.com/product/intent，它的定位能说明 intent-oriented programming 现在在发生什么。

![image-20260420213142856](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260420213142856.png)

它的模式是这样的：你给一个 intent spec，AI 拆解成多个 agent 并行工作。Auth Token Agent 做认证模块，Gateway Middleware Agent 做网关验证，两个 agent 共享同一个规格文档，各自实现自己的部分，最后串起来。

关键点不是多个 agent，而是你给的指令是**结构化的规格文档**，不是一句帮我加上 JWT 认证。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260421152935575.png" alt="image-20260421152935575" style="zoom:50%;" />

结构化意味着：每个字段的格式是定的，每个接口的输入输出是定的，边界情况是定的。AI 对着规格做，做完验收也对着规格验收。

这不是 vibe coding，这是 intent-oriented。

---

intent-oriented 的问题是：它要求你先想清楚你要什么。

vibe coding 的好处是探索阶段你可以先丢一个模糊的想法，看 AI 给你什么，然后慢慢调整。这个过程不需要你一开始就把所有细节想明白。

intent-oriented 要求你先写规格，再让 AI 实现。这对有经验的开发者来说可能是自然流程，对探索阶段的项目来说有时候反而是限制。

所以这两者不是非此即彼的关系：

- 探索阶段，vibe coding 快速验证想法
- 方向明确了，切换到 intent-oriented 做扎实实现

McKinsey 的说法也是这个意思。

> 未来十年，是formal 编程语言 + 自然语言的混合，而不是纯自然语言。formal 部分是规格，自然语言部分是表达意图。

---

## 

我自己在用 AI coding 工具的时候，这个转变的感受是很明显的。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260421153016806.png" alt="image-20260421153016806" style="zoom:50%;" />

一开始用 vibe coding，我丢一句帮我做一个博客系统，AI 给我一个能跑的架子。我觉得很爽。

但深入用下去，我发现**能跑**和**能用**之间差了很多东西：分类标签的实现有问题、搜索功能的边界情况没处理、SEO 的 meta 标签不规范。AI 帮我快速搭了一个架子，但每个细节都需要我重新排查。

后来我换了一个方式：先在脑子里想清楚我要的博客系统有哪些功能模块、每个模块的输入输出是什么、错误怎么处理。想到了，用结构化的方式告诉 AI。

结果完全不一样。AI 给的东西一开始就是对的，不需要我反复修。

不是 AI 变了，是我的表达方式变了。

---

## 

vibe coding 不是凉了，是升级了。

它没有被抛弃，它进化成了 intent-oriented programming。许愿的方式从大概描述感受变成了精确描述规格。

这个转变的本质不是工具变了，是人对 AI 的用法变了：你给 AI 的信息质量越高，AI 给你的结果质量也越高。

Vibe coding 是 AI 帮你快速探索，intent-oriented 是 AI 帮你精确实现。

两个都用，比非此即彼更实际。



Sources:
- [Intent is the New Syntax: Why Vibe Coding Represents a Shift - Everest Group](https://www.everestgrp.com/blog/intent-is-the-new-syntax-why-vibe-coding-represents-a-shift-in-software-development-blog.html)
- [Vibe Coding: Toward an AI‑Native Paradigm - Vinay Bamil](https://arxiv.org/html/2510.17842v1)
- [Build with Intent | Augment Code](https://www.augmentcode.com/product/intent)
- [Intent-Oriented Programming: Bridging Human Thought and AI Machine Execution](https://kotrotsos.medium.com/intent-oriented-programming-bridging-human-thought-and-ai-machine-execution-3a92373cc1b6)
- [Unlocking the value of AI in software development - McKinsey](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/unlocking-the-value-of-ai-in-software-development)
- [Vibe Coding in 2026: How AI Is Changing the Way Developers Write Code](https://daily.dev/blog/vibe-coding-2026-ai-changing-how-developers-write-code)
