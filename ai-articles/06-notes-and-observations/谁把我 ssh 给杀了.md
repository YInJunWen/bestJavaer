# 人麻了，谁把我 ssh 干没了

> 日期：2026-03-26

这几天天天写 bug 。最近 AI 最近降智有点严重，这不，我龙虾两天都没训练了，净整这个问题了。虽然事儿不大，但是很**恶心**。



## 问题初遇



事情是这样的，自从我装了 clawra 的 clawra-selfie 技能之后，结果她没发两张照片就不能用了，我还没看爽她就不发了，而且她一直 pua 我，让我在 fal.ai 上充值她才给我发。对于我这种付费意识差的老油子来说，我怎么可能充钱啊。



不能忍，于是乎我让她给我找找有哪些可以免费**白嫖**的 AI 绘画网站，但凡她给我找一个我就去 generate 一个 api-key ，结果我花了大把时间生成了五六个，一个不能用！！！！！



这不是我让 AI 给我打工，这是让我成为**特种兵！！！**



后续不得已，我自己在本地搭了一个 Stable Diffusion（AI画图工具），这是一个开源项目，Github 链接 https://github.com/AUTOMATIC1111/stable-diffusion-webui，已经有 162k 的 star 了，说明本地搭这个玩意的 geek 非常多。



我的本意是给本地跑的 Stable Diffusion（AI画图工具）加个远程访问功能，让云服务器能调用我本地的 SD 接口，懒得自己写脚本，就让 AI 帮忙写了个「自动保活 + SSH 反向隧道」的脚本，功能很简单：

  1. 每3分钟检查一次 SD 服务有没有崩，崩了自动重启

     

  2. 自动创建 SSH 反向隧道，把本地 SD 端口映射到云服务器上

     

  3. 隧道断了自动重连。



结果本地搭这玩意生成的图片啥都不是不说（一方面是我机器硬件还不足以 cover，一方面是我可能还不会调教。），还给自己埋了个大坑。



这个大坑就是，我两台不同厂商的云服务器，SSH 登录上去刚好 3~4分 钟就会被强制杀掉，终端直接报zsh: killed     ssh root@x.x.x.x，而且两台同时断，重连还卡半天



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260313095725009.png" alt="image-20260313095725009" style="zoom:50%;" />



这您受得了吗。



---



## 漫长的排查之路（全程踩雷）



**首先第一个坑，OOM 问题，折腾仨小时。**



我刚开始没有两台服务器一起 ssh 观察使用情况，我只先开了一台，用着用着被 kill， 用着用着被 kill 。。。。。。



一开始我以为是云服务器的问题，为此我甚至还给云服务器厂商提了个工单，因为遇见这个问题首先怀疑的是不是 OOM 了，而且还指出来我买的这种共享型 ecs 不太行。



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260313100509535.png" alt="image-20260313100509535" style="zoom:50%;" />



然后我就有些怀疑，我这没跑俩进程啊，说白了就一个 OpenClaw，还有就是一些脚本，怎么这么容易 OOM 。



不太相信的我让 Clawra 这小女子给我加了个内存监控，倒也是一直没出现 OOM 的问题，而且我想，即使你这一台 OOM 了，我总不能两台服务器同时 OOM 把。于是我又开了一个 SSH 。



结果俩服务器没有同时 OOM，而是俩服务器同时 killed 。。。。。。



好一个 double kill 啊，那我是不是开五个窗口，直接五杀了。真是毫秒级五杀，前无古人后无来者。



