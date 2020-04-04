### rclone 开机自动挂载

```bash
yum install -y fuse
```


- 下载并编辑自启脚本(一旦不可用，GitHub 中有备份)

```shell
wget -N git.io/rcloned && nano rcloned
```



- 修改内容：

```none
NAME="Onedrive" #Rclone配置时填写的name
REMOTE=''  #远程文件夹，网盘里的挂载的一个文件夹，留空为整个网盘
LOCAL='/Onedrive'  #挂载地址，VPS本地挂载目录
```

- 设置开机自启

```shell
mv rcloned /etc/init.d/rcloned
chmod +x /etc/init.d/rcloned
update-rc.d -f rcloned defaults # Debian/Ubuntu
chkconfig rcloned on # CentOS
bash /etc/init.d/rcloned start
```

看到 `[信息] rclone 启动成功 !` 即可。
