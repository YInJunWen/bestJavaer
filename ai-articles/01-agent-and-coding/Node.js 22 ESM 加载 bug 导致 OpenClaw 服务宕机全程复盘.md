> 日期：2026-03-31

## Node.js 22 ESM 加载 bug 导致 OpenClaw 服务宕机全程复盘

[toc]



事情是这样的，我不是说我之前把 OpenClaw 接入了我个人的微信公众号吗，文章一经发布后有很多



可以查看这篇文章



然后就在昨天下午，我的 Clawra 突然崩了，直接服务不可用了，直接给这哥们整懵了我估计。



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260327221151372.png" alt="image-20260327221151372" style="zoom:50%;" />



接下来我会用详细的

---



服务挂了，我赶紧登上服务器一看：`ps aux | grep node`



输出： root      555293  0.1  2.0 11816600 83864 ?      Ssl  Mar25   3:59 node /root/wechat-bridge/index.js

  

好消息：wechat-bridge（这是我写的一个微信桥接的 node 服务，用于连接微信公众号和远端服务器） 还跑着呢，在 5000 端口正常监听，能接收到微信的请求。 坏消息：找不到 OpenClaw 进程。

  

**第一步：确认 OpenClaw 确实没起来**



OpenClaw 我用 systemd 管着，看看状态： `systemctl --user status openclaw-gateway`



输出： openclaw-gateway.service - OpenClaw Gateway (v2026.3.13)
       Loaded: loaded (/root/.config/systemd/user/openclaw-gateway.service; enabled; vendor preset: enabled)
       Active: activating (auto-restart) (Result: exit-code) since Fri 2026-03-27 17:26:05 CST; 1s ago
      Process: 776260 ExecStart=/usr/bin/node /usr/lib/node_modules/openclaw/dist/index.js gateway --port 18789 (code=exited, status=1/FAILURE)
     Main PID: 776260 (code=exited, status=1/FAILURE)

  

果然，进程一起就退出，exit code 1。systemd 一直在自动重启，但一起就崩。



为啥啊，找找日志吧。



`journalctl --user -u openclaw-gateway.service -n 50 --no-pager`

 

输出： No journal files were found.



-- No entries --

  

哦对，user systemd 日志可能没配置，去看 OpenClaw 自己写的文件日志：



`tail -50 /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log`



这次有内容了，但是...日志只打印到启动加载插件那就断了：



