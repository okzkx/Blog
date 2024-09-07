# Github

名词解释

- issue : 关于项目的疑问
- fork : 为他人项目做分叉成为自己的项目
- 分叉 : 分叉分为对项目的分叉和对分支的分叉
- starts ; 收藏项目

#### 图片显示出现问题

**原因**

- github 的域名解析出错,

**解决思路**

- 直接修改 hosts, 帮助域名解析

**步骤**

1. 查找域名对应的 IP 地址

   [https://www.ipaddress.com/](https://www.ipaddress.com/)

   githubusercontent.com

2. 打开文件

   > C:\Windows\System32\drivers\etc\hosts

3. 在文件最后写入域名解析结果

   xxx.xxx.xxx.xxx .githubusercontent.com

#### 能使用 vpn 访问 Github 但是没法推送拉取

说明 vpn 没有代理到 git 软件, 需要配置

> git config --global http.proxy 127.0.0.1:9788 --replace-all
