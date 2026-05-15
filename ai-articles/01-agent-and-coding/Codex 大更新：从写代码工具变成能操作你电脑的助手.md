# Codex 大更新：从写代码工具变成能操作你电脑的助手

> 日期：2026-04-18

Codex 迎来了重大更新，新推出了 `Computer use` 电脑助手这个功能，需要在 Mac 本地安装 Computer use 插件（Windows 估计也很快就推出了）

你可以在 Codex 的桌面版上设置里面来安装 Computer use 。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260418155533936.png" alt="image-20260418155533936" style="zoom:50%;" />

你可以直接借助计算机，Codex 可以查看和操作 macOS 上的图形用户界面。

它可以用于命令行工具或结构化集成不足以完成的任务，例如检查桌面应用程序、使用浏览器、更改应用程序设置、处理非插件形式的数据源，或重现仅在图形用户界面中出现的错误。

Codex 官方是这么形容的。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260418100838365.png" alt="image-20260418100838365" style="zoom:50%;" />

如果你要在 Mac 上面安装，需要开启

1. **屏幕录制**权限，以便 Codex 可以看到目标应用程序。
2. **授予辅助功能**权限，以便 Codex 可以点击、输入和导航。

不过只要你 Install Computer Use ，CodeX 会指引你进行安装的，非常丝滑。

![c4bd2859fd980a588df1eac79b73c05d](https://cdn.jsdelivr.net/gh/doggaifan/picbed/c4bd2859fd980a588df1eac79b73c05d.jpg)

Computer use 解决了一个长期痛点：给那些没开放 API 的软件一点 GUI 级别的震撼。以前 agent 碰到这类应用就歇菜，现在 Codex 直接像人一样手动操作。

它的原理是：模型截取屏幕截图，理解界面内容，然后返回要执行的操作指令——"点击右上角按钮"、"在输入框填入 XXX"、"滚动页面"。这些操作交给 harness 执行，结果再截图回去，形成闭环。

这一波更新还加入了 90 多个插件，把 Atlassian Rovo、CircleCI、CodeRabbit、GitLab Issues、微软全家桶、Databricks 旗下的 Neon 等工具都接了进来。

## 实测：它到底能做什么

我让 Codex 自己回答了一下它都能做哪些事情。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/af3e279b8998f87d6dd19279b3dc2e9b.png" alt="af3e279b8998f87d6dd19279b3dc2e9b" style="zoom:50%;" />

然后我让它演示了一下它风险最低的测试——让它看了一下系统的运行情况。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260418154335455.png" alt="image-20260418154335455" style="zoom:50%;" />



演示了一下，让它给我媳妇发一条微信消息。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260417204735178.png" alt="image-20260417204735178" style="zoom:50%;" />

甚至发一条朋友圈也不是什么问题。

<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260418154419351.png" alt="image-20260418154419351" style="zoom: 67%;" />

## 官方说：什么时候该用，什么时候不该用

OpenAI 官方文档里有一句重要提示：

> Because computer use can affect app and system state outside your project workspace, use it for scoped tasks and review permission prompts before continuing.

官方列出了适合用 Computer use 的场景：

- 测试 macOS 应用、iOS 模拟器流程或其他桌面应用
- 与没有 API 的软件交互
- 重现只在图形界面里出现的 bug
- 更改应用设置
- 处理没有插件形式的数据源

不适合的场景：涉及支付、删除大量数据等高风险操作，以及需要登录认证且无法人工确认的敏感操作。

## 我感觉它是 GUI 版 Claw

OpenClaw 大家都知道——让 AI 控制你电脑的工具，鼠标、键盘、浏览器都能接管。好处是免费开源，坏处是配置麻烦，纯命令行，新人劝退。

而我感觉 Codex 内置的 Computer use ，就相当于是一个 GUI 版本的 Claw，不知道大家如何认为的，毕竟 OpenClaw 最为严厉的父亲 Peter 已经入职了 OpenAI，我认为这次的 Computer use ，就是以后 AI 会成为接管你个人 PC 的雏形。

Codex 现在就是这套逻辑的 GUI 版本——有界面、有项目隔离、有 Skills 扩展、有 Automations 自动化。OpenClaw 能做的它基本都有，还多了 OpenAI 原厂的支持和分发渠道。

Claude Code、Cursor 这些竞品都在往通用 Agent 的方向走。OpenAI 这次的策略很清楚：把 Codex 从编辑器里的编程助手，变成一个能跨应用、跨时间、跨工具链持续干活的数字同事。

这不是功能叠加，这直接是**定位迁移**了。

## 来源

- [Computer Use – Codex app - OpenAI Developers](https://developers.openai.com/codex/app/computer-use)
- [New Codex features include the ability to use your computer in the background - Ars Technica](https://arstechnica.com/ai/2026/04/new-codex-features-include-the-ability-to-use-your-computer-in-the-background/)
- [OpenAI expands Codex beyond coding with computer use, memory and plugins - Neowin](https://www.neowin.net/news/openai-expands-codex-beyond-coding-with-computer-use-memory-and-plugins/)
- [OpenAI's Codex Desktop can run your computer now - ZDNET](https://www.zdnet.com/article/openai-codex-desktop-update/)
- [Run long horizon tasks with Codex - OpenAI Developers](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex)
- [OpenAI drastically updates Codex desktop app - VentureBeat](https://venturebeat.com/technology/openai-drastically-updates-codex-desktop-app-to-use-all-other-apps-on-your-computer-generate-images-preview-webpages/)
- [OpenAI Codex Update Adds Computer Use, Image Generation, and Plugins - MacRumors](https://www.macrumors.com/2026/04/16/openai-codex-mac-update/)