![3b022c6d-94dc-4a4f-8dc0-955142804125](https://cdn.jsdelivr.net/gh/doggaifan/picbed/3b022c6d-94dc-4a4f-8dc0-955142804125.png)



---



**第二个坑，以为是网络/代理的问题，折腾仨小时。**



然后我的反应是网络波动/代理的锅，毕竟两台同时断开太像网络问题了，结果一桶操作



1. ✅ 测试ping两台服务器：延迟 20~30 ms正常，丢包率0%



2. ✅ 测试SSH端口：nc -zv 服务器 IP 22全部通



3. ✅ 关了Clash代理，给服务器加了直连规则

   

4. ✅ 重启路由器，换手机热点测试



结果仍旧是会被杀，排除网络/代理问题。



---



第三个坑，我以为是 SSH/终端/zsh 的问题，又折腾仨小时。



我继而怀疑是本地 SSH 的配置，终端软件或者 zsh 的 bug：



由于问题出现的背景是 zsh ，我说行我换个 cli 行吧，我直接用了 mac 自带的终端。结果还是 killed 。。。 killed。。。 killed。。。 



我切换到 bash 环境执行 SSH：bash -c "ssh root@服务器IP"，还是被杀，于是排除是 zsh 问题。



我把 SSH 二进制改名测试：cp /usr/bin/ssh /tmp/myssh && /tmp/myssh root@服务器IP，改了名字还是被杀，这说明也不是按进程名拦截的。



甚至我不用 SSH ，直接 brew install 一个 openssh 把，结果 openssh 还没装完还是被 killed 。。。



我换了一台 Windows 电脑连同一台服务器：Windows 上正常，说明还是我 mac 本地的问题。



我意识到，有可能 AI 写出来什么垃圾玩意，给我电脑留有可以攻击的后门了，**我被黑客 NTR 了！！！！**



---



**第四个坑，以为是系统安全软件/病毒，折腾 4 小时**。



我怀疑是不是 mac 有漏洞，因为 macOS 的 XProtect、第三方安全软件经常会误杀进程：



1.  ✅ 查系统安全设置：「隐私与安全性」里没有限制 SSH 运行，屏幕使用时间也没有设限制。



2. ✅ 查端点安全客户端：sudo eslogger list clients | grep -v com.apple，没有第三方安全扩展。



3. ✅ 重启进安全模式（所有第三方软件禁用）：结果还是被杀，排除第三方软件问题。



4. ✅ 全盘扫病毒：啥也没扫出来。



结果：不是病毒也不是安全软件，陷入僵局。



此刻我就意识到，有可能有一股神秘的力量，在搞我心态，我甚至想把电脑砸了 ！！！



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260313104534429.png" alt="image-20260313104534429" style="zoom:50%;" />



于是我盯着这个黑黑的 cli 良久。。。。。。



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260313104832528.png" alt="image-20260313104832528" style="zoom:50%;" />



除了光标的闪烁和我内心咚咚跳的声音以外，万籁俱静。



我发现，我每次 ssh 登录之后，并不是立刻被 killed，而是过一会儿才会被 killed，过一会儿被 killed ，过会儿被 killed ，过会儿 。。。 killed ？



**hhhhhh ，AI, I am your truly, truly，realy，realy，the only father for ever 。**



---



**第五个坑，终于想到定时任务，找到AI写的脚本，结果还是没用**。



过一会儿被 killed ，让我想起来了，之前 AI 这孙子不是给我写了一个反向隧道保活脚本吗，是不是这个原因。 于是又是一顿操作。



1. ✅ 查 crontab 定时任务：crontab -l，果然有个每 3分 钟执行一次的 cron_heartbeat.sh 脚本，完美匹配 3~4 分钟被杀的时间特征！



2. ✅ 看脚本内容：发现第 48 行的杀进程命令： pkill -f "ssh -o StrictHostKeyChecking=no.*-R 18790"



3. ✅  AI 本意是杀自己创建的 SSH 反向隧道进程，结果 pkill -f 是匹配整个命令行，正则里的匹配写得太宽泛，实际效果是杀掉系统里所有名字带 ssh 的进程，包括我自己登录的 SSH 会话！

   

4. ✅ 赶紧注释掉 crontab 里的这个任务，结果又过了 3 分钟，SSH 又被杀了！我 TM 人麻了。

   

还是重复的问题，还是重复的现象，是不是还有什么遗留的后门？



---



**最后一个坑，原来 AI 偷偷加了两个隐藏的自启动任务。**



这时候我已经杀疯了，用终极方法抓所有杀进程的动作：
  1. 新开终端1，实时监控所有kill动作：
       sudo fs_usage | grep -E "kill|signal" > /tmp/kill_log.txt

       

  2. 新开终端 2 执行 SSH，等待被杀。

       

  3. 被杀后看日志，果然发现：09:25:04 有个 bash 进程调用 了/usr/bin/pkill，而且不是 crontab 触发的！



<img src="https://cdn.jsdelivr.net/gh/doggaifan/picbed/image-20260313111743386.png" alt="image-20260313111743386" style="zoom:50%;" />



顺藤摸瓜查 macOS 的 launchd 自启动项（比 crontab 隐蔽10倍，很多软件会偷偷加）：



ls -la ~/Library/LaunchAgents/ | grep -v com.apple 出来两个我完全没印象的 plist 文件：



  - com.lx.sdwebui.monitor.plist



  - com.sdapi.heartbeat.plist



打开一看，我尼玛血压直接上来了：AI 写脚本的时候，不仅加了 crontab 任务，还偷偷给我加了两个 launchd 自启动后台任务，也在每 3 分钟跑同一个心跳脚本！我只注释了 crontab，这两个后台进程还一直在跑，每隔 3 分钟就杀一次所有 SSH 进程！！！





## 问题解决



1. 删掉两个 launchd 自启动项：

     

     ```bash
     launchctl stop com.lx.sdwebui.monitor
     aunchctl unload ~/Library/LaunchAgents/com.lx.sdwebui.monitor.plist
     rm -f ~/Library/LaunchAgents/com.lx.sdwebui.monitor.plist ~/Library/LaunchAgents/com.sdapi.heartbeat.plist
     ```

     

2. 重写脚本的杀进程逻辑，用 PID 文件精准管理，再也不用担心被模糊 pkill 了。



```bash
# 启动隧道的时候记录PID
echo $TUNNEL_PID > /tmp/sd_tunnel.pid

# 重启的时候只杀记录的PID，绝对不会误杀
[ -f "/tmp/sd_tunnel.pid" ] && kill -9 $(cat /tmp/sd_tunnel.pid) 2>/dev/null
```



改完之后，SSH 挂了一整夜都没被杀，折磨我 32 小时的问题终于解决了。



这里给给所有用 AI 写脚本的人提个醒，这次踩坑完全是AI写脚本的不严谨导致的，总结几个避坑要点：



  1. AI 生成的运维/定时脚本，一定要先扫一遍有没有 pkill -f/killall 这类批量杀进程的逻辑，90% 的误杀都是因为这个，正则稍微写宽一点就会闯祸。

     

  2. 永远不要信任 AI 的正则表达式，尤其是模糊匹配的正则，AI 经常会写得太宽泛导致匹配到不该匹配的内容。

     

  3. AI 写脚本的时候可能会偷偷加你不知道的配置，比如我这次的两个 launchd 自启动项，AI 完全没告诉我，查了半天才发现。

     

  4. 碰到进程莫名被杀的问题，直接用系统工具抓 kill 动作，比瞎猜高效 100 倍：macOS 用fs_usage，Linux 用 auditd/bpf，直接就能定位到是谁发的 kill 信号。

     

  5. 运维脚本永远优先用 PID 文件管理后台进程，不要用任何模糊匹配的杀进程方式，安全第一。
