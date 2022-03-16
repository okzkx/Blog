# DirectX 12 3D 游戏开发实战

Direct3D 12 Programming Guides

## 前言

Visual Studio 2015
Github d3d12book
创建新的项目

## 必备的数学基础

### 向量代数

### 矩阵代数

### 变换

## Direct3D 基础



### 预备知识

#### 概述

通过Direct3D这种底层图形应用程序编程接口( Application Programming Interface, API), 即可在应用程序中对图形处理器( Graphics Processing Unit, GPU )进行控制和编程

render target 将像素绘制到特定的缓冲区

GPU 支持 Direct3D，应用程序控制 Direct3D

#### 组件对象模型

Component Object Model ，COM

智能释放

1. Get
2. GetAddressOf
3. Reset

#### 纹理格式

#### 交换链和页面翻转

front buffer, back buffer

presenting 呈现

swap chain 前后缓冲区交换

double buffering, triple buffering 

texture texel, pixel 纹理，纹素，像素，像素专门指颜色

#### 深度缓冲

范围 0 ~ 1

view frustum 视锥体

depth buffer 深度缓冲

z - buffer

#### 资源与描述符

draw call

bind, link

descriptor 资源描述符，对再 GPU 内资源描述的轻量级结构

- 描述符会为 GPU 解释资源、
- 告诉 GPU 资源的用途
- 资源的哪部分可以被使用
- view 和 descriptor 是同义词

descriptor heap 描述符堆

#### 多重采样技术原理

antialiasing 反走样

supersampling SSAA 超采样

resolve, downsample 降采样

MSAA 多重采样

#### 利用 Direct3D 进行多重采样

#### 功能级别

#### 功能支持的检测

#### 资源驻留

程序应当避免在短时间内于显存中交换进出相同的资源，这会引起过高的开销。最理想的情况是，所清出的资源在



### CPU 和 GPU 之间的交互

CPU 和 GPU 应道并行工作，少同步

#### 命令队列和命令列表

command queue，命令队列，环形缓冲区 ring buffer

command list 命令列表

调用ExecuteCommandLists方法才会将命令真正地送入命令队列,供GPU在合适的时机处理

#### CPU 与 GPU 之间的同步

CPU 和 GPU 使用同个资源冲突，需要

fence point 围栏点

flushing the command queue 刷新命令队列，cpu 强制等待 gpu 执行完所有命令队列内的命令

#### 资源转换

resource hard 资源冒险，

特定类型的资源才能执行特定类型的操作，

transition resource barrier 转换资源屏障

### 初始化 Direct3D

### 计时和动画

### 应用程序框架示例

### 调试 Direct3D 应用程序

### 小结

