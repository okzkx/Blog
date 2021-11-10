# 美化

## Hexo

### 基础使用

[** hexo-github**](https://github.com/hexojs/hexo)

[文档](https://hexo.io/zh-cn/docs/)

### 常用配置

[**Hexo-设置阅读全文**](https://www.jianshu.com/p/78c218f9d1e7)

[Hexo使用攻略-添加分类及标签](https://linlif.github.io/2017/05/27/Hexo%E4%BD%BF%E7%94%A8%E6%94%BB%E7%95%A5-%E6%B7%BB%E5%8A%A0%E5%88%86%E7%B1%BB%E5%8F%8A%E6%A0%87%E7%AD%BE/)

## NexT

### 基础使用

[** hexo-theme-next-github**](https://github.com/theme-next/hexo-theme-next)

[文档](http://theme-next.iissnan.com/getting-started.html)

### 常用配置

进入 \themes\next\\\_config.yml

* 打开 menu 的菜单栏中需要的菜单
* Schemes 设置主题
* auto\_excerpt 开启自动截断

### 第三方服务

#### 访问者统计 + 评论系统

[Hexo博客使用valine评论系统无效果及终极解决方案](https://www.jianshu.com/p/f4658df66a15)

使用一个 LeanCloud 应用即可实现访问者统计和评论系统，

在 Valine 可以同时配置这两个系统，不要使用 leancloud\_visitors 。

#### 搜索

[Hexo个人博客NexT主题添加Local Search本地搜索](https://blog.csdn.net/mqdxiaoxiao/article/details/93257866)

#### 桌宠

[hexo 给自己的博客添加萌宠或萌妹子](https://www.jianshu.com/p/c59a15d90759)

[妹子预览](https://huaji8.top/post/live2d-plugin-2.0/)

#### 鼠标点击特效

[Hexo优化鼠标点击效果](http://blog.duanzy.xyz/2019/01/18/HexoBetter/#more)

#### 字数统计

根据 \_config.yml 中配置即可

```
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
  totalcount: true
  separated_meta: false
```

#### 部署到 Gitee

Gitee 需要新建仓库名与个人名 Gitee 域名相同，其他与部署 Github 一致

#### 背景图片

[Hexo 添加背景图片并自适应](https://vonsdite.github.io/posts/c08e78b.html)
