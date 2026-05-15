# Claude Code 100 万上下文：到底是真需求还是营销废话？

> 日期：2026-04-16

## 背景

Anthropic 官方博客发了一篇技术文章，讲 Claude Code 是怎么做会话管理的，以及 100 万 Token 上下文在实际开发里到底怎么用。

老实说，这个话题在开发者社区引起了不小的讨论。有人觉得这是工程上的突破，也有人在问：一般人真的用得上吗？

---

## 官方怎么说

Anthropic 那篇博客的核心内容是关于会话管理的架构设计。三个阶段：

**会话初始化** → 读取项目文件、扫描目录、构建初始上下文

**对话循环** → 用户输入 → Claude 推理 → 工具调用 → 结果写回上下文，每个循环都会重新计算可用空间

**会话持久化** → 写入 `.claude` 目录，支持跨会话恢复

这套系统的本质是**上下文生命周期管理**，不是简单地把对话历史塞进去。它会动态决定什么内容该保留、什么该压缩、什么该丢弃。

---

## 网上争什么

Reddit 上有个帖子标题很直接：**「100 万上下文对 99% 的用户来说就是营销废话」**。

这个观点有它的道理。核心批评是：

大多数人的日常开发场景，单个文件几百行、一个项目几十个文件，塞进 100 万上下文就像用油轮去送一份外卖——不是不能，是没必要。

HN 上的讨论更理性一些。有人提到，100 万上下文真正的价值场景是：

- 一次性消化整个代码库（尤其是接手老项目）
- 大型重构任务，Claude 需要同时理解几十个文件的关系
- 长线任务，比如让 AI 记住你项目里所有历史决策

但同时有人指出实际问题：上下文越长，每次 API 调用的延迟越高，成本也越高。用 100 万上下文跑一个简单任务，等于用牛刀杀鸡。

---

## 一个 trade-off 判断

不是所有人都在处理需要 100 万上下文的场景。但有些开发者确实是。

比如你接手一个三年前的老项目，代码没有文档、架构没人说得清。这时候 100 万上下文能让你一次性把整个代码库喂给 Claude，让它自己理解模块之间的关系。这比起逐个文件复制粘贴，效率是质的提升。

再比如你要做跨文件重构，改一个函数会影响二十个文件。传统方式是你告诉 Claude 每个文件改什么，Claude 可能忘掉；100 万上下文让 Claude 始终看到全局，改动的连贯性完全不一样。

**但**如果你日常就是改改小功能、写写脚本，100 万上下文对你来说确实感知不强。

这不是技术不行，是使用场景不匹配。

---

## 社区的另一个关注点：成本

有个 YouTube 视频的标题很实在：**「不要用 Claude 的 100 万上下文，除非你看过这个」**

成本是真实的问题。上下文越长，每次请求的 Token 消耗越大。在公测期可能感知不强，但正式收费后，长期跑大上下文session的成本会成为选择门槛。

这也是为什么很多人更关心的是：**什么时候该用它，什么时候不该用它。**

---

## 官方文档透露的细节

Claude Code 的模型配置文档里提到，100 万上下文是通过 Claude 3.5 Sonnet 实现的，支持在代码.claude.com 项目里直接开启。

实际操作上，用户可以在项目里配置是否启用 100 万上下文模式。启用后，Claude 会自动管理上下文窗口，包括对长输出做摘要、动态调整对话历史的占比、优先保留代码文件而非对话记录。

这意味着官方也承认了上下文空间是稀缺资源，需要精细管理。

---

## 一个判断

100 万上下文不是噱头，但也不是人人需要。

它的目标用户是：**需要 AI 同时理解大量信息的场景**。接手大型代码库、复杂重构、跨模块分析，这类任务它能显著降低沟通成本。

但如果你的日常工作是在单个文件里写函数、调 API，100 万上下文对你的意义更多是心理安慰而不是实际效率提升。

营销层面，Anthropic 宣传 100 万确实吸引眼球。真实价值上，它是有选择性地释放给有相应需求的开发者。

**不是所有人都需要一头大象，但你不能否认大象在需要的时候比蚂蚁管用。**

---

Sources:
- [Using Claude Code Session Management and 1M Context](https://claude.com/blog/using-claude-code-session-management-and-1m-context)
- [1M context window is basically marketing BS for 99% of users - Reddit](https://www.reddit.com/r/ClaudeCode/comments/1qxaddx/1m_context_window_is_basically_marketing_bs_for/)
- [HN Discussion on Claude Code 1M Context](https://news.ycombinator.com/item?id=46902427)
- [Claude Code 1M Context Window: Cost, Limits, and When to Use](https://www.claudecodecamp.com/p/claude-code-1m-context-window)
- [Claude 1M Token Context Window: What It Means for AI Agents](https://www.mindstudio.ai/blog/claude-1m-token-context-window-ai-agents/)
