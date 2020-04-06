# 整理我常用的 tmux 指令

### tmux 分栏

```bash
# 纵向分栏
Ctrl + b -> %
# 横向分栏
Ctrl + b -> "

# 切换
Ctrl + o

```

### tmux批量操作：

```
ctrl+b 以后输入
:set synchronize-panes
```

现在你输入的命令会同时显示在8个窗口中, 如需关闭：
```
:set synchronize-panes off
```
