# 我把 OpenAI 的接口扒了，这事儿挺像悬疑故事

> 日期：2026-05-03

就在前两天，我研究了一下 OpenAI 给我 6 个月 Pro 20x 的这个事儿，昨天还写了一篇文章复盘。




这事儿就像我上一篇文章里说的那样：

官方给你发了票，你拿着票去看戏，结果走到门口，发现进不去。

文章发出来之后，很多小伙伴给我出谋划策。

> 但是我说一句，大家都想得太简单了。

这里不会把所有方法都聊个遍。一方面没必要，另一方面也不适合写成什么操作指南。

大致说一下为什么那些路都走不通。

首先就是 Google Play 和苹果礼品卡。

这种方式我已经试过了，它只能走 web 端订阅，所以直接 pass。

这个领取入口是 OpenAI 直接发到邮箱里的。从邮件点进去，URL 里会带着 `promoCode`。这个点一开始就很迷惑，因为我以前订阅 GPT，基本没见过这种路径。

通过 URL 点进去，它会直接弹出 checkout 界面。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503085227444.png" alt="image-20260503085227444" style="zoom:50%;" />

然后你 Upgrade to Pro，它就会弹出绑定银行卡的界面。

真的，如果不是亲身经历，谁都不会相信就这一个绑卡界面会把人搞得心态崩塌。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503103433256.png" alt="image-20260503103433256" style="zoom:50%;" />

MasterCard、VISA、3DS，在这个界面上简直不堪一击。

> 大家也不要再推荐我去找代订渠道了。我基本问了一圈，很多人一看这个订单界面就说接不了。这个方向不靠谱，也不建议大家走。

![image-20260503102218027](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503102218027.png)

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503102406902.png" alt="image-20260503102406902" style="zoom:50%;" />

游戏玩到这里，基本上已经没什么可玩性了。

直接 end game，关机睡觉。

但我还是想讲点故事。

这个故事不像普通的失败复盘，更像是你在凝视深渊的时候，深渊也在凝视你的**悬疑故事**。

因为这已经不是搞一张信用卡这么简单的事儿了。

## 我扒了一下接口

作为一枚古法时期的程序员，在 AI 时代，虽然已经 AI Native 了，但是我仍然保留着古法程序员的好奇心。

就算权益领不了，我也要知道问题出在哪里。

于是我点了一下 OpenAI 的 Subscribe 按钮，把 F12 打开，开发者模式，启动！！！

OpenAI 的 checkout 走的是 Stripe，所以请求里带的字段贼多。点完 Subscribe，checkout session 接口一返回，几乎就是把"我是谁、我从哪来、我要付多少钱"全摊开了。

![image-20260503113538122](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503113538122.png)

我第一次抓到的几个关键字段是这样的：

```text
currency:                 sgd
geocoding.country_code:   SG
context.timezone:         Asia/Shanghai
context.locale:           zh-CN
is_business_ip2:          true
approval_method:          manual
submission_attempt.state: requires_approval
failure_reason:           confirm_error
```

我当时网络出口在新加坡。

但接口里的几个字段像证物一样摆在桌上：

- 系统时区是上海
- 浏览器语言是中文
- IP 还带着一个 `is_business_ip2: true` 的标记

`is_business_ip2` 这个字段没有公开文档。从字面看，"business IP" 在风控领域通常指机房 / 数据中心 IP，与"住宅 IP"相对。在我这次请求里，它返回的就是 `true`——这是接口里写的，不是我猜的。OpenAI 内部具体怎么用它，我看不到。

把这些线索拼起来，风控看到的画像大致是这样：

> 一个带着中文浏览器环境、上海时区、机房 IP 标记，却用 SG 地区价格进入 checkout 的用户。

单独看每一条，都不至于让一笔订单挂掉。

但它们同时出现在同一笔 checkout 里，**就不像巧合了**。

