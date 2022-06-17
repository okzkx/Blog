# cpp 工程结构

## 目录

|-- project
    |-- build   
        |--  debug
        |-- release
    |-- dist
    |-- doc
    |-- include 
        |-- module1
        |-- module2
    |-- lib
    |-- module1
    |-- module2
    |-- res
    |-- samples 
        |-- sample1
        |-- sample2
    |-- tools
    |-- COPYRIGHT
    |-- Makefile
    |-- README
    |-- bin

## 使用

- 核心目录
  - build :项目编译目录，各种编译的临时文件和最终的目标文件皆存于此，分为debug/和release/子目录
  - res :资源目录
  - tools\thirdPaty : 项目依赖
  - Makefile, CMakeLists.txt:项目构建配置文件，当然也有可能是其他类型的构建配置文件,比如bjam
  - README : 项目的总体说明文件
  - src : 源代码
- 开源项目
  - include : 给源码库使用者的头文件目录
  - lib : 静态链接库
  - doc : 保存项目各种文档
  - samples : 样例程序目录
  - copyleft:版权声明文件，当然也可以叫做copyright :-)
  - dist : 分发目录，最终发布的可执行程序和各种运行支持文件存放在此目录，打包此目录即可完成项目分发
  - bin : 可执行文件