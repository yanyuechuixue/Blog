

## 利用Google GCE为教育网搭建6to4服务

**转载原创文章请注明，转载自： [Luminous' Home](https://luotianyi.vc/) » [利用Google GCE为教育网搭建6to4服务](https://luotianyi.vc/1941.html)**



各大高校接入的中国教育网（CERNET）的IPv6网络一直是不设限速而且免费的，而IPv4的网络要么是价格高昂（1块1G的都有），要么采用双栈接入直接走三大运营商的带宽。Luminous的校园网就是属于后面那一种，IPv4由电信提供，并不走教育网线路，电信出口限速20M（不过5块一个月还要啥自行车(￣▽￣)”）

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_07-47-44.png)

当初其实刚刚办理了校园网就发现学校采用的是千兆的交换机组网，然而只有在走教育网的时候才不会被限速到20M，满足这个条件的就两个，要么是位于教育网网内的IPv4服务器，要么是使用IPv6访问网站

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_07-49-03.png)

当然，一直以来的玩法局限于拿来下科大镜像站的系统镜像，常常能跑到50-70m/s

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-00-15.png)

前几天突然有了通过IPv6连接代理实现6to4顺便利用这带宽爽一下的念头d=====(￣▽￣*)b，开搞

------

## 一、在坑里摸索

> **全文用作代理的工具为V2ray，模式为WS+TLS+NGINX转发，受于特殊原因后文不再详述，如有需要自行了解**

既然要走IPv6，那非常简单啊，~~国内找个最好~~（然而那点国内那点带宽不如直连算了……）

不慌，美西还有一大把带IPv6的服务器，随手摸了个DigitalOcean的VPS测试（⊙ｏ⊙）

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-09-42.png)

emmm……速度不太如意(⊙x⊙;)，从大佬那里了解到CERNET的国际出口只有10G到美西he.net和10G到香港hkix，以及最近在美西接入的cogent

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-18-26.png)

显然，到do走的he.net的那个口子，而且被严重的QoS了，没办法跑起来……在美西还有一个cogent，对国内用户很少有卖cogent单线的服务器的，翻了一下CF的ASN找到了一组走cogent到cf的/48段（V2可以用CF进行转发）

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-22-24.png)

然而实际测试发现找不到这个IP段下属的服务器……/48只分配到了交换机，IPv6也没有办法去扫段，果断弃之（其实看那个延迟也觉得不是多靠谱）

------

这下就只能考虑走HKIX出口的了，问来问去，也就Leaseweb和dmit走HKIX直连，其他的大多亚太的IPv6都要绕美西的he.net

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-41-33.png)

Leaseweb就不提了，大厂的东西也实在是不太好买价格也不亲民，看看dmit吧……

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-41-01.png)

15刀一个月！告辞！(///￣皿￣)○～

突然想起看油管教育网直连是走hkix过去到澳大利亚的，那么到谷歌云GCP香港也应该也是走hkix

问题是GCP的VPS并不支持IPv6，一番查找之后得知可以通过谷歌云的负载均衡（LoadBalance）进行转发，那么就开始吧

------

## 二、配置Load Balance并绑定IPv6

首先我们要创建一个VM实例，然后在Computer Engine选项下选择实例组，创建一个与VM区域对应的单区域实例组

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-51-01.png)

然后直接在上方搜索Load Balance（原谅我没一下找到），进去控制台之后直接选择高级菜单

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-54-08.png)

上方需要配置的三个选项卡如图

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-55-38.png)

首先选择后端服务，创建后端服务。如图，协议选择TCP，选好对应的刚建立的实例组，然后填入VPS上代理的端口（我是通过NGINX转发所以代理端口是443端口）![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_08-58-46.png)

随后选择目标代理选项卡，创建目标代理。选择好对应的后端，类型TCP，代理协议选择关闭。

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-02-09.png)

最后选择转发规则，点击创建全局转发规则。

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-04-02.png)

选择TCP协议，端口为转发到LB的的端口，选择一个合适的即可。IP选择V6，目标选择上一步建立的目标代理。

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-05-06.png)

创建，静候5分钟，访问端口，可以看到转发已经成功

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-08-46.png)

------

## 三、测试效果

路由测试，延迟65ms，非常优秀……

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-11-31.png)

再测试一下速度，非常优秀！不错！捞比！

受限于最小的VPS的性能，这个速度基本上是达到极限了(。・∀・)ノ

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-12-18.png)

------

## 四、价格

之所以选择GCE，是因为GCE有免费300刀一年的试用（。＾▽＾）

根据谷歌的官方文档是每小时0.035美金，算上连续使用优惠，大概是15刀一个月……

![img](https://blog.cdn.luotianyi.vc/wp-content/uploads/2019-07-02_09-17-08.png)

所以说嘛……爽爽完了就够了……真用起来也不便宜的……
