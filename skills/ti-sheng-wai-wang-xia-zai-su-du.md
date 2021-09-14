# 提升外网下载速度

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

