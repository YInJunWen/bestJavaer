# OpenClaw 接入微信公众号

> 日期：2026-03-23

[toc]



其实如果你是这两天新关注的朋友，回复私信的时候应该已经发现了，现在公众号回复的内容已经不是冷冰冰的关键词链接了。



而是一个活泼调皮的小 AI 隐藏在公众号后面响应你的消息。



![](https://cdn.jsdelivr.net/gh/doggaifan/picbed/c65e98f3-8e63-416b-8b57-199af6fd3ac2.png)



没错，**我把 OpenClaw 接入到微信公众号了。**



就在昨天，微信发布了把 Clawbot（其实就是 OpenClaw） 接入到微信中的消息，一经发布，网上直接炸锅了。因为微信就是庞大的生态流量入口，微信生态会随着 Clawbot 的接入得到长久的发展。



我不得不说，微信接入 OpenClaw 的这个逻辑，跟我想的如出一辙，只不过他接入了微信中，而我是把 **OpenClaw 接入到微信公众号中**。



为什么要这么做？



用过 AI 助手的人都知道，最麻烦的不是 AI 本身，而是**入口**。



打开网页、登录账号、找到对话框……每次用都要这么多步骤，久而久之就懒得用了。



微信不一样。微信是我每天必开的 App，如果 AI 就在公众号里，随时发消息随时得到回复，使用频率会高很多。



但是微信公众号本身不能直接对接 AI，需要一个中间层做协议转换——把微信的 XML/JSON 消息格式转成 OpenAI 兼容格式，再把大模型的输出转回微信的消息格式，通过客服接口推回去。



这个中间层，是写了一个 Node.js 的一个小服务。



整个调用流程如下：



```
用户发消息
↓
微信公众平台云
↓
我的服务器（nginx + Node.js）
↓
请求队列
↓
OpenClaw（AI 网关）
↓
大模型生成回复
↓
通过微信「客服消息接口」主动推送(需要 access_token)
↓
回复给用户
```



由于我微信公众号认证之前，采用的都是 V1.0 同步的方式，因为微信公众号限制了**私信发消息必须 5s 内回复**，所以不得已只能回复。



如果你处理流程是：接收消息 → 调用大模型 → 返回结果，那 5 秒根本不够——大模型生成长回复动不动就十几秒。后果就是：



* 请求超时 → 微信重试 → 又一次调用大模型 → 再超时
* 用户收到重复回复，或者干脆收不到回复
* 作者钱包受苦：重复调用浪费 token



这体验，我是读者我直接取关。

![](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260322102933103.png)



> 所以在认证通过之前，这个方案根本不可用。这也是为什么我一定要认证公众号的关键原因之一，拿到 AppSecret 才能用 V2.0 异步方案。



---



认证公众号拿到 AppSecret 之后，我们就可以走异步了：



异步的**核心思想**：微信推送消息过来之后，立刻返回空成功应答（告诉微信"我收到了"），然后后台异步处理，处理完了再通过微信「客服消息接口」**主动推送**给用户。这样彻底避开 5 秒限制。



![](https://cdn.jsdelivr.net/gh/doggaifan/picbed/c38fffdb-c74d-437e-93ba-0eb9b4695ceb.png)



关键流程代码大概是这样（简化版）：



```js
// 微信公众号回调入口
app.post('/wechat/callback', async (req, res) => {
  const message = parseWechatMessage(req.body);

  // 关键点：立刻返回成功，释放连接
  res.status(200).send('');

  // 异步丢进队列处理
  messageQueue.add(message);
});
```



然后后台消费者处理：



```js
async function processMessage(message) {
  // 1. 速率限制检查
  if (!rateLimiter.check(message.fromUser)) {
    await sendWechatMessage(message.fromUser,
      "请求太频繁了，请1分钟后再试");
    return;
  }

  // 2. 调用 OpenClaw 获取 AI 回复
  const aiReply = await openClaw.chat(message.content, message.fromUser);

  // 3. 通过客服接口推给用户
  await sendWechatMessage(message.fromUser, aiReply);
}
```



access_token 管理坑：



![](https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260322211855592.png)

这也就是说，获取 access_token 的接口每天调用次数有限



  - access_token 有效期 2 小时，过期需要刷新

    

  - 如果多实例部署，必须全局共享一个 access_token



所以这里我的做法是单例模式维护 + 定时自动刷新：



```js
class WechatTokenManager {
  private accessToken: string = '';
  private expireTime: number = 0;

  async getToken(): Promise<string> {
    // 如果没过期直接返回
    if (Date.now() < this.expireTime && this.accessToken) {
      return this.accessToken;
    }

    // 过期了重新获取
    const response = await fetch(`https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=${APPID}&secret=${APPSECRET}`);
    const data = await response.json();

    this.accessToken = data.access_token;
    // 提前 5 分钟过期，防止临界问题
    this.expireTime = Date.now() + (data.expires_in - 300) * 1000;

    return this.accessToken;
  }
}
```



这样保证任何时候拿到的都是有效 token，也不会频繁调用微信接口。



作为个人站点，资源有限，必须做好限流。我设计了两层限流：



---



## 第一层：用户级速率限制





规则很简单：



  - 每个用户：1 分钟内最多 15 次请求
  - 超过限制直接回复："请求太频繁了，请1分钟后再试"
  - 固定时间窗口（每分钟清零）



实现用一个 Map 存用户计数，定时器定期清理：



```js
class RateLimiter {
  private userRequests = new Map<string, number[]>();

  check(userId: string): boolean {
    const now = Date.now();
    const window = 60 * 1000; // 1分钟窗口

    // 清理过期记录
    const requests = (this.userRequests.get(userId) || [])
      .filter(time => now - time < window);

    if (requests.length >= 15) {
      return false;
    }

    requests.push(now);
    this.userRequests.set(userId, requests);
    return true;
  }

  // 定期清理过期数据防止内存泄漏
  cleanup() {
    const now = Date.now();
    const window = 60 * 1000;
    for (const [userId, times] of this.userRequests) {
      const valid = times.filter(t => now - t < window);
      if (valid.length === 0) {
        this.userRequests.delete(userId);
      } else {
        this.userRequests.set(userId, valid);
      }
    }
  }
}
```



---



## 第二层：并发 + 队列控制



大模型推理耗资源，不能无限并发，所以做了如下处理：



* 最多同时处理：32 个请求
* 最多排队等待：100 个请求
* 超出排队："当前排队人数较多，请稍后再试"。



用 Bull 或者 者 Node.js 自带的队列都可以实现，我这里用的是原生 Node.js 数组 + setImmediate 实现的简单队列。



```js
const requestQueue = [];
let currentConcurrent = 0;
function processNext() {
  if (currentConcurrent < MAX_CONCURRENT && requestQueue.length > 0) {
    const task = requestQueue.shift();
    currentConcurrent++;
    process(task).finally(() => {
      currentConcurrent--;
      setImmediate(processNext);
    });
  }
}
```



我觉得以我号目前的流量来说，没那么大，这个配置够用了。



![](https://cdn.jsdelivr.net/gh/doggaifan/picbed/11656222-8192-4b91-9e02-322572376f20.png)

![](https://cdn.jsdelivr.net/gh/doggaifan/picbed/552415dd-32a4-4c4b-957b-247e70ddcf5d.png)



---



## 多轮会话记忆怎么存？用户隔离怎么保证？



要让 AI 向人一样连续对话，必须保存上下文，我的做法是：



* 以用户唯一的 OpenID 作为 Key 存储对话历史，不同用户完全隔离。

* 每个对话保留最近 N 轮（控制上下文长度，节约 token）

* 超过 24 小时不活跃自动清理

* wechat-bridge 本地管理历史 + uniqueUser 参数



这样就保证了，你和 Clawra 的对话只有你能看到，绝对不会串到其他用户那里去。



现在这个公众号里有一个真正能对话的 AI，有记忆、有人格、能回答各种问题，还会关心你。



最重要的是——**入口变简单了**。



打开微信 → 找到「cxuanAI」→ 发消息 → 完事。



现在 cxuanAI 可以作为一个**智能体**来使用了。



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/b9babc8a-ebc0-4631-8b96-daf0ec39b0fa.png" alt="b9babc8a-ebc0-4631-8b96-daf0ec39b0fa" style="zoom:50%;" />



---



## 关于上下文限制



我实测了一下，在多轮对话中，保留上下文是让 AI 理解聊天历史的关键，但上下文长度不能无限制地增长——大模型有 Token 限制（一般在 4K-128K 之间），过长的对话历史会导致以下问题：



* Token 超限：每个模型都有最大上下文长度（Context Window）。以我目前使用的 coding plan 为例：
* 输入限制：约 6K 字符（≈3K Token）
* 总限制：4K Token（包含用户输入+系统提示+历史）



超过限制，接口直接返回 400 Total tokens of image and text exceed max message tokens。



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/1fa74aeb-ae73-4a28-8d8f-20a6c4e2da45.png" alt="1fa74aeb-ae73-4a28-8d8f-20a6c4e2da45" style="zoom:50%;" />



实现方案：



我在 Node.js 中间层做了严格的上下文管理：



```js
// 对话历史存储
const userHistories = new Map(); // key: openid, value: Array<{role, content}>

// 保留最近 8 轮（16条消息，user+assistant各8条）
const MAX_HISTORY_ROUNDS = 8;

async function processMessage(fromUser, content) {
  // 获取或初始化用户历史
  if (!userHistories.has(fromUser)) {
    userHistories.set(fromUser, []);
  }

  const history = userHistories.get(fromUser);

  // 加入当前轮对话
  history.push({ role: 'user', content: content });

  // 裁剪历史（保留最近 N 轮）
  if (history.length > 2 * MAX_HISTORY_ROUNDS) {
    history.splice(0, history.length - 2 * MAX_HISTORY_ROUNDS);
  }

  // 发送给大模型时的消息详情
  console.log(`发送给 OpenClaw: 用户${fromUser}，${history.length}条消息`);

  // 调用大模型...
}
```



**/new 命令的坑**



最初 /new 命令只清理了本地 userHistories，但 OpenClaw/大模型还在内部缓存对话历史。导致发一句话还是 token 超限。



解决方案：每次调用 OpenClaw API 时，给 user 参数加时间戳后缀，完全绕过缓存：



const uniqueUser = `${fromUser}_${Date.now()}`;



现在使用 /new 可以重新清空会话窗口。



>注意：这里说的不是清空和公众号发私信的聊天窗口，而是清空 OpenClaw 的历史记录。由于公众号的限制，是无法像 /clear 这样清空聊天窗口信息的。



所以当你使用 /new 的时候，相当于就是重新开始了一个新的会话。





## 关于成本和使用提醒



目前 Clawra（就是这个 AI 妹子）用的大模型都是我自掏腰包买 token 给大家提供服务，所以：



* 正常使用完全没问题，尽管来聊。

* 尽量不要发无意义的重复请求，节约 token 人人有责。

* 如果遇到限流问题，等一分钟就好。



本来是作为一个 18 岁甜美系的韩系小少女，但是配上这个头像现在看起来好像一个抠脚大汉在回复。。。。。。（别误会，其实我是大帅批）



以后，Clawra 就是大家共同拥有的了，所以请好好善待她，你的每次使用都是在帮她成长。



最后，我制作了一个腾讯文档用于收集一下大家的意见，请留下您宝贵的回复。