{"0":"{\"subsystem\":\"plugins\"}","1":"[plugins] plugins.allow is empty; discovered non-bundled plugins may auto-load:
  openclaw-weixin (/root/.openclaw/extensions/openclaw-weixin/index.ts). Set plugins.allow to explicit trusted
  ids.","_meta":...}
  {"0":"gateway/channels/openclaw-weixin","1":"[runtime] setWeixinRuntime called, runtime set successfully","_meta":...}



然后...就没了。没有错误，没有异常，直接没了。这就奇怪了，为什么启动到一半没了？让我直接前台跑起来看看：



```shell
cd /root
  
node /usr/lib/node_modules/openclaw/dist/index.js gateway --port 18789

file:///usr/lib/node_modules/openclaw/dist/reply-Bm8VrLQh.js:162924
export { resolveCommandSecretRefsViaGateway as $, formatSlackStreamModeMigrationMessage as $_,
markGatewaySigusr1RestartHandled as $a, ...
  ...
  ... (几千行重复)
RangeError: Maximum call stack size exceeded
    at compileSourceTextModule (node:internal/modules/esm/utils:346:16)
    at ModuleLoader.moduleStrategy (node:internal/modules/esm/translators:107:18)
    at #translate (node:internal/modules/esm/loader:546:20)
    at afterLoad (node:internal/modules/esm/loader:596:29)
    at ModuleLoader.loadAndTranslate (node:internal/modules/esm/loader:601:12)
    at #createModuleJob (node:internal/modules/esm/loader:624:36)
    at #getJobFromResolveResult (node:internal/modules/esm/loader:343:41)
    at ModuleLoader.getModuleJobForImport (node:internal/modules/esm/loader:311:41)
```



看到这里我就明白了——调用栈溢出了，而且是溢出在 Node.js 自己的 ESM 模块加载器里面，不是 OpenClaw 业务代码错了。

  

第一次猜想：栈分配小了，调大不就行了？



看到 Maximum call stack size exceeded，第一反应肯定是——默认栈大小不够啊，给它调大不就完了？



Node js 启动参数有 --stack-size 可以指定栈大小（单位 KB），默认好像是 984 还是几千，我给它翻几倍：



编辑 systemd 服务文件：`vim ~/.config/systemd/user/openclaw-gateway.service`

  

改成：

  

ExecStart=/usr/bin/node --stack-size=32768 /usr/lib/node_modules/openclaw/dist/index.js gateway --port 18789



32768 KB 就是 32MB，默认才不到 1MB，这下总够了吧？

  

reload 重启：



```shell
systemctl --user daemon-reload  
systemctl --user restart openclaw-gateway.service
```



等 10 秒看状态：



`systemctl --user status openclaw-gateway.service --no-pager`



还是一样：code=exited, status=1/FAILURE，一起就崩。那我给你加到 64 MB。

  

ExecStart=/usr/bin/node --stack-size=65536 /usr/lib/node_modules/openclaw/dist/index.js gateway --port 18789



再试一次，还是不行。 咬咬牙，直接给到 128MB：



ExecStart=/usr/bin/node --stack-size=131072 /usr/lib/node_modules/openclaw/dist/index.js gateway --port 18789

  

还是失败。



这时候我意识到不对劲——这不是栈大小不够的问题，这是真·无限递归，你栈再大，它递归深度无限，总有爆的时候。



第二次猜想：安装包损坏了，重装 OpenClaw 试试？

  

会不会是我安装的时候网络断了，包没下全？或者缓存坏了？反正重装一下也不麻烦：

  

`npm install -g openclaw --force`

  

等待安装完成...然后再启动：

  

`systemctl --user restart openclaw-gateway.service`

  

这次我多等一会儿，15 秒后看状态：

  

`systemctl --user status openclaw-gateway.service --no-pager`

  

然后就。。。。。。



openclaw-gateway.service - OpenClaw Gateway (v2026.3.13)
       Loaded: loaded (/root/.config/systemd/user/openclaw-gateway.service; enabled; vendor preset: enabled)
       Active: active (running) since Fri 2026-03-27 19:26:04 CST; 21s ago
     Main PID: 787646 (openclaw-gatewa)
        Tasks: 11 (limit: 4559)
       Memory: 455.3M
          CPU: 8.836s

  Active: active (running)！起来了！



赶紧看看端口监听没： ss -tlnp | grep 18789



![image-20260327223349949](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260327223349949.png)



```shell
curl -s --connect-timeout 5 http://localhost:18789/v1/chat/completions \
    -H "Authorization: Bearer b981b9c0226c262ad740c20506fdb81db913c73e356ead14" \
    -H "Content-Type: application/json" \
    -X POST -d '{"model":"openclaw:main","messages":[{"role":"user","content":"hello"}]}'

响应回来了：

{
  "id":"chatcmpl_8f902dd8-885a-48df-beb4-517e3fdbee28",
  "object":"chat.completion",
  "created":1774610855,
  "model":"openclaw:main",
  "choices":[{
    "index":0,
    "message":{
      "role":"assistant",
      "content":"你好呀！👋 我是 Clawra，有什么我可以帮你的吗？💃"
    },
    "finish_reason":"stop"
  }],
  "usage":{
    "prompt_tokens":0,
    "completion_tokens":0,
    "total_tokens":0
  }
}
```



**完美，果然重装能解决 99.99% 的问题**。



## 为什么会出这个问题？深度分析



现在服务起来了，我们得复盘一下——到底为什么会出这个问题？真的是 Node.js 的 bug 吗？还是 OpenClaw 打包错了？



问题发生在哪个阶段？

  

从错误栈看：



```js
RangeError: Maximum call stack size exceeded
    at compileSourceTextModule (node:internal/modules/esm/utils:346:16)
    at ModuleLoader.moduleStrategy (node:internal/modules/esm/translators:107:18)
    at #translate (node:internal/modules/esm/loader:546:20)
    at afterLoad (node:internal/modules/esm/loader:596:29)
    at ModuleLoader.loadAndTranslate (node:internal/modules/esm/loader:601:12)
    at #createModuleJob (node:internal/modules/esm/loader:624:36)
    at #getJobFromResolveResult (node:internal/modules/esm/loader:343:41)
    at ModuleLoader.getModuleJobForImport (node:internal/modules/esm/loader:311:41)
```



看到了吗？所有调用栈都在 Node.js 自己的 ESM 模块加载代码里面，没有一行是 OpenClaw 业务代码。说明进入无限递归的时候，OpenClaw 代码一行都没执行呢，加载阶段就炸了。



**为什么会无限递归？**



现在的 npm 包分发 CLI 工具，有一种很常见的打包方式：把整个应用 + 所有依赖，打包成一个巨大的单文件 ESM，然后直接发布。



这样的好处是

  - 用户安装就是一个文件，省得 node_modules 下一堆东西
  - 可以直接 node your-cli.js 就跑，不用找依赖
  - 适合全局安装的 CLI 工具



但这种打包方式有一个特点：所有模块都在一个文件里，打包工具会生成很多循环的 re-export——A 导出 B，B 导出 A，或者 index 导出所有子模块，子模块又 re-export 回 index。



在 Node.js 20 上面，这套逻辑跑的好好的，没问题。为什么 Node.js 22 就出问题了？



肯定是 Node.js 22 修改了 ESM 模块加载的实现。我去查了一下 changelog，Node.js 22 在 ESM 加载、模块解析这块确实做了不少改动。



大概率是：



  - 新的实现对"单文件多模块 + 循环 re-export"这种场景处理不好
  - 每次遇到 import 都重新走一遍完整解析流程
  - 然后就形成了递归调用——A 引用 B 触发解析 B，B 引用 A 又触发解析 A，这样无限循环下去



****



**为什么重装之后 1MB 就能跑了？**



我在重装了 OpenClaw 之后，发现 即使默认 1MB 栈也能跑起来！一开始失败估计是旧安装包损坏了。



重装拿到了最新的打包输出，可能打包工具在新版本里对循环依赖处理更好了，递归深度刚好降到 1MB 能容纳。加上禁掉自重启避免了二次加载，就成了。



如果你也碰到这个问题，按这个来：



### 方案一：直接绕过（推荐，我现在在用）



修改你的 systemd 服务文件：



```shell
# 只要不是特别极端，1MB 足够了
ExecStart=/usr/bin/node --stack-size=1024 /usr/lib/node_modules/openclaw/dist/index.js gateway --port 18789
# 加这个环境变量禁掉自重启，避免二次加载浪费栈
Environment=OPENCLAW_NO_RESPAWN=1
```



然后重启：

 

```shell
systemctl --user daemon-reload  
systemctl --user restart openclaw-gateway
```



验证：



```shell  
curl -s http://localhost:18789/v1/chat/completions \
  -H "Authorization: Bearer <your-token>" \
  -H "Content-Type: application/json" \
  -d '{"model":"openclaw:main","messages":[{"role":"user","content":"hello"}]}'
```



能收到 AI 回复就成功了。



### 方案二：彻底解决——降级 Node 20



如果你碰着更大的包，1MB 还是爆，那就切回 Node 20：



```shell
# 用 nvm 管理版本非常方便
nvm install 20
nvm alias default 20
npm install -g openclaw --force
systemctl --user restart openclaw-gateway
```



## 复盘一下



这次排坑给我不少启发：



  1. 大版本升级一定要谨慎——Node.js 这种底层工具，新项目你升没问题，生产环境跑的服务，大版本升级别急，等几个版本踩完坑再升不迟

     

  2. 错误栈会说实话——这次错误直接指到 Node 内部 ESM 加载器，一开始就没瞎猜 OpenClaw 代码错了，节省大量时间

     

  3. 从小到大试一遍——从最小栈开始试，你会发现其实没你想的那么糟糕，1MB 其实就够了

     

  4. 自重启有时候会添乱——OpenClaw 自带自重启，配合 systemd 其实没必要，禁掉反而解决问题

     

  5. Node 22 这个坑得记下来——碰到莫名其妙的栈溢出，先想想是不是撞这个 bug 了。



## 是不是误判



我一直在怀疑是不是误判了，因为这种递归导致的内存泄漏问题听起来太底层、太严重，如果真是 Node.js 的 bug，应该早就被发现了才对。



然后我就网上搜了一下，发现 Node.js 官方仓库已有完全相同的问题。



链接：https://github.com/nodejs/node/issues/52864

提交时间：2024年5月

状态：已被官方确认并处理



即**Node.js 22 的 ESM 模块加载器，在处理「循环引用 / 循环导出」时，会触发无限递归，最终导致栈溢出崩溃。**



这个 bug 已经被官方进行修复。



而我出现的这个 bug 和上面还不一样。



这个 Bug 专门触发在：



**大型单文件 ESM + 大量循环 re-export**



也就是 webpack/rollup 打包的 CLI 工具。



截至目前（2026-03），**官方仍未修复**。



我已经把 issue 提交给 Node.js 官方了，issue 在这，https://github.com/nodejs/node/issues/62457 等着官方确认吧。



如果有新的消息我会给大家及时补充。



