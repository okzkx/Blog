# Windows

## shutdown

shutdown -s -t 3600


## [Windows 系统命令行设置环境变量](https://blog.csdn.net/leonliu06/article/details/78586803/)

```
# 永久设置 GIT_HOME 变量为 abc
$ setx /m GIT_HOME abc

# 将 C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin 追加到 PATH 变量
$ setx -m PATH "%PATH%;C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin"
```
