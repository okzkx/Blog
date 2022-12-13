# Windows

## 激活

- Office 或 Window 可以直接使用 KMS 激活
- 百度云搜索 Office
- [手把手教你安装激活 Microsoft Office 365 站长亲测可用！Excel最新版 破解版](http://excel880.com/blog/archives/10432)

## shutdown

shutdown -s -t 3600

## [Windows 系统命令行设置环境变量](https://blog.csdn.net/leonliu06/article/details/78586803/)

```
# 永久设置 GIT_HOME 变量为 abc
$ setx /m GIT_HOME abc

# 将 C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin 追加到 PATH 变量
$ setx -m PATH "%PATH%;C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin"
```

## 下载加速

1. DNS 查询

[http://tool.chinaz.com/dns/](http://tool.chinaz.com/dns/)

查询对应下载网站的 TTL 最小的DNS 节点

1. 修改 host 文件,将下载路径指向该 DNS

C:\Windows\System32\drivers\etc\host

文件末尾 加上

110.53.72.104 download.visualstudio.microsoft.com

1. 刷新dns

打开cmd

ipconfig /flushdns

1. ping 一下测试速度

   ping download.visualstudio.microsoft.com

## Bat

* [windows - batch/bat to copy folder and content at once - Stack Overflow](https://stackoverflow.com/questions/5062503/batch-bat-to-copy-folder-and-content-at-once)
* [Windows Bat命令常用操作 - zhihua - 博客园](https://www.cnblogs.com/ddzzhh/p/15727681.html)
* [windows bat常见操作_smile_pbb的博客](https://blog.csdn.net/smile_pbb/article/details/120395065)
