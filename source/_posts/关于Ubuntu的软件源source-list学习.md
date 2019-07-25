---
title: 关于Ubuntu的软件源source.list学习
date: 2019-07-25 03:02:19
categories: 
- Linux相关
tags:
- Linux
- 软件源
---

## 背景说明
最近参与到公司的白盒交换机项目中，需要编译ONL和sonic，其中软件源的设置让我非常头疼，编译依赖有很大的问题。决定深入学习一下Linux软件源.主机环境为ubuntu16.04.  

## 软件库存储文件*.list  
ubuntu使用apt来管理软件包，apt将软件库存储在如下文件中:  

```
/etc/apt/sources.list
/etc/apt/sources.list.d/目录中带.list后缀的文件
```  
 
可以通过man sources.lis来查看apt的完整存储机制。  

## sources.list格式和写法  
1. 以`#`开头的行是注释行  
2. 以deb或deb-src开头的是`apt respository`，具体格式为：  
  1. deb：二进制包仓库  
  2. deb-src：二进制包的源码库  
  3. URI: 库所在的地址，可以是网络地址，也可以是本地的镜像地址  
  4. codename：ubuntu版本的代号，可以通过命令`lsb_release -a`来查看当前系统的代号  
  5. components：软件的性质(free或non-free等)  

### ubuntu系统代号
codename是ubuntu不同版本的代号
| 版本号 | 代号(codename) |
| :----: | :--: |
| 10.04 | lucid |
| 12.04 | precise |
| 14.04 | trusty |
| 14.10 | utopic |
| 16.04 | xenial |
| 18.04 | bionic |  

### deb说明
deb后面的内容有三大部分：deb URI section1 section2  
以`deb http://us.archive.ubuntu.com/ubuntu/ xenial main restricted`为例进行说明。  
URI是库所在的地址，支持http，fpt以及本地路径,访问`http://us.archive.ubuntu.com/ubuntu/`可以看到如下信息：
{% asset_img ubuntu.png ubuntu %}  
`dists`和`pool`这两个目录比较重要。`dists`目录包含了当前库的所有软件包的索引。这些索引通过codename分布在不同的文件夹中。例如`xenial`所在的目录。  
{% asset_img xenial.png xenial %}
上图中的文件夹名其实就是对应了section1，我们可以根据需要填写不同的section1.
这里面的文件都是用以下格式命名的：  

```
codename
codename-backports    #unsupported updates
codename-proposed	  #pre-released updates
codename-security	  #important security updates
codename-updates	  #recommanded updates
```  

打开其中一个任一文件夹，例如`xenial-updates`:  
{% asset-img xenial-updates.png xenial-updates %}  
里面有`main,multiverse,restricted,universe`文件夹，这些文件夹对应deb后面的section2,里面包含了不同软件包的索引。它们的区别在于：  

```
main：完全的自由软件
restricted: 不完全的自由软件
universe: ubuntu官方不提供支持与补丁，全靠社区支持
multiverse: 非自由软件，完全不提供支持和补丁
```  

打开main目录下的binary-i386子目录下的Packages.gz文件，可以看到如下内容：  
{% asset_img packages.png packages %}
说明：Packages.gz这个文件其实就是一个“索引”文件,里面记录了各种包的包名(Package)、运行平台(Architecture)、版本号（Version）、依赖关系(Depends)、deb包地址(Filename)等。Filename指向的是源服务器pool目录下的某个deb。猜测：`apt-get install`某个软件是，其实就是基于这些Packages.gz来计算依赖关系，然后根据其中的filename地址来下载所需的deb，最后执行`dpkg -i pacckage.deb`来完成软件包的安装。  

## 替换源
先将默认的sources.list进行备份，然后仿照下表修改源：  

```
\# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
\# deb-src https://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
\# deb-src https://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
\# deb-src https://mirrors.aliyun.com/ bionic-backports main restricted universe multiverse
deb https://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
\# deb-src https://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

\# 预发布软件源，不建议启用
\# deb https://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
\# deb-src https://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```

推荐使用阿里云的源，延迟低，异常少！  

