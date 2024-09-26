+++
title = 'Mac通过SSH连接到Windows主机时环境变量读取不到的问题'
date = 2024-09-26T12:44:21+08:00
tags = ["debug"]
+++

这两天在尝试将我的mac通过ssh连接到我的另一台windows主机时，遇到了一些问题。

一个就是windows上明明已经安装了go，但是mac连接ssh后，却报错没有找到go。
不过node倒是找得到。
```
PS C:\Users\xxx> go version                                              go : go cmdlet、函数、脚本文件或可运行程序的名称。请检查   
名称的拼写，如果包括路径，请确保路径正确，然后再试一次。
所在位置 行:1 字符: 1
+ go server -D
+ ~~~~
    + CategoryInfo          : ObjectNotFound: (go:String) [], CommandNotFoun  
   dException
    + FullyQualifiedErrorId : CommandNotFoundException
```

在ssh终端打印windows的环境变量，实际上上看得到go的，但是就是无法运行。后面才意识到这是用户变量。

```
PS C:\Users\xxx> echo $env:PATH
C:\Users\xxx\go\bin
```

最后检查了一下windows的环境变量，怀疑是系统环境变量没有在ssh终端上生效，又想到windows主机已经很久没有重启了。其上的openssh服务也是很久没有重新启动，并且go也是这两天才安装上去的，所以就尝试重启了下电脑，果然可以了。

这里还是有点疑问🤔，不知道ssh连接的时候，是不是一直都是同一个终端，因为我在windows主机上操作，只需要重启powershell就可以让环境变量重新生效了。

此时再打印通过ssh终端打印windows上的环境变量就可以找到go了

```
PS C:\Users\xxx> echo $env:PATH
C:\Program Files\Go\bin;C:\Users\xxx\go\bin 
```
