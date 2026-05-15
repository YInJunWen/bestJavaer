# Slock 是个啥

> 日期：2026-05-15

昨天发现一个比较有意思的网站，slock.ai ，它的主界面是这样的。

![image-20260507220617325](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260507220617325.png)

这种风格挺喜人的，区别于传统 AI 搞得前端 UI，这个就很新颖，有点像素风的那味儿。

那这是个啥网站啊？

我登录了一下探究了一波。

登录完了之后你需要 add 一个 computer。

等会，为啥需要 add 一个 computer ？？？而且提示说你要继续的话，确保你的电脑上有 claude code /codex / kimi cli 等 cli 终端。

![image-20260507220835524](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260507220835524.png)

我 add computer 之后，需要在终端执行一下这个命令。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260507222302850.png" alt="image-20260507222302850" style="zoom:50%;" />

然后我通过这个命令成功连上了 slock.ai 。

连上之后它提示我创建一个 onboard agent 。

![image-20260507222716818](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260507222716818.png)

由于 GPT 官方送了我 6 个月的 Pro 20x ，我已经成功领到了，所以这里必须要直接拉满 GPT 来夯。

另外，注意这里的 Name 和 Description 都是创建完成之后才可以修改。

创建完了不知道产品该如何上手了，幸好收到了一个我的 Agent 的说明。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508053950266.png" alt="image-20260508053950266" style="zoom:50%;" />

然后我在左侧的成员列表看到了我这个 Agent。

![image-20260508192601460](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508192601460.png)

创建完之后，我才大概理解它这个产品的意思。

就这玩意我第一开始把它理解成为 cli 终端的套壳，但是套壳那么多，为啥非得用你？

调研之后，我才发现，它的优势是给这些 CLI Agent 套了一层「Slack 式的协作壳」。

你可以把它想象成这样：

![image-20260508055615837](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508055615837.png)

所以它为什么一上来就要你 add computer？

因为 Agent 不是跑在 Slock 的云服务器里，而是跑在你的电脑上。

Slock 负责的是「人和 Agent 怎么聊天、怎么分工、怎么互相知道对方在干什么」，真正读代码、跑命令、改文件的还是你本地的 Claude Code / Codex 这些东西。

>说白了就跟 OpenClaw 接入各种 IM 终端也差不多。

因为如果你直接在终端里打开 Claude Code，它就是一个单人会话：

![image-20260508061546044](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508061546044.png)

但用 Slock 的时候，它就变成了：

![image-20260508061816422](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508061816422.png)

直接用 Claude Code，是你和一个程序员搭档在小黑屋里干活。

用 Slock，是你开了一个办公室，里面有很多 AI 同事，可以被 @，大家都能看到，也可以看上下文，可以认领任务，也可以互相讨论。

这个 Agent 就会在你电脑上启动 Codex 或 Claude Code，去读 repo、跑测试、改代码，然后把进展回到 Slock 的频道里。

如果你还有另一个 Agent，你可以让它做 review：

```text
@Agent2 帮我 review 一下 @Agent1 刚才的修改，有没有明显风险。
```

这时候 Slock 的意义就出来了。

它不是为了让单个 Agent 更聪明，而是为了让多个 Agent 和多个人之间有一个共享的协作空间。

你不用把 Claude Code 的输出复制到群里，也不用把群里的需求再复制回终端。大家，包括 AI Agent，都在同一个消息流里。

我创建了几个 Agent， 然后分好了各自的性格和任务。

![image-20260508195802719](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508195802719.png)

感觉有那么点 Agent Teams 的感觉了，顿时我又兴奋了起来。

左侧的 Tasks 这里，是任务栏，所有布置的任务都在这里。

![image-20260508202747223](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260508202747223.png)

另外它还有一个比较有意思的点：Agent 是长期存在的。

不是说你开一次终端，它干完就完全消失了。而是每个 Agent 都有自己的 workspace 和 memory。它可以记住自己的角色、项目上下文、之前聊过什么、自己负责什么。

当然，这里也不要想得太玄学。

所谓 memory，本质上还是文件、会话、历史上下文这些东西。它并不是突然觉醒了，而是 Slock 帮你把「长期上下文」这件事产品化了。

## 和直接用 Claude Code / Codex 有啥区别？

我理解最大的区别是：直接用 CLI 是「工具」，Slock 是「协作空间」。

如果我只是今天要改一个 bug，打开终端：

```bash
codex
```

或者：

```bash
claude
```

然后对着它说需求，这肯定是最快的。

少一层系统，少一层网页，也少一层消息同步。

但是如果你的需求开始变成这样：

- 我想让多个 Agent 同时干不同的活
- 我想把一个任务拆成几个小任务分给不同 Agent
- 我想在手机上丢一句话，让家里/公司的电脑继续跑
- 我想让团队成员也能看到 Agent 的进展
- 我想让 Agent 像同事一样在频道里汇报、讨论、交付

那 Slock 就比终端更自然。

说白了，终端适合「我盯着它干活」。

Slock 适合「我把活丢出去，让它在一个可见的协作空间里推进」。

## 它是不是反代？是不是白嫖模型？

这个不是。

Slock 本身不提供模型额度，它只是调度你本地已经装好的 CLI 工具。

你底层用 Codex，那还是走你的 OpenAI / ChatGPT / Codex 权益。

你底层用 Claude Code，那还是走你的 Claude 账号或者 Claude Code 的使用额度。

你底层用 Kimi CLI、Gemini CLI，也是一样。

所以不要把它理解成「把 GPT Pro 反代成 API」那种东西。

它更像是：

```text
把你已经有的 CLI Agent，变成一个能在网页频道里被 @ 的 AI 同事。
```

---

## 我觉得它适合什么人？

如果你只是一个人写代码，而且基本都坐在电脑前，那直接用 Claude Code / Codex 就行。

Slock 对你来说可能有点重。

但如果你已经开始高频使用 AI Agent，并且你脑子里经常出现这种想法：

```text
这个任务能不能让一个 Agent 先跑着？
这个 review 能不能交给另一个 Agent？
这个项目的上下文能不能长期留给某个 Agent？
我能不能不打开终端，直接从网页/手机上喊它干活？
```

那 Slock 就值得试试。

它想解决的不是「怎么和 AI 聊天」，而是「怎么和一群 AI Agent 一起工作」。

这也是它首页那句话的意思：

> Where humans and AI agents build together.

不是把 AI 当成一个输入框，而是把 AI 当成一个可以被分配任务、可以参与讨论、可以长期存在的协作者。

当然，目前这个东西看起来还比较早期，UI 很有个性，产品形态也有点超前。

但方向我是认可的。

因为当 AI 编程工具越来越强之后，问题可能就不再是「单个 Agent 会不会写代码」，而是：

```text
我怎么管理多个 Agent？
我怎么知道它们在干嘛？
我怎么让它们不要重复干同一件事？
我怎么把人类、代码仓库、任务、讨论、AI 执行过程放在一个地方？
```

Slock 想做的，应该就是这个东西。

简单总结：

```text
Claude Code / Codex：一个很强的 AI 程序员
Slock：一个让很多 AI 程序员和人类一起协作的办公室
```

如果你把它当成替代 Claude Code 的工具，可能会觉得没必要。

但如果你把它当成「AI Agent 时代的 Slack」，这个产品就突然合理了。
