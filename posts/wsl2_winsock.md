# WSL2: 参考的对象类型不支持尝试的操作

这一般是由于有软件使用了 winsock 引起的, 例如 proxifier.



很遗憾目前(20191015)还没有办法同时使用 proxifier 安装版 和 wsl2. 但可以使用 proxifier 便携版.



让 WSL2 恢复正常工作(同时破坏 proxifier)的方法是:

​		使用管理员权限在 PowerShell 或 CMD 中运行:

​		`netsh winsock reset`

