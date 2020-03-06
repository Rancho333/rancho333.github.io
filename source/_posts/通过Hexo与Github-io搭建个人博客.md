---
title: 通过Hexo与Github.io搭建个人博客
date: 2019-07-14 11:05:48
tags:
---

## 前言
我信奉好记性不如记下来，上学时在笔记本上记笔记，后来在csdn上写点东西(有广告，不舒服)，后来笔记都记在有道云笔记上。最后发现利用github.io可以很方便的搭建个人博客。下面记录的是Blog搭建的过程(完成blog的上传)以及源码的备份，后面的文章记录一些优化与使用技巧。

## 环境准备
```
Github账号
Linux服务器
  git
  node.js(6.9版本以上)
```
Github账号的注册在这里不做赘述，然后创建一个名为yourname.github.io的仓库（仓库名格式一定要符合）。我用的Linux是ubuntu16.04 server版(家用)与ubuntu18.04(aws云服务器,一年免费，可以用来科学上网)。
安装git  
```
sudo apt-get install git-core
```

安装node.js
```
wget -qO- https://raw.github.com/creationix/nvm/v0.33.11/install.sh | sh
安装完成后，重启终端后执行如下命令:
npm install stable
```

依赖程序安装完成之后，就可以使用npm安装Hexo.
```
npm install -g hexo-cli
```
之后进行Hexo的初始化
```
hexo init <folder>
cd <folder>
```
完成之后，指定文件夹的目录如下：  
![]</images/hexo_tree.png>  
这里面不包含pbulic文件夹，会在执行第一次`hexo g`命令后生成，并且在执行`hexo d`命令后会生成`.deploy_git`文件夹，这两个文件夹中的内容是相同的，是最终部署到github.io中的文件。  
然后执行`npm install`产生node_modules文件夹，至此，服务端的基本初始化完成。 
修改`_config.yml`配置文件如下：  
```
deploy:
  type: git 
  repository: git@github.com:yourname/yourname.github.io.git
  branch: master
```
部署到github.io即完成，执行`hexo g -d`。这时候访问：`https://yourname.github.io/`即可访问Blog。  

## Hexo常用命令  
| command | description |  
| :-----: | :---------: |  
| hexo init [folder] | 新建一个网站 |  
| hexo new `<title>` | 新建文章，如标题包含空格用引号括起来 |  
| hexo generate | 生成静态文件，简写hexo g |  
| hexo deploy | 部署网站，简写hexo d |  
| hexo clean | 清除缓存文件 |
| hexo version | 查看Hexo版本 |
| hexo --config custom.yml | 自定义配置文件路径，执行后不再使用_config.yml |  

注意，每个主题下面也有一个_config.yml文件(作用域是该主题)，主目录下的是全局配置(作用域是整个网站)。  

## 源码备份  
hexo d只是将生成的静态网页部署到github.io上，这样存放源码的服务器到期或者多台PC开发时便会产生不便，下面说明将源码部署到github.io上。  
创建README.md文件
```
echo "# shiningdan.github.io" >> README.md
```
初始化git仓库(在hexo init folder的folder目录下执行)，hexo d操作的仓库是.deployer_git.
```
git init 
git add README.md 
git commit -m "first commit"
```
和github.io建立映射  
`git remote add origin https://github.com/yourname/yourname.github.io.git ` 
master分支作为deploy的分支，创建hexo分支用来备份源码  
```
git branch hexo  
git push origin hexo  
git checkout hexo  
```
将github.io中的默认分支修改为hexo(这样下载时就是下源码)，因为user page的发布版必须位于master分支下  
后续开发在hexo分支下执行，执行`hexo g -d`生成网站并部署到github的master分支上，执行`git add、git commit、git push origin hexo`提交源码  

### 重新部署
1. 下载源码`git clone https://github.com/yourname/yourname.github.io.git`
2. 安装依赖`npm install -g hexo-cli、npm install、npm install hexo-deployer-git`,注意不需要执行`hexo init`  

**参考资料：**  
[HEXO官方文档](https://hexo.io/zh-cn/docs/)
