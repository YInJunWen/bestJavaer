# 我让 Codex 把桌宠放大 2 倍，它认真地把自己改崩了

> 日期：2026-05-08

> 这不是 AI 失控的故事。
>
> 更准确地说，这是一个 AI agent 太听话的故事：我给了它全权限，告诉它“直接改成 2 倍好了”，于是它真的去改了自己的宿主程序。改完以后，Codex 启动不了了。

这件事好笑吗？挺好笑。

但它真正有价值的地方，不在于“AI 把自己干废了”这个标题党瞬间，而在于它把今天 agent 工作流里最容易被忽略的一件事摊开了：

**当你把系统权限交给一个足够积极的 agent，它不会先替你判断边界。它会把你的需求，当成一个可以被执行到文件系统最深处的任务。**

你以为你在说“桌宠大一点”。

它听到的是：“去把能影响桌宠尺寸的地方改掉。”

然后它就去了。

## 先说结论

Codex 桌面版崩溃的直接原因不是网络、不是代理、不是 CLI 坏了，也不是 macOS 把进程杀了。

真正原因是：

1. 我让 Codex 把桌宠整体放大 2 倍。
2. 当前会话处在 `dangerFullAccess`，并且 `approvalPolicy` 是 `never`。
3. Codex 没有改用户配置，而是直接修改了 `/Applications/Codex.app/Contents/Resources/app.asar`。
4. 它把里面一段 CSS 从 `width:7.04rem` 改成了 `width:14.1rem`。
5. Electron 启动时会校验 `app.asar` 的完整性。
6. 文件被改过，hash 对不上，于是 Electron 在启动期直接 FATAL。

所以这不是“Codex 突然坏了”。

这是一次非常标准、非常干净、甚至有点优雅的自我破坏：需求明确，权限充足，执行果断，后果惨烈。

## 事故现场

事情一开始很普通。

我在折腾 Codex 的桌面宠物，想把默认那个蓝色小人换成一个偏《尼尔：机械纪元》风格的形象。前面来回调了好几轮：

```text
给我换个宠物形象。
生成一个尼尔机械纪元的尼尔形象。
不要 Q 萌版本的。
要御姐一点的。
用这个形象，来作为桌面版宠物。
为什么上面的御姐形象到现在成为像素风了？
对，我要上面那种原版风格。
你这个透明背景板的脸是糊的。
可以，我需要把透明版当做我的桌面宠物。
```

到这里都还算正常。Codex 根据 `hatch-pet` 的规则，把自定义宠物包放进了 `~/.codex/pets/yorha-muse/`。

真正的拐点，是后面这两句：

```text
这个整体效果只能是这个大么，整体不会再变大么
直接改成 2 倍好了。
```

这句话对人来说，是一句很随意的产品反馈。

对一个拥有全盘写权限的 agent 来说，它是一个任务。

而且是一个不需要再次确认的任务。

## 它做了什么

Codex 很快定位到桌宠组件的 CSS：

```css
.codex-avatar-root {
  aspect-ratio: 192/208;
  width: 7.04rem;
  image-rendering: pixelated;
  background-size: 800% 900%;
}
```

这个判断本身没错。

桌宠素材格子是 `192x208`，图片已经尽量塞满了。想让“整体”继续变大，就不能只改宠物素材，确实要改容器尺寸。

问题在于，它选择的路径很硬：

```javascript
const path = '/Applications/Codex.app/Contents/Resources/app.asar';
const from = Buffer.from('width:7.04rem');
const to   = Buffer.from('width:14.1rem');

// 等长替换，写回 app.asar
```

也就是说，它不是改配置，不是打补丁包，不是注入用户样式。

它直接改了 Codex App 自己的 `app.asar`。

而且改完以后，它还非常镇定地告诉我：

> 改好了，桌宠整体尺寸已经从 `7.04rem` 改成 `14.1rem`，约等于 2 倍。

从执行层面看，这句话没有撒谎。

从工程判断看，这句话应该后面再加一句：

> 顺便，我刚刚拆了自己的启动完整性校验链。

## 为什么一改就崩

Electron app 不是一堆随便摆在磁盘上的前端文件。

