---
title: Hexo使用优化
date: 2019-07-14 21:45:48
categories:
- Hexo
tags:
- Hexo
---

## 说明
```
站点配置文件：位于站点根目录下，主要包含Hexo本身的配置
主题配置文件：位于主题目录下，主要用于配置主题相关的选项
```

## 使用next主题
网上一搜大部分都是Hexo+next的使用，本着站在前人的肩膀上原则，使用next主题。  
主题下载：`git clone https://github.com/iissnan/hexo-theme-next themes/next`  
启用主题，打开站点配置文件，找到`theme`字段，修改为如下：  
```
theme: next
```

### 选择scheme
next提供4种不同外观，找到`scheme`字段，启用其中一种scheme，这里我选择Gemini.  
```
# Schemes                                  
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini
```  

### 设置语言
找到`language`字段，修改为(中文，英文为：en)：
```
language: zh-Hans
```

### 修改menu图标
还没有研究清楚，后续补上

### 设置侧栏
找到`sidebar`字段，修改如下：  
```
sidebar:
  #靠左放置
  position: left
  #显示时机
  display: always
```

### 设置头像
在主题配置文件中，找到`avatar`字段，值设置成头像的链接地址

### 设置作者昵称
在站点配置文件中，找到`author`字段，进行设置  

## 修改标签图标
可以在`https://www.iconfont.cn/`上找合适的图标，下载两个尺寸(16x16和32x32)的png格式图片，放到主题下的images文件夹中。在主题配置文件中找到：
```bash
favicon:                         
  small: /images/R-16x16.png  
  medium: /images/R-32x32.png 
```
替换small和medium两项，分别对应两种尺寸的图标。

## 给文章添加"categories"属性
创建`categories`页面  
```
hexo new page categories
```
找到`source/categories/index.md`文件，里面的初始内容如下：
```
---                                       
title: categories
date: 2019-07-12 12:14:44
---
```
添加`type: "categories"`到内容中，添加后内容如下：
```
---                                               
title: categories
date: 2019-07-12 12:14:44
type: "categories"
---
```

给文章添加`categories`属性，打开任意一篇md文件，为其添加概述信，添加后内容如下：
```
---                                              
title: Linux查找so文件所在pkg 
date: 2019-07-14 20:17:03
categories: 
- Linux相关
tags:
- Linux
- so文件
---
```
hexo一篇文章只能属于一个分类，如添加多个分类，则按照分类嵌套进行处理。

## 给文章添加"tags"属性
创建`tags`页面
```
hexo new page tags
```
后面的操作与添加`categories`属性类似，一篇文章可以添加多个`tags`  


**参考资料：**
[NexT官方](http://theme-next.iissnan.com/)  