---
title: ‍‬⁡​‍​‍‬‌⁡⁣⁣​﻿‌​‌​‍​⁢‌⁡⁤‬⁣⁣​​⁢​‬⁢‌⁣﻿⁢‍​﻿‬​​‍​​⁤​⁢【教程】本地知识库-部署Qwen2.5 Omni+Obsidian教程文档0709 - 飞书云文档
source: https://kow0q7t873.feishu.cn/docx/W3NvddJzRohfnHxcFKDcrP52nvh
category: web
author: 
published: 
created: 2025-07-14
description: 
tags:
  - clippings
---
## 飞书云文档

```dataview
list
where category = [web]
```

搜索

主页

云盘

我的空间和共享空间搬到云盘了

知识库

置顶文档

置顶知识库

我的文档库

存储管理

回收站

## 【教程】本地知识库-部署Qwen2.5 Omni+Obsidian教程文档0709

Xuan

7月10日修改

1.

下载与安装:

◦

下载对应您操作系统的安装包并完成安装。

2.

搜索并下载模型:

◦

打开 LM Studio 应用程序，点击左侧菜单的 放大镜图标 (🔍) 进入搜索页。

◦

在搜索框中输入 Qwen 2.5 7B 。

◦

在搜索结果中，选择官方发布的 qwen/qwen2.5-vl-7b 模型。

◦

点击 Download 按钮，等待下载完成。

![飞书文档 - 图片](https://api3-eeft-drive.feishu.cn/space/api/box/stream/download/v2/cover/XGaKbnZk7oUggOxQpIKcd2HHnDg/?fallback_source=1&height=1280&mount_node_token=doxcn1jtTd3jLU78yQsb9M7k0td&mount_point=docx_image&policy=equal&width=1280)

第二步：启动本地AI服务器

模型下载完成后，我们需要通过LM Studio启动一个本地服务器，以便Obsidian可以与之通信。

1.

进入服务器页面:

◦

点击LM Studio左侧菜单的 Developer (开发者) 图标 ( </> )。

2.

加载模型:

◦

在页面顶部，点击 “Select a model to load” 的下拉菜单，选择您刚刚下载的 qwen/qwen2.5-vl-7b 模型。

3.

启动服务器:

◦

等待模型加载完毕后，点击 Start Server 按钮。

◦

当您看到日志中出现 Server started 并且状态显示为绿色的 Running 时，代表服务器已成功启动。 此窗口在后续使用中必须保持运行。

第三步：在Obsidian中配置Copilot插件

现在，我们将Obsidian连接到这个正在运行的本地服务器。

1.

进入插件设置:

◦

打开 Obsidian，进入 设置 \> 社区插件 \> Copilot 的选项面板。

2.

添加自定义模型:

◦

选择 Models 选项卡。

◦

在 Chat Models (聊天模型) 下，点击 Add Custom Model (添加自定义模型)。

3.

填写模型配置:

![飞书文档 - 图片](https://api3-eeft-drive.feishu.cn/space/api/box/stream/download/v2/cover/HChgbclc3omYjdxrVFpcmneQn3f/?fallback_source=1&height=1280&mount_node_token=doxcnD6sv6wENThUQ3yVvwspLW7&mount_point=docx_image&policy=equal&width=1280)

4.

添加并使用:

◦

填写完毕后，直接点击 Add Model 按钮。

◦

回到Copilot聊天窗口，在模型列表中选择刚刚添加的 Qwen 2.5 Omni ，即可开始使用。

5.

Vision Recall插件以同样方式配置。

三、 性能优化配置

为了让模型能处理更长的对话和输出更完整的内容，可以进行以下优化。

优化一：提升上下文长度 (影响输入)

让AI 一次能“记住”多长 的对话历史。

1.

在 LM Studio 中，进入 我的模型 (My Models) 页面。

2.

点击 qwen/qwen2.5-vl-7b 模型，右侧会弹出默认参数设置。

Obsidian-1.8.9.exe

254.39MB

Obsidian-1.8.9.dmg

181.50MB

plugins.zip

11.56MB

评论（0）

跳转至首条评论

- 👻 Xuan酱的AI知识库
当前文档通知