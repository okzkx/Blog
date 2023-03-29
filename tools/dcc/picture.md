# 图片

## draw.io

#### 个人规范

- 方形：类
- 圆角方形：对象
- 箭头：数据流传递
- 方框：所属区域

## MarkDown 制图

[MarkDown 思维导图](https://markmap.js.org/repl/)

[流程图](https://mp.weixin.qq.com/s?__biz=Mzg5OTE5MTY4Nw==&mid=2247483770&idx=1&sn=47eb659fcf86b9e3b7a15327c1b6f9b6&chksm=c0564792f721ce84c050174071d86abde69c4a8b2a3f068c4ad7b284138bf140cc26777c7fce&token=839258608&lang=zh_CN&scene=21#wechat_redirect)

```text
graph TB
    Start(开始) --> Open[打开冰箱门]
    Open --> Put[把大象放进去]
    Put[把大象放进去] --> IsFit{"冰箱小不小？"}

    IsFit -->|不小| Close[把冰箱门关上]
    Close --> End(结束)

    IsFit -->|小| Change[换个大冰箱]
    Change --> Open
```

[时序图](https://mp.weixin.qq.com/s?__biz=Mzg5OTE5MTY4Nw==&mid=2247483780&idx=1&sn=0f2598b2657406b4dfce008489367fe1&chksm=c056476cf721ce7a683d5ac7dceadbfb7d23918519376dba69c8beaf9a326972c233f5b03f13&token=2124347599&lang=zh_CN#rd)

```text
sequenceDiagram
    小程序 ->> 小程序 : wx.login()获取code
    小程序 ->> + 服务器 : wx.request()发送code
    服务器 ->> + 微信服务器 : code+appid+secret
    微信服务器 -->> - 服务器 : openid
    服务器 ->> 服务器 : 根据openid确定用户并生成token
    服务器 -->> - 小程序 : token
```

## 图片处理

[搞定抠图](https://www.gaoding.com/koutu)

[都都网](http://www.topdodo.com/meditor)

[Photopea | Online Photo Editor](https://www.photopea.com/)

## 图片压缩

[图压](https://tuya.xinxiao.tech/)

## 像素艺术

[Palette List](https://lospec.com/palette-list)

[Photopea | Online Photo Editor](https://www.photopea.com/)
