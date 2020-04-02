

# Caddy 配置自动启动与反向代理

本文转发自 https://xiaoxx.cc/

本质上要做的事情是让caddy做反向代理服务器转发其他网站的流量，caddy的好处是自己申请证书实现https。



# 安装

## Get caddy

```
sudo curl https://getcaddy.com | bash -s personal
```


# 配置

## 注册caddy服务

让caddy拥有非root用户打开端口的权限

```
sudo setcap 'cap_net_bind_service=+ep' /usr/local/bin/caddy
```

如果出现`setcap: command not found`那就安装一下`libcap2-bin`：

```
sudo apt install libcap2-bin
```

创建用户和所需目录并且只赋予必要的权限

```
sudo groupadd -g 33 www-data
sudo useradd \
  -g www-data --no-user-group \
  --home-dir /var/www --no-create-home \
  --shell /usr/sbin/nologin \
  --system --uid 33 www-data

sudo mkdir /etc/ssl/caddy
sudo chown -R root:www-data /etc/ssl/caddy
sudo chmod 0770 /etc/ssl/caddy

sudo touch /var/log/caddy.log
sudo chown root:www-data /var/log/caddy.log
sudo chmod 0770 /var/log/caddy.log

sudo mkdir /etc/caddy
sudo chown -R root:root /etc/caddy
sudo touch /etc/caddy/Caddyfile
sudo chown root:root /etc/caddy/Caddyfile
sudo chmod 644 /etc/caddy/Caddyfile

sudo mkdir /var/www
sudo chown www-data:www-data /var/www
sudo chmod 555 /var/www
```

上面创建了三个目录，`/etc/caddy/Caddyfile` 是 Caddy 的配置文件，`/etc/ssl/caddy` 存放证书，`/var/www` 是默认的网站目录。

把官方提供的脚本 [caddy.service](https://github.com/mholt/caddy/blob/master/dist/init/linux-systemd/caddy.service)下载到 `/etc/systemd/system/` 并重新加载 `systemd daemon`，让配置生效。

```
wget https://raw.githubusercontent.com/caddyserver/caddy/master/dist/init/linux-systemd/caddy.service
sudo cp caddy.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/caddy.service
sudo chmod 644 /etc/systemd/system/caddy.service
sudo systemctl daemon-reload
sudo systemctl start caddy.service
```

让 Caddy 开机自启。

```
sudo systemctl enable caddy.service
```

至此，Caddy 已经成功注册服务，并能够开机自启了。

## 配置Caddyfile

修改`/etc/caddy/Caddyfile`文件内容如下，用来配置反向代理，`mydomain.me`替换为你的域名：
（点右上链接后可以编辑配置文件后再复制）

```
/etc/caddy/CaddyfileCaddyfile
mydomain.me
{
  root /var/www/mydomain.me
  tls 你的邮箱
  log /var/log/caddy.log
  proxy /path sourcesite:port {
    websocket
    header_upstream -Origin
  }
}
```

重启caddy服务器

```
sudo systemctl restart caddy
```

