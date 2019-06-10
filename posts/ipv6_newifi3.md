## 免费上校园网（ipv6百兆带宽），自建宿舍NAS和网络打印机，技术分享！


**作者：卡卡罗特先生**

富强，民主，文明，和谐，自由，平等，公正，法治，爱国，敬业，诚信，友善

> 免责声明：本文讨论的技术和相关方法仅作为学术交流之用，因使用本教程造成的任何后果与文章作者无关。
> 未经作者许可，严禁转载抄袭（不好意思，我保存备用，该文章如无意外不会被搜索引擎搜到）。原地址：https://zhuanlan.zhihu.com/p/68421076

## **一 前 言**

你是否还在为校园网的收费而小心翼翼？你是否还在为网速不够快而影响科研进程？

你是否还在为处理舍友关系而费经心思？ 你是否还在为不能给舍友提供价值而苦恼？

那么，叶子团队或许能够帮助到你解决上述问题。利用技术手段合法免费上校园网，将网速提高至百兆带宽，为舍友搭建NAS私密文件分享平台与网络打印机。打造黑科技内部小密圈，废话不多，直接上图！

![img](https://pic4.zhimg.com/80/v2-f09e8cecbf62c85a02dae74ab993ab23_hd.jpg)（整体搭建、网速优化后测试数据）

![img](https://pic3.zhimg.com/80/v2-83de420f5214e6171be19df66acc24ba_hd.jpg)（油兔播放8K视频时速度展示，7680*4320@60hz 26Mbps）

![img](https://pic1.zhimg.com/80/v2-6efba79c8f55fd2f4486a9facdc0609c_hd.jpg)（每月流量1000G，足够常规使用需求）

![img](https://pic4.zhimg.com/80/v2-2ef38e97be49fc45cfba4a6a0a1c588f_hd.jpg)（宿舍分享后，反响不错）

阅读本文，您大约需要15分钟时间，实现本文技巧，您大约需要两个小时（当然你也可以远程求助）。

> 文章词汇替代词及命令集合包（下载阅读体验更好哦）：

```text
链接: https://pan.baidu.com/s/159FoAs-Pot2eBrRglHxsvQ 提取码: 182r
```

## **二 到底能实现什么功能?**

虽然IPV6在国内还没有普及，但是在校学生还是经常用到IPV6网络的。很多学校IPV6带宽没有限制的， 不但速度快，而且免费使用。因此，IPV6转IPV4不仅能让教育网用户免费高速上网，而且相对于较高的网费，价格低廉。

- 你的路由器能直接做代理吗？

**实现功能：**通过改造路由器系统，酸酸搭理配置，实现ipv6访问网络，大幅度提高速度。

- 你的路由器能接硬盘？

**实现功能：**通过接大容量硬盘，实现宿舍内部文件共享。

- 你的路由器能安装App？

**实现功能：**安装流量抓取工具（wireshark等），知道你家的设备访问详细情况。比如，家里来了谁？这个对抓小三或者带绿帽子有大杀器功效。

- 你的路由器能远程遥控？

**实现功能：**全网数据泄露后，第一时间将下载链接提交路由器下载，百兆带宽，供你第一时间掌握数据。

- 你的路由器能迅雷下载？

**实现功能：**下载大型游戏（100G以上）等，具消耗网速和电脑时间时，直接提交路由器下载至硬盘，为玩游戏保驾护航。

- 你的路由器能连接打印机吗？

**实现功能：**连接打印机，实现远程打印文件。

- ……………………………………………………等

实现如上功能原理拓扑结构如下：

![img](https://pic4.zhimg.com/80/v2-8d91041bee2c0c4aedaddd77923e3e4b_hd.jpg)（原理拓扑图）

当然，需要最重要的核心器件就是我们的 Newifi3(D2) 路由器。其实这种路由器并非是独一无二的。与它配置几乎一样的就有好几款，极路由，小米路由，等等。他们在硬件上的配置几乎完全一样，都是基于MT7620、MT7621、ar71xx等芯片的解决方案。

这里只打算谈一下 Newifi3。别的各家什么区别，各家什么的好。就不废话了。这些不是本文讨论的内容。可以说一下为什么选择它。答案是：因为便宜。在同等配置下，这款最便宜（官网原价599，因为矿难的原因，咸鱼目前是85-100左右可以入手）。

废话不多，要想做这个，先检测你的网络是否支持ipv6（用你家的网线连接电脑），测试地址：

```text
http://test-ipv6.com/
```

如下图，网络便是可以支持v6的。

![img](https://pic2.zhimg.com/80/v2-b26f4e8effafb70203ad5d7a342744fd_hd.jpg)（测试网络是否支持ipv6，若不支持打电话给运营商）

## **二 硬件层面需要什么？**

- Newifi3 （D2 ）路由器（咸鱼购买）；
- 有网口的电脑一台；
- 两根网线

## **三 软件层面需要什么？**

- PuTTY 远程连接器；

> 链接: [https://pan.baidu.com/s/1o5VqtgFsboXsAxL04ZNTdw](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1o5VqtgFsboXsAxL04ZNTdw) 提取码: zee4

- HFS搭建http服务器软件；

> 链接: [https://pan.baidu.com/s/1ua9dkn6XURTaWJtj5oaXgw](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1ua9dkn6XURTaWJtj5oaXgw) 提取码: ermu

- newifi-d2-jail-break.ko 破解原有路由器系统文件；

> 链接: [https://pan.baidu.com/s/1Rz-3oB1juV4AAcJddeZayw](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1Rz-3oB1juV4AAcJddeZayw) 提取码: 7k3q

- 路由器潘多拉系统文件；

> 链接: [https://pan.baidu.com/s/1nWxBGdumoIyiHazY71nHmQ](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1nWxBGdumoIyiHazY71nHmQ) 提取码: 9d9g

## **四 操作实现详细步骤（手把手教系列）**

## 第一阶段（路由器breed控制界面写入）

1. 购买Newifi d2 路由器

![img](https://pic2.zhimg.com/80/v2-da8ddf2a3ed570b6883201d9e8d6eccd_hd.jpg)（Newifi3（D2） 路由器）

2. 渠道路由器后，将你的路由器和电脑用一根网线连接（记得路由器供电并开启）；

a) 开启路由器，进入管理界面 (路由器管理 IP 地址是 192.168.99.1)；

b) 在浏览器中输入 [http://192.168.99.1/newifi/ifiwen_hss.html](https://link.zhihu.com/?target=http%3A//192.168.99.1/newifi/ifiwen_hss.html)；并进入，显示 success 即表明已开启 SSH；

c) 使用 PuTTY远程管理软件进入路由器操作系统；

![img](https://pic1.zhimg.com/80/v2-2d8aa31450a6ad8ae7dd6ee4412009a0_hd.jpg)（进入路由器系统SSH操作界面）

d）下载附件 newifi-d2-jail-break.ko； 用wget命令上传至 /tmp目录（这里推荐使用HFS搭建本地http服务器后，将newifi-d2-jail-break.ko上传至本地http服务器，在SSH里面用wget命令下载该文件）

```text
执行命令1（见文章开头附件）
```

> （注意，我这里的ip是自己根据路由器内网随便设置的，你可以自己选择，不懂得话，百度一下hfs怎么用就知道了）

![img](https://pic2.zhimg.com/80/v2-f394626bdb0117ae26700003459d8931_hd.jpg)

e）将上述文件下载至/tmp目录后，SSH 进入 /tmp 目录；加载 newifi-d2-jail-break.ko文件（这里的操作就是将newifi原有系统破解后，刷入breed控制台（breed相当于pc端的bois），为第二阶段将路由器刷入潘多拉系统做准备）；

```text
执行命令2（见文章开头附件）
```

f）此时 SSH 会停止响应，因为 newifi-d2-jail-break.ko 会冻结系统的其他功能，强制写入 Newifi3 D2 专用版 Breed 到 Flash；

g）成功后路由器会自动重启。断电后按复位健（直到lan口灯闪烁），开机便可进入 Breed控制台；

h）然后浏览器访问192.168.1.1 进入。显示如下页面，至此第一阶段成功！

![img](https://pic2.zhimg.com/80/v2-950661e02e580dedb7db35e021a96375_hd.jpg)（第一阶段成功标志）

## 第二阶段（路由器潘多拉系统写入）

1. 接下来刷入固件就简单了，点击固件更新，选择下载好的固件文件(潘多拉系统文件)，上传，之后确定刷入；

![img](https://pic4.zhimg.com/80/v2-85dccaf28b452039ebfca489ff97f187_hd.jpg)

![img](https://pic4.zhimg.com/80/v2-0026c62e2ae386aab947526a1ac7955f_hd.jpg)

2. 之后路由器重启（大约需要3-4分钟，别着急），路由器后台192.168.99.1，用户名为 root ,密码为 admin（如果访问不了后台，需要把电脑ip地址改为自动获得ip地址）。点击登录；

![img](https://pic1.zhimg.com/80/v2-5ecd0e95d8151a8d15daa3ee3862cd88_hd.jpg)

3. 将另一根网线一头是学校的网口，一头接路由器wan口；打开登录界面中的系统—软件包界面；

![img](https://pic4.zhimg.com/80/v2-8a5bf7c44f31214fe483f9722ad0dc07_hd.jpg)

a）在过滤器中搜索，这两个软件包并下载。

```text
执行命令3（见文章开头附件）
```

b）同时，安装这个软件包为防止上面那两个不能以后有备用解决方案。

```text
执行命令4（见文章开头附件）
```

c）卸载命令5；打开服务—如下界面，至此第二阶段成功！

```text
执行命令5（见文章开头附件）
```

![img](https://pic4.zhimg.com/80/v2-e9e88d8903a2c771e4af8256e6406b1b_hd.jpg)

## **第三阶段（vps配置）**

1. 在conoha或者linode 购买vps（笔者是在linode购买，每个月是5美刀）；基本是1个月1000G流量，配置是最低配。购买后开通vps，关于区域选择，一般各家VPS厂商都有对应区域的服务器测速网址可以测试一下载速度，选择速度快的区域即可，不知道怎么测可以优先选择日本、新加坡；

![img](https://pic2.zhimg.com/80/v2-4c8ef2b88e5ea1572c2e3dac61ddf841_hd.jpg)

2. 配置vps

a）用putty登录刚才购买的vps（ip，用户名，密码都是你刚才设置的）；

![img](https://pic1.zhimg.com/80/v2-ebc065736a78defe2dc3963e0b61f828_hd.jpg)

b) 输入如下命令配置酸酸：

```text
执行命令6（见文章开头附件）
```

![img](https://pic4.zhimg.com/80/v2-f16b7f146b107a49c2135dc527274c57_hd.jpg)

看到上图时，继续执行第二条命令：

```text
执行命令7（见文章开头附件）
```

![img](https://pic1.zhimg.com/80/v2-14effd707cff117460dee196a3f91c4c_hd.jpg)

看到上图时，继续执行第三条命令：

```text
执行命令8（见文章开头附件）
```

![img](https://pic2.zhimg.com/80/v2-0b0054f7bdcf33f9c9c5677c1784a94d_hd.jpg)

出现上图，请自己设置一个密码用于客户端，只要你自己能记住就行，输完回车，输入Default port，即端口号，端口号一般选择10000以上20000以下的数字（笔者是11111），输完回车。

![img](https://pic3.zhimg.com/80/v2-ade0ab1e59012371a5e1a5d14c12e84a_hd.jpg)

然后提示输入加密方式的编号，推荐chacha20-ietf-poly1305，也就是13(配图是之前的)，回车后等待配置完成，会出现如下画面：

![img](https://pic4.zhimg.com/80/v2-0d97cb4c379a63e2149b093ff61f4d2f_hd.jpg)

当出现以上信息时（服务器IP\服务器端口\密码\加密方式），切记，一定要好好保存以上信息：Server IP（服务器IP）、Server Port（服务器端口）、Password（密码）、Encryption Method（加密方式），因为之后你用任何设备在任何平台科学上网都需要填写这些信息！

c）之后我们需要设置ipv6支持；

```text
输入  执行命令9（见文章开头附件）  回车；
```

![img](https://pic3.zhimg.com/80/v2-0f97734a790f8255d7813ddb9c59834e_hd.jpg)

方向键移动光标，将server、fast_open两项改成我图上这样，其他不用动（注意！server中是两个英文冒号，这样修改后，酸酸是支持ipv4和ipv6的），然后crtl+o下面显示这样：

![img](https://pic1.zhimg.com/80/v2-e92a8161954484931f985df257852c64_hd.jpg)

回车保存，然后crtl+x退出，回到命令行，输入：

```text
执行命令10（见文章开头附件）
```

执行完后输入：

```text
执行命令11（见文章开头附件）
```

这时即vps配置完成！

## **第四阶段（路由器代理配置）：**

1. 登录路由器控制界面（192.168.99.1），进入潘多拉系统后，按照下图配置，将刚才的信息（版本选择酸酸原版； 服务器地址记得填写ipv6的地址，这个地址在linode界面查看； 端口号填写刚才你在vps设置的端口号； 密码也是刚才设置的）填写如下：

![img](https://pic3.zhimg.com/80/v2-6d9b414967f21e926e257e21ec8b7276_hd.jpg)

![img](https://pic4.zhimg.com/80/v2-56b49b4a04b969731d9a8f96b4b5f593_hd.jpg)

2. 在酸酸乳配置界面中的基本设置中，按照如下设置（记住，在点击启用之前，一定要退出校园网账户后再点击，这样才能实现免费上网）：

![img](https://pic1.zhimg.com/80/v2-3ace132fb2c7c19e6b0e8c28184929f4_hd.jpg)

至此，整个利用ipv6形式形成路由器代理后，大幅度提高上网速度和实现免费上网（需要注意的是：测速别用国内网站，这样是形成了4次循环。使用[www.speedtest.net](https://link.zhihu.com/?target=http%3A//www.speedtest.net/) 测速）

## **第五阶段**（硬件加速优化）**：**

第五阶段是利用路由器实现硬件加速，使网络速度更快！

1. 进入网络—硬件加速，按照如下配置：

![img](https://pic3.zhimg.com/80/v2-3079491131d6abafd1ef9aaa4dc299be_hd.jpg)

2. 进入网络—优化中心，按照如下配置：

![img](https://pic1.zhimg.com/80/v2-049e5921580a7ef49dd0da81ac90e4c0_hd.jpg)

3. 进入网络—Turbo ACC 网络加速，按照如下配置：

![img](https://pic4.zhimg.com/80/v2-5e6c92b582dc1f9842ad9d10b37b129f_hd.jpg)

4. 至此，进入系统—重启路由器后，至此所有步骤全部完成。测速如下：

![img](https://pic1.zhimg.com/80/v2-1fbdf28016bc355d16d23d6c95996d68_hd.jpg)（酸酸代理在pc的测速）

![img](https://pic4.zhimg.com/80/v2-4a82a57e7277502ebb323377eebfc3d3_hd.jpg)（酸酸代理在路由器的测速）

> 说明：因为，路由器和电脑端对网络包加解密的性能不同，因此带来测试不同。

后记，如果想继续降低网络流量延迟的话，在vps端配置bbr，一般网络延迟能够降低40%左右。类似的代理方式还有微兔软，配置更加复杂，但是作为备用手段，不失为一种解决方案。

## **五 更多的骚操作**

**1. 网络共享打印机**

路由器的usb接口插打印机，能实现共享打印（简单描述，感兴趣自己搞）。

a)潘多拉（就是我们用的这个）:去路由器软件包页面添加usbprint之类的插件，安装完重启路由器在服务标签内找到USB打印服务器就行了。

b)原版openwrt:也是安装插件，只是找的位置有点不同

c）网件固件:自带，按照说明书操作就行

d）提示:

- 其一、打印功能很吃路由器存储，8m以下rom的路由器谨慎操作尤其想直接驱动打进路由器的，16m以下路由器请不要打完整驱动，勾上x64就行，ia64和x86不要勾;
- 其二、打印功能也考验路由器CPU负载能力，建议去路由器内设置优先级到弱(使用webshell设置)，否则可能引起网络卡顿。
- 其三、不是所有的打印机都可以，不是所有的路由器都正常访问打印机，企业应用不靠谱。

**2. NAS网络存储文件分享**

NAS，这将是每个家庭的必备（打造内部文件分享存储）；优点如下；

a）低成本，一个路由器就是全部。也就是大概一个内存的钱。

b）低门槛，插上网线，接入移动硬盘即可。如果是2.5T的硬盘，连硬盘供电都省了，全靠USB（注意：newifi是12V2A，基本插入路由器硬盘在1-2T没问题；4T的可能就得注意，4T的移动硬盘一般都在0.8-1.2A，此时用Typec接口，因为typec口允许的供电比usb3.0大得多）。

c）低功耗，5V/1A的适配器就是全部的动力源。

d）低噪音，没有风扇，虽然散热鸡肋，但除了硬盘卡塔卡塔声以外，没有可以发出声音的部件。

e）低维护需求，7×24小时也不担心。宕机也好，掉线也好，重置一下，全都恢复正常。

f）有效的利用了宽带的闲置时间。减少的PC的损耗。

## **六 总 结**

1. 酸酸就是我们常说的socket5 代理；他是原生的，只对网络里面的包加密，完全遵循socket5协议。

2. 酸酸乳，在原生socket协议的基础上加入了混淆和相关的协议插件，然后可以把socket5的包伪装成HTTP的包或者HTTPS的包。但是这个伪装和混淆，相对来说他都不能完整的支持HTTP或HTTPS协议，防火墙去访问一下你这个IP，会发现你的服务器返回一个bad request。然后他就能检测出来，你这个不是真正的网站。同时你这个ip也没有用域名去绑定。所以说防火墙就有可能会把你这个判定为违规，而直接封掉你的IP。

## **七 后记：关于微兔软**

1. 微兔软，整合的TCP、KCP和websocket、quic协议。四种模式都支持。我们推荐配置的是websocket+tls。这个是完整的支持HTTPS协议的。而且可以通过反向代理，让我们ip访问变成一个真正的网站，这样防火墙去访问他，或者去ping/访问你的服务器，跟一个正常的网站的操作是一模一样的。因此，防火墙就识别不出我们的服务器是一个代理服务器。

2. 人们常用微兔软的TCP协议，其实与我们常说的带混合插件的SSR是一样的。同时，他还有一个KCP，这个协议是自己改过的。类似于TCP的另外一个协议，但比TCP的发包要灵活，隐秘性更好。

3. 总体来说就是酸酸 、酸酸乳。这两种配置微兔软都支持。但是微兔软比其他两种多了三种更高更好的代理方式。KCP、websocket+tls，还有一种就是quic（较为难配）。

4. 目前，咱们校园网IPv6，现在是没有防火墙的。所以说使用原生酸酸也无大碍，我们只是希望用IPv6科学上网+免流，酸酸就够用了，IPv4可以不用。但是，我们也要留一个后手。

希望我们的文字和经验能够帮助到有需要的人，如果你认为对你有帮助或者启发了你的思路，请多多点赞加关注啦！欢迎加入我们一起讨论各类黑科技。
```
酸酸：ss
酸酸乳：ssr
微兔软：V2ray


执行命令1：cd tmp
                   wget  http://192.168.99.123/newifi-d2-jail-break.ko
执行命令2：insmod newifi-d2-jail-break.ko
执行命令3：luci-app-ssr-pro     luci-i18n-ssr-pro-zh-cn
执行命令4：luci-app-v2ray-pro 
执行命令5：shadowfog
执行命令6：wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
执行命令7：chmod +x shadowsocks.sh
执行命令8:  ./shadowsocks.sh 2>&1 | tee shadowsocks.log
执行命令9：nano /etc/shadowsocks.json 
执行命令10: ssserver -c /etc/shadowsocks.json -d stop
执行命令11: ssserver -c /etc/shadowsocks.json -d start

```
