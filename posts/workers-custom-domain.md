### 创建项目

开始之前，我们检查一下自己是否已经安装了Wrangler，一般来说是没有的。

那么我们就需要先安装Wrangler，首先是配置Rust 运行时：

```shell
curl https://sh.rustup.rs -sSf | sh
```

然后安装Wrangler：

```shell
cargo install wrangler
```

接着我们创建一个项目，名字就叫test-worker：

```shell
cd ~
wrangler generate test-worker
cd test-worker
```

然后列出目录文件，你会看到一个index.js，编辑它：

```shell
vim index.js
```

修改完之后保存。

### 发布项目

你需要配置你的Cloudflare账户信息，首先我们打开Cloudflare官网：https://www.cloudflare.com/

登录到dashboard 后，右上角的头像那里点一下，然后点击My Profile，接着会进入个人资料页。

底部有一个API Keys，点击Global API Key 那一栏右边的View 按钮查看你的API Key，接着会要求你输入密码并进行人机验证，有个谷歌验证码。

![img](https://i.natfrp.org/caa043463ddf373ba06721c36c6a2072.png)

确认后就会显示出你的API Key，将其复制并保管好，它和你的密码一样重要。

回到Shell，输入以下命令进行配置：

```shell
wrangler config --api-key
```

然后我们修改wrangler.toml，这个是项目的配置文件，里面记录了一些关于这个Worker 的选项：

```
# 你的 Worker 名字
name = "test-worker"

# 你的 Cloudflare 账户 ID
# 在域名的 Overview 页面的右下角可以查看到 Account ID
account_id = "1234567890abcdefghijklmn"

# 你的 Cloudflare 域名 ID
# 同样是在域名的 Overview 页面右下角可以看到 Zone ID
zone_id = "abcdefghijklmn1234567890"

# 你想要绑定的域名
# 记得结尾加个 /*
route = "blog.natfrp.org/*"

# 设置应用程序的类型，默认是 webpack，无需更改
type = "webpack"
```

然后将项目构建并发布到Cloudflare：

```shell
wrangler build
wrangler publish --release
```

最后一步，增加DNS 解析，将你设置的域名前缀解析到你的workers.dev 域名。

例如我绑定了blog.natfrp.org，我的workers.dev 域名是zerodream.workers.dev，那么就将blog 前缀解析到zerodream.workers.dev，类型CNAME，并打开CDN 模式（点亮黄色的云）。

![img](https://i.natfrp.org/f074eead5507d5144a5fca7f67427317.png)

完成，现在你可以访问你的域名查看效果了。
