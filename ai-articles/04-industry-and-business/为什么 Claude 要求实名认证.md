# 为什么 Claude 要求实名认证？

> 日期：2026-04-16

## Anthropic 的 KYC 不是合规动作，是战略布局

Anthropic 开始要求用户做身份验证了。官方说法是“确认用户身份及合规用途”，听起来很标准、很安全。但我跟你说，别被这个官方措辞糊弄了——**这根本不是简单的合规动作，而是一场用户分层战争的开始**。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/2f96aff8d509dcdd45daf2f3c8ae598a.png" alt="2f96aff8d509dcdd45daf2f3c8ae598a" style="zoom:50%;" />

想想看，Anthropic 强调“身份验证数据不会用于模型训练，不会共享给第三方”。这句话反过来什么意思？他们在说：你的数据我不要，我只拿你的身份。换句话说，**他们要的不是你的数据，是你这个人**。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/4aaf93c97931fa7c66b6b48e9c37b0c2.png" alt="4aaf93c97931fa7c66b6b48e9c37b0c2" style="zoom:50%;" />

为什么要人？因为 AI 的能力正在突破边界。Claude Mythos 能主动发现并利用系统漏洞，这件事已经把金融监管机构（美、英、加）惊动了。高盛用 Claude 处理交易核算和 KYC 流程，汇丰给 3.1 万工程师配上 AI 编码助手——**AI 不再只是聊天玩具，它正在成为金融基础设施的一部分**。

基础设施需要实名制。这个逻辑很简单：ATM 机需要实名吗？不需要，因为它的能力有限。但如果 AI 能自主执行端到端支付（桑坦德银行刚干成这事），能处理交易核算和客户准入，那它的用户就必须被追踪和问责。

所以 Anthropic 的 KYC，本质上是在为**能力升级后的责任归属**做准备。

## 身份认证的三大真实目的

### 第一：合规挡箭牌

地区限制不是新问题。中国大陆用户订阅 Claude Pro 被封号这事，早在 KYC 推行之前就有了——“不受支持地区”政策一出，VPN + 虚拟卡这套玩法就该淘汰了。现在搞 KYC，只不过是把这件事做得更正式、更难绕过。

换句话说：**封号不是 KYC 的结果，而是 KYC 的原因**。他们需要一套机制来执行地区限制，而不仅仅是声明“不支持某地区”。

### 第二：风险隔离

Claude Mythos 这类模型的能力已经触发了金融监管的敏感神经。监管机构关注的不是 Anthropic 会不会作恶，而是**这些能力一旦流入市场，会不会产生不可控的涟漪**。

KYC 相当于一道过滤网：验证通过的用户，意味着“我知道你是谁，你跑不掉”。这比单纯限制模型能力更容易让监管机构接受。

### 第三：能力分级的前置条件

这才是重点。OpenAI 推出 GPT-5.4-Cyber 时搞了个 TAC（Trusted Access for Cyber）机制，核心逻辑是：**不同验证等级对应不同模型能力**。高阶用户才能访问“较宽松限制”的版本。

Anthropic 的 KYC 很可能在走同一条路。今天让你验证身份，明天就能根据验证等级开放不同能力。**身份认证不是终点，能力分级才是终点**。

## Anthropic 和 OpenAI 的两种路线分歧

这两家公司最近在 AI 资安领域的策略差异非常有意思。

Anthropic 的做法是**“限制能力”**——把 Claude Mythos 严格限制在 Glasswing 计划内的约 40 个组织（Amazon、Apple、Microsoft 这些），近乎封闭测试。

OpenAI 的思路是**“管理用户”**——通过 KYC 验证来决定谁能访问更进阶的能力，但整体上更倾向于扩大访问范围。OpenAI 甚至在官方文章里明说：他们不认为“由少数人集中决定谁有资格保护自己是实际可行或适当的”。

这个分歧背后是两种安全哲学的对抗：

- Anthropic：能力太危险 → 限制扩散 → 只给可信实体
- OpenAI：能力无法消除 → 管理使用 → 让更多防御者获得工具

有意思的是，**Anthropic 的做法正在被批评为“科技巨头对关键基础设施防御资源的垄断”**。你品品——限制模型能力听起来很安全，但如果只有大公司能接触这些能力，那小安全公司、独立研究者怎么办？

这场争论没有标准答案，但有一个趋势是确定的：**AI 资安正在从“模型能力竞赛”转向“治理框架竞争”**。

## 反驳一个可能的质疑

有人会说：KYC 就是收集用户数据，最终会变成数据垄断。

这个担忧有一定道理，但**混淆了重点**。Anthropic 明确说了，身份数据由 Persona 托管，不用于训练，不搞营销共享。这不是承诺，是技术架构层面的隔离。

真正的风险不在于 Anthropic 收集了什么，而在于**身份认证会成为行业标准**。当所有 AI 提供商都开始要求实名，那问题就不是某家公司可不可信，而是整个行业会形成怎样的准入门槛。

## 写在最后

Anthropic 的 KYC 不是终点，而是起点。

**AI 正在从“开放实验场”变成“受监管基础设施”**，这个趋势不会因为哪个公司标榜“负责任”而改变。监管机构的关注、模型能力的突破、用户规模的扩大——这三股力量会共同推动 AI 实名制的到来。

对于我们这些开发者来说，**最重要的准备不是技术层面的，而是心理层面的**：接受一个不再匿名的 AI 生态，并学会在新的规则下找到自己的位置。

当然，如果你现在还在用虚拟卡绑 Claude Pro，我建议你：**别折腾了，该来的总会来**。

---

**你支持 AI 实名制吗？欢迎在评论区留下你的观点。**



## 参考来源

1. [Anthropic要开始推行身份认证了- 前沿快讯 - LINUX DO](https://linux.do/t/topic/1971480) - Search Engine
2. [Claude 这是要赶尽杀绝啊，杜绝一切非法使用啊- 前沿快讯 - LINUX DO](https://linux.do/t/topic/1971469) - Search Engine
3. [Anthropic新模型Claude Mythos引發金融業資安疑慮，美英加監理機關 ...](https://www.ithome.com.tw/news/175081) - Search Engine
4. [11.3 数据隐私与合规：Data Privacy 与Compliance | Claude 技术指南](https://yeasy.gitbook.io/claude_guide/di-si-bu-fen-shi-zhan-pian/11_safety/11.3_privacy) - Search Engine
5. [AI 資安攻防升級：OpenAI 推GPT-5.4-Cyber，釋出策略與 ...](https://today.line.me/tw/v3/article/eL51vPg?view=topic&referral=AI) - Search Engine
6. [Claude Pro 订阅封号：原因、规避与申诉全解析 - 吐槽- Angryecho](https://www.angryecho.com/u/V2Xsuperherowhd/p/P2TB4qRzkYLjGd98tHPVV) - Search Engine
7. [国外金融业AI 应用动态简报3.16-华道数据股份有限公司](https://www.chinadatagroup.com/info/6316.html) - Search Engine