![84e67cd89c028419017fc14b35b25b6d](https://cdn.jsdelivr.net/gh/doggaifan/picbed/84e67cd89c028419017fc14b35b25b6d.png)

`approval_method: manual` 加上 `requires_approval`，至少说明一件事：这笔订单没自动过，进入了需要审批的状态。

`confirm_error` 这个字段单独看，不能直接证明"已经被人工驳回"。它告诉我的只有一件事——确认订阅这一步失败了。

像悬疑故事里第一具尸体出现了。凶手还不能定，但已经知道有人死了。

第一次失败，意料之中。

**这次我判断大概率是 IP 环境污染。**

## 那我把信号全对上看看

我做了件挺二的事：把地区相关信号都对齐到美东试试。IP、账单地址、支付方式、系统时区，我都尽量保持一致。

理论上这次该齐了吧？

我重新点 Subscribe，再抓一次接口：

```text
currency:                 usd                      ✓
geocoding.country_code:   US                       ✓
context.timezone:         Asia/Shanghai            ✗
context.locale:           zh-CN                    ✗
is_business_ip2:          true                     ✗
failure_reason:           confirm_error
original_error_message:   "出错了，请重试。"          ← 中文
```

**信号根本没对上。**

我以为我改了时区，但接口里还是 `Asia/Shanghai`。

后来我反应过来——浏览器里 JavaScript 拿时区用的是 `Intl.DateTimeFormat().resolvedOptions().timeZone`，这个读的是 ICU 库底层的配置，跟我在 macOS 系统设置里改的那个时区不是一回事。我只舞刀弄枪的改了改表面的东西，看不见的东西没改。

我以为 IP 信号干净了，但 `is_business_ip2` 还是 `true`。我换的所谓"美国住宅"代理，在 OpenAI 这套判断里依然没被认作普通住宅 IP。具体它是怎么识别的，我不知道，但结果就摆在那里。

`navigator.language` / `navigator.languages` 我压根没动，浏览器还是把 `zh-CN` 老老实实报上去了。

这个字段本身并不神秘——浏览器语言可以改，`Accept-Language` 也可以变。问题在于，它只是最表层那一粒豆子。当它和时区、IP 标记、账号历史、Support 工单一起出现的时候，它就从一个普通语言偏好，变成了拼豆玩具里的其中一粒豆子。

而最骚的一段在最后那行：

```text
"original_error_message": "出错了，请重试。"
```

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503112747740.png" alt="image-20260503112747740" style="zoom:50%;" />

OpenAI 最后**返了我一句中文报错**。。。。

> 这仿佛是在营造一种喜剧效果。

最直接的嫌疑是请求头里的 `Accept-Language: zh-CN`——浏览器自己加的。后端按这个 header 选语言返页面，是常见做法。

也就是说：**我以为自己把舞台布置成了美国现场，但请求头里仍然留着中文语境的脚印。**

OpenAI 内部日志怎么记，我看不到。但我能看到的是：他们返给我的，确实是一句中文错误。

这不就像是你以为你 vibe better，实际上你只是 vibe shit。

---

## 账号才是真正的案发现场

把两次接口的 JSON 仔细比对了一下，有几个字段非常关键：

- `customer.id`：Stripe 那边给我邮箱分配的客户 ID
- `userId`：OpenAI 这边给我账号分配的用户 ID

**这两个我改不了。**

它们不像 IP、语言、时区那样是一次请求里的表层信号——表层信号每次请求都可以重新填。这两个 ID 是档案编号，只要我用同一个邮箱、同一个账号，它们就跟我绑死。

任何成熟的支付风控系统，判一笔新订单时都会带上这个 customer_id 的历史一起评估——这不是黑魔法，是行业里的标准做法。

至于 OpenAI 和 Stripe 内部具体怎么用这段历史，我看不到。但只要它在被用，**第一次失败的污染，就会被带进第二次**。

更别说我之前还开过一次 OpenAI Support 工单。。。。。。而且 OpenAI 还正式回复了。

人工客服已经明确说过：如果账号基于或主要访问自不受支持地区，Codex for Open Source 这类权益可能无法兑换。

> 这条客服记录会不会被同步到风控决策里，我不能 100% 断言。

但在悬疑故事里，它至少是一条不能忽略的线索。

我说"理论上"——是因为这是我从结果反推的。两次几乎一模一样的失败模式（同样的 `confirm_error`、同样的 `requires_approval`、同样的中文报错），让我倾向于这样推测：

**问题不只在这次 checkout，更在我这个账号本身已经进入了某种更深一层的审核逻辑。**

或者更直接一点的可能：账号在 OpenAI 这边已经被挂上某种"地区不符"的内部标记，每次尝试 checkout 返回的不过是按这个标记走出来的模板结果。

我在 IP、时区、支付方式这一层折腾，像是在案发现场擦脚印。

但真正的档案，可能早就锁在另一个柜子里了。

---

## GitHub 上还有人在硬刚

研究到一半，我刷到一个项目：[DanOps-1/Gpt-Agreement-Payment](https://github.com/DanOps-1/Gpt-Agreement-Payment)。

这哥们干的事很硬核：把整条 `Stripe Checkout → PayPal billing agreement → ChatGPT manual-approval → Codex OAuth + PKCE` 链路从抓包里拆出来，写成了一套能跑的客户端。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260503120349282.png" alt="image-20260503120349282" style="zoom:50%;" />

它里面有反自动化校验、浏览器指纹环境、自愈守护循环、自动化执行链路——基本就是工业级。

是真的在和 OpenAI 风控打**协议级别**的对抗。

作者自己在 README 里写了一行数据：

> 批量注册次日存活率约 2%

这句话很吓人。

读懂它的意思——**用上面这一整套工业级工具链，跑出来的全新干净账号，第二天还活着的概率大约是 1/50**。

这是项目作者自己的经验数字，不是 OpenAI 官方数据。但作为参照，已经足够说明这条路有多不稳定。

而我这个号是：

1. 已经向 OpenAI Support 确认过地区问题的
2. 有两次 `confirm_error` 失败历史的
3. checkout 过程中出现过跨地区信号跳变的
4. 浏览器环境长期是中文 + 上海时区的

四个 buff 叠满，怎么看都不像是这局游戏能轻松通关的样子。

这个权益看似是 OpenAI 给你发了个福利让你蹬，对我来说，本质上更像是在告诉你：

**饭到嘴边了，但你就是吃不上。**

之前那种无所畏惧、直接开淦的勇气，我现在已经没了。

仿佛陷入了一种自我怀疑的逻辑怪圈。

而且这种方式也没有其他大佬可以请教。我至今没有看到过肉身在国内的国人，可以稳定地把这条链路打通。

更让我 fomo 的是，国外的人收到这个权益，可能真的就是一键领取。

---

接口扒完之后，我反而平静下来了。

不是我没掌握技巧，而是这件事从协议层就没给我留入口。

`is_business_ip2`、`Intl.timeZone`、`navigator.language`、`Accept-Language`、`customer.id` 历史、Support 工单——六束手电筒从六个不同的方向照过来。

每一束单独看，都不能定案。

但它们叠在一起，画出来的轮廓就很清楚了。

那个 GitHub 项目，本质就是把前四束手电筒一根一根拆掉。结果呢？新号存活率 2%。

技术能解决最浅那一层信号；解决不了平台地区政策，也解决不了账号历史和后续审计。

这就是上一篇文章那句话——**技术是全球化的，服务不是**——的具象化版本。

回到题图那句话：你在凝视深渊，深渊也在凝视你。

只不过深渊比你看得清楚。

它看到的不是一个"我想领福利"的开源作者。

它看到的是一组特征：zh-CN 浏览器、Asia/Shanghai 时区、可疑网络标记、跨地区跳变、Support 里已经确认过的地区限制。

这些特征拼在一起，比我自己照镜子还清楚。

OpenAI 给我发了 6 个月的 Pro 20x。我就站在他们家门口，把每一道门都亲手试了一遍。

试完，关机睡觉。

---

## 一点声明

这篇文章只是我个人扒接口的真实记录，**不构成任何账号操作、支付方式或合规建议**。

文里出现的字段名、JSON 结构都来自我自己浏览器开发者工具里看到的明面数据。我没有提供任何绕过风控的步骤，也不建议读者模仿类似操作。

我只是把页面自己返回给我的东西读了一遍，并写下我能看到的线索。

文中关于 OpenAI 内部风控、Support 工单记录是否影响后续判断等内容，都是我从失败模式做出的推测。OpenAI 内部具体怎么判，我没有也不可能有访问权限。

对中国大陆用户而言，OpenAI 的服务地区政策清晰且公开。本文不鼓励、不建议任何人通过虚假地区、异常支付方式、批量注册或其他规避政策的方式领取权益。具体以 OpenAI 官方支持地区列表和官方政策为准。

谢谢大家。