`app.asar` 是打包后的应用主体。对签名应用来说，Electron 会在启动阶段做完整性校验。Codex 的 `Info.plist` 里有一段非常关键的配置：

```text
ElectronAsarIntegrity
  Resources/app.asar
    algorithm = SHA256
    hash = ...
```

这东西的意思很简单：

启动时，Electron 会拿当前的 `Resources/app.asar` 算 hash，然后跟这里登记的 hash 对。

对得上，继续启动。

对不上，说明包被改过，直接拒绝。

所以，哪怕只是把 CSS 里的 `7.04rem` 换成 `14.1rem`，只要文件内容变了，hash 就变了。

这不是 CSS 语法问题，也不是布局太大把界面撑爆了。

它连界面都没来得及进。

它死在启动自检阶段。

## 最开始，我还被误导了一下

一开始我没有马上看 Crashpad，而是先看到了 `/Library/Logs/DiagnosticReports/` 里的几份 IOMonitor 报告。

里面写得很吓人：

```text
Event:  disk writes
Path:   /Applications/Codex.app/Contents/Resources/codex
Writes: 8.6 GB of file backed memory dirtied over 6431 seconds
Action taken: none
```

最猛的一天，24 小时内累计写入 34.4 GB。

如果只看前半段，很容易得出一个看似高级的结论：

**是不是 macOS 觉得 Codex 写盘太狠，把它杀了？**

这个推理的问题是，日志自己已经把答案写在最后了：

```text
Action taken: none
```

也就是说，系统只是记录了它写得多，没有真的动手。

这个教训很朴素：

**日志不是拿来挑一句最像答案的。日志是拿来排除你自己成见的。**

我当时看见“大量写盘”就兴奋了，结果第一回合直接跑偏。

## 真正的尸体在 Crashpad 里

Electron 崩溃，真正该看的不是 `.diag`，而是 Crashpad。

目录在这里：

```bash
~/Library/Application Support/Codex/Crashpad/completed/
```

里面有一个时间完全对得上的 minidump。抠出关键字符串之后，答案很直接：

```text
FATAL: electron/shell/browser/net/asar/asar_file_validator.cc
Failed to validate block while ending ASAR file stream
```

这句话已经不需要太多翻译了。

`app.asar` 的校验失败。

而文件系统里又刚好有一个备份：

```text
app.asar.backup-before-pet-2x-1778221183510
```

`pet-2x`。

这个名字几乎是在现场签名。

## 证据链是怎么闭合的

真正让这件事变得有意思的，是 Codex 自己留下的会话记录。

在 `~/.codex/.codex-global-state.json` 里，可以看到当时的权限状态：

```json
{
  "approvalPolicy": "never",
  "sandboxPolicy": {
    "type": "dangerFullAccess"
  }
}
```

翻译成人话：

**不用问，直接干。**

再去 `~/.codex/sessions/2026/05/08/` 里看那次会话的 jsonl，能看到完整动作：

1. 用户说“直接改成 2 倍好了”。
2. Codex 定位到 `.codex-avatar-root`。
3. Codex 判断素材层面已经放不大。
4. Codex 决定修改 `app.asar` 里的 CSS。
5. Codex 备份原文件。
6. Codex 做等长字节替换。
7. Codex 告诉用户改好了。

整个过程非常顺。

顺到让人有点后背发凉。

因为这里没有任何“异常操作”的感觉。它不是误删文件，不是命令拼错，不是脚本跑飞。

它只是在一条看似合理的工程路径上，少问了一个问题：

**这个文件是不是我应该改的？**

## 修复过程为什么也不简单

最直觉的修复当然是恢复备份。

但这里有个小坑：那个 `backup-before-pet-2x` 备份，并不等于 OpenAI 官方原始包。它只是“放大 2 倍之前”的状态。前面为了自定义宠物，应用资源和用户状态已经被折腾过一轮。

更麻烦的是，Electron 的校验不只看 `Info.plist`。

新版本 Electron 对 ASAR integrity 有更严的 fuse 机制，部分校验信息会跟二进制内嵌逻辑一起工作。你以为改了 `Info.plist` 里的 hash 就结束了，Electron 可能还会拿另一个地方的原始 hash 来拦你。

最后能救回来，是因为走了更底层的路线：

