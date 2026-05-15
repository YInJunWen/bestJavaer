# Vercel 被黑了，这次是从一个 AI 工具突破的

> 日期：2026-04-21

## 事件

4 月 19 日，Vercel 确认了一次安全事件，Vercel 的声明报告 https://vercel.com/kb/bulletin/vercel-april-2026-security-incident。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260420122614051.png" alt="image-20260420122614051" style="zoom:50%;" />

黑客声称已经拿到了 Vercel 的内部数据，正在以 200 万美元的价格出售。Vercel 随后发布公告，承认系统被入侵，影响范围波及部分客户。

OpenAI、Cursor、Pinterest、Bose 这些公司都在 Vercel 的客户名单里。如果数据真的泄露，影响不会小。

---

## 怎么被突破的

Vercel CEO Guillermo Rauch 在 X 上披露了细节：

攻击的起点是一个**第三方 AI 工具**。

一名 Vercel 员工使用了某个第三方 AI 工具，这个工具的 Google Workspace OAuth 应用被入侵了。攻击者通过这个 OAuth 应用的访问权限，进入了该员工的 Google 账号，进而渗透到 Vercel 的内部系统。

整个链路是：

**第三方 AI 工具 → 被污染的 OAuth 应用 → 员工 Google 账号 → Vercel 内部系统**

这不是 Vercel 自身代码的漏洞。是第三方工具的安全问题，把 Vercel 一起拖下水了。

![image-20260420133158746](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260420133158746.png)

Vercel 随后发布了 CVE-2026-23869 来记录这次事件，并建议所有客户检查自己的环境变量、开启敏感环境变量保护、必要时轮换密钥。

---

## 加密开发者最先慌了

这件事出来之后，最先动作的是加密领域的开发者。

因为 Vercel 托管着大量的 Web3 应用，这些应用通常依赖 API 密钥来访问区块链节点和钱包服务。如果这些密钥被泄露，攻击者可以直接操纵用户的资金。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260420131304599.png" alt="image-20260420131304599" style="zoom:50%;" />

所以消息一出来，很多加密项目方紧急开始轮换 API 密钥。这件事的影响不只是在技术圈，它直接关系到资产安全。

Vercel 在通报中强调了一个技术细节：

>“攻击者得以访问部分 Vercel 环境以及**未被标记为‘敏感’**的环境变量。在 Vercel 中，标记为‘敏感’的环境变量会以一种无法被读取的方式存储，目前我们没有证据表明这些敏感值遭到了访问。”

然而，许多开发者出于便利，并未开启“敏感环境变量保护”功能。这意味着，存储为明文的环境变量（如 API 密钥、数据库密码、签名私钥）在此次事件中属于高危暴露面。

---

## 一个值得想的点

Vercel 是做基础设施的公司。Next.js 是他们开发的，Edge Functions、Serverless、CI/CD 这些服务在开发者圈子里是标杆级的产品。

但再强的基础设施，也扛不住员工电脑的第三方 AI 工具。

这不是 Vercel 的问题。这是整个行业的问题。

现在有多少开发者在用各种 AI 工具？写作助手、代码补全、翻译、摘要生成。这些工具越来越多地拿到了你的 OAuth 权限——可以访问你的 Google 账号、GitHub 账号、工作台邮件。

这些权限的渗透深度，可能连你自己都不清楚。

Vercel 这次出事，不是被黑客正面进攻打穿的，是被侧翼的一个第三方工具带进来的。这个攻击路径，在安全圈有个名字叫**供应链攻击**。

**你安全，但你用的工具不安全，你就也不安全。**

大家记住这句话。

---

## ShinyHunters 是谁

这次声称对事件负责的是 ShinyHunters。

![黑客论坛发帖](https://cdn.jsdelivr.net/gh/doggaifan/picbed/vercel-hacking-forum-post.jpg)

这个黑客组织最近很活跃，前脚刚攻击了 GTA 的开发商 Rockstar Games，现在又打穿了 Vercel。他们的风格是入侵大公司、窃取数据、公开出售。

200 万美元的叫价，不是在吓唬人。如果 Vercel 的数据里真的包含大客户的源码或者密钥，这个价格不算离谱。

Vercel 目前的说法是：**没有敏感数据被访问**，服务本身没有受影响，只影响了部分客户。

但这个说法需要打一个问号。

数据泄露这种事，从发现到确认影响范围通常需要几周时间。Vercel 现在说没有，不等于真的没有。

对于 Vercel 的客户来说，现在该做的不是等官方结论，是先检查自己的环境变量、轮换密钥、确认没有异常访问。

Vercel 是一家靠安全口碑吃饭的公司。这次事件对品牌信任的损伤，不是赔多少钱能算清楚的。

但更深的问题是：这不会是最后一家因为「第三方 AI 工具」被突破的公司。

当 AI 工具越来越多地渗透到开发者的日常工作，它们的安全风险也在同步放大。这些工具拿到的 OAuth 权限，本质上和你自己代码库的安全是同一个级别。

你信你的 AI 助手吗？你知道你用的那个 AI 工具背后的 OAuth 配置是什么吗？

大多数开发者不知道。

这件事给整个行业提了一个醒：**AI 工具的安全，不只是 AI 公司的事。用这些工具的企业，也得把 AI 工具纳入自己的安全审计范围。**

不然，下一个被第三方拖下水的，不会只有 Vercel。

---

Sources:
- [Vercel confirms breach as hackers claim to be selling stolen data - BleepingComputer](https://www.bleepingcomputer.com/news/security/vercel-confirms-breach-as-hackers-claim-to-be-selling-stolen-data/)
- [Vercel hacked, hacker using ShinyHunters name to sell data for $2 million - India Today](https://www.indiatoday.in/amp/technology/news/story/vercel-hacked-hacker-using-shinyhunters-name-to-sell-data-for-2-million-2898719-2026-04-20)
- [Hack at Vercel sends crypto developers scrambling to lock down API keys - CoinDesk](https://www.coindesk.com/tech/2026/04/20/hack-at-vercel-sends-crypto-developers-scrambling-to-lock-down-api-keys)
- [Vercel April 2026 security incident - Hacker News](https://news.ycombinator.com/item?id=47824463)
- [Summary of CVE-2026-23869 - Vercel](https://vercel.com/changelog/summary-of-cve-2026-23869)
