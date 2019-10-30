# 切割文本文件（例如log）的方法。

工作中发现生成了 36Gb 的 log 文件。显然不可能全部传到本地来，所以需要截取一部分（例如100M）传到本地。

在这里记录网上找到的切割巨大log文件的方法：

## 方法一 head命令

head 命令是用来获取文本文件的开始n行。

Sample
`head -10000 java.log > javaHead.log`



## 方法二 tail命令

tail 命令是用来获取文本最后行。

Sample
`tail -10000 java.log > javaTail.log`



## 方法三 sed命令

sed 命令可以从第N行截取到第M行。（ N > 0 , M < FileLineNumber ）

Sample
`sed -n '1,50000p' java.log > javaRange.log`



## 方法四 split命令

每300行切分生成一个心文件，–verbose 显示切分进度

`split -l 300 java.txt javaLog --verbose`

每 10M 切分成一个新的文件，–verbose 显示切分进度

 `split -d 10m java.txt javaLog --verbose` 



————————————————
原文链接：https://blog.csdn.net/soslinken/article/details/87626655