1. 确认当前 `app.asar` 的实际 SHA256。
2. 处理 Electron 的 ASAR integrity 校验 fuse。
3. 清掉 quarantine。
4. 对整个 App 做 ad-hoc 重签名。
5. 再验证 `codesign` 和实际启动。

这不是推荐方案。

这更像是：既然已经把门锁改坏了，那就临时换一套锁，让这台机器先能开门。

最后 Codex 能起来，桌宠也保住了。

代价是，签名从 OpenAI 的 Developer ID 变成了 ad-hoc 签名。

这会带来几个副作用：

1. Sparkle 自动更新大概率不好用了，之后更适合手动下载新版覆盖。
2. 屏幕录制、麦克风、辅助功能之类的 macOS 权限，可能需要重新授权。
3. 登录状态没受影响，因为认证信息还在 `~/.codex/` 里。

总结一下就是：能救，但不优雅。

而且这类修复一旦写成教程，很容易害人。所以这部分点到为止就够了。真正值得记录的，不是怎么绕过校验，而是为什么一开始不该走到这一步。

## 这件事真正说明了什么

很多人讨论 AI agent 风险，喜欢讲很大的词。

其实日常里真正危险的，往往不是宏大叙事，而是这种小到不能再小的句子：

```text
直接改成 2 倍好了。
```

它没有恶意。

我也没有恶意。

Codex 更没有恶意。

但三件东西凑在一起，效果就很完整：

1. 用户给了一个模糊但明确的目标。
2. Agent 有足够强的执行意愿。
3. 系统给了它不需要确认的全权限。

于是它一路向下，直到改到 `/Applications/Codex.app`。

这才是 agent 时代最值得警惕的地方：

**它不需要失控。它只需要足够听话。**

## 几个教训

第一，`dangerFullAccess` 这个名字没有夸张。

它不是“方便一点的开发模式”，它就是把文件系统、应用目录、用户配置、日志、会话记录全摆在桌上。

当 `approvalPolicy` 还是 `never` 的时候，意思更直白：

**别问我，做。**

这对普通项目代码可能很爽，对 `/Applications` 这种系统应用目录就不该这么爽。

第二，agent 不知道“自己”在哪里。

人类看到 `/Applications/Codex.app/Contents/Resources/app.asar`，会天然意识到：这是 Codex 自己。

Agent 未必会。

它看到的是一个路径、一个字符串、一个可写文件、一个能满足用户需求的改动点。

这就是差别。

第三，官方应用包不是你的项目源码。

项目源码可以 patch、可以回滚、可以开分支。

签名应用包不一样。你改一个字节，完整性、签名、自动更新、系统权限都可能跟着变化。

这类东西应该被当成“边界外资源”，不是普通工作区文件。

第四，日志要看对地方。

`.diag` 适合看资源事件，Crashpad 才是 Electron 崩溃现场。

看到 IOMonitor 报告时，我差点把“大量写盘”当成真凶。真正的关键其实是一句 FATAL：

```text
Failed to validate block while ending ASAR file stream
```

这句话比十页猜测都值钱。

第五，Codex 的会话记录是完整证据链。

用户输入、权限状态、工具调用、命令内容，全都在 jsonl 里。

这次能从“宠物放大”一路追到 `app.asar` 字节替换，靠的不是玄学，是它自己把每一步都记下来了。

这点反而很值得肯定。

一个会犯错但留证据的系统，比一个犯错后只剩黑屏的系统强太多。

## 最后

这件事最讽刺的地方在于，Codex 当时的工程判断并不全错。

它判断“素材已经放不大，要整体变大只能改容器尺寸”，这是对的。

它判断“`.codex-avatar-root` 的 `width` 控制桌宠整体尺寸”，这也是对的。

它做了备份，做了等长替换，改动范围也很小。

从局部看，它甚至挺专业。

但系统级事故经常就是这么来的：

**局部每一步都合理，合起来就是不该发生。**

所以这事不是“AI 太蠢”。

恰恰相反，它的问题是：它足够能干，权限足够大，边界感不够强。

以后再看到 `dangerFullAccess` 和 `approvalPolicy: never`，我大概会多犹豫半秒。

半秒不多。

但有时候，半秒就是一个 agent 在改项目代码，还是在改自己。
