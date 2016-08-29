---
layout: post
title:  "详细记录使用jekyll/GitHub/Coding搭建个人博客全过程"
date:   2016-08-18 19:00:00
categories: jekyll
tags: jekyll
excerpt: 第一篇文章，记录下自己本博的搭建全过程，仅供参考！
---

* content
{:toc}

这是这个博客的第一篇文字，就记录一下本博的搭建过程。

## 基本介绍

本博客系统使用了Jekyll，代码托管在page.github.com和coding.me，域名使用DNS智能解析，默认解析到github，国内IP解析到coding，国内外访问都会比较快。

### Jekyll是什么
> Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。

### 特点
+ 写作方便：使用markdown语法、本地写作，写完后提交git并push到origin即可；
+ 使用git管理文章，完全不用担心数据丢失；
+ 免费可靠：代码托管在github和coding两大平台，不用自己租用服务器，利用两大平台的CDN达到国内外都快速访问的效果；
+ 简单：搭建和维护都很简单，专注于文章写作

下面介绍具体搭建过程。

## 准备工作
+ github.com账号
+ coding.net账号（github和coding二者至少需要一个）
+ node.js运行环境
+ ruby环境
+ git客户端
+ markdown编辑工具
+ 域名，含DNS解析

> 说明：准备工作一般都是非常简单的操作，这里就不详细介绍了。
> 本博客一般也不会完整介绍怎么注册账号、安装windows程序，无论中文还是英文，请自行Google。

## 方法步骤

### 本地搭建Jekyll

* 本次安装在Linux下进行

1. 安装运行Jekyll

    ```shell
    
        gem install jekyll
        jekyll new myblog
        cd myblog
        ~/myblog $ jekyll serve
    ```
    
    Jekyll安装完毕！

    Jekyll服务默认端口是4000，打开浏览器，输入：http://localhost:4000 即可访问

2. 使用Jekyll写文章
    
    Jekyll的基本目录结构如下：
    
    ```
    .
    ├ about.md
    ├ _config.yml
    ├ css
    │   └ main.scss
    ├ feed.xml
    ├ _includes
    │   ├ footer.html
    │   ├ header.html
    │   ├ head.html
    │   ├ icon-github.html
    │   ├ icon-github.svg
    │   ├ icon-twitter.html
    │   └ icon-twitter.svg
    ├ index.html
    ├ _layouts
    │   ├ default.html
    │   ├ page.html
    │   └ post.html
    ├ _posts
    │   └ 2016-08-18-welcome-to-jekyll.markdown
    └ _sass
        ├ _layout.scss
        └ _syntax-highlighting.scss
    ```
    我们撰写的文章需要保存在`_post`目录，和众多博客程序一样，安装完毕默认已经有一篇演示文章了，我们可以使用markdown编辑器修改默认文章，保存后即可看到修改效果。
    
    发表一篇新文章，你所需要做的就是在_posts文件夹中创建一个新的文件。 文件名的命名非常重要。Jekyll 要求一篇文章的文件名遵循下面的格式：
    
    ```
    年-月-日-标题.MARKUP
    ```
    
    在这里，`年`是4位数字，`月`和`日`都是2位数字。`MARKUP`扩展名代表了这篇文章 是用什么格式写的。下面是一些合法的文件名的例子：
    
    ```
    2011-12-31-new-years-eve-is-awesome.md
    2012-09-12-how-to-write-a-blog.textile
    ```
    
    所有博客文章顶部必须有一段`YAML头信息`(YAML front- matter)。 在它下面，就可以选择你喜欢的格式来写文章。Jekyll支持2种流行的标记语言格式： `Markdown` 和 `Textile`。 这些格式都有自己的方式来标记文章中不 同类型的内容，所以你首先需要熟悉这些格式并选择一种最符合你需求的。

本文只介绍怎么搭建博客，所以具体的Jekyll使用方法请参考官方文档。

* [官方网站（英文）](https://jekyllrb.com/)
* [jekyllcn.com](http://jekyllcn.com/)
* [jekyll.bootcss.com](http://jekyll.bootcss.com/)

### 创建git仓库

1. 添加git忽略文件
    
    ```shell
    cd myblog
    touch .gitignore
    vi .gitignore
    ```
    编辑忽略文件，加入一下内容
    
    ```
    _site
    .sass-cache
    _exclude
    .idea
    .jekyll-metadata
    .vscode
    ```

2. 创建仓库并提交
    
    ```shell
    cd myblog
    git init
    git add --all #添加到暂存区	
    git commit -m "提交jekyll页面" #提交到本地仓库
    ```
    
    本地git仓库创建完毕，并提交

### 将代码部署到github

1. github 新建仓库`username.github.io`（这里的`username`是你自己的github账号，下同）；
2. 将上面创建的仓库添加为本地仓库远程，并push本地仓库到github
    
    ```shell
    git remote add origin git@github.com:hustshenl/hustshenl.github.io.git
    git push origin master
    ```
    
    稍等片刻，访问`username.github.io`就可以看到博客已经OK了；
    
    发布新文章后再次执行一下命名即可推送到github
    
    ```
    git add --all
    git commit -m "提交jekyll页面" 
    git push origin master
    ```

### 为username.github.io绑定域名

1. 到域名服务商增加你的`CNAME`记录。 添加一条记录`www`的主机记录，记录类型为`CNAME`类型，线路为`默认`。 记录值请写`username.github.io.`,值得注意的是io后面还有一个圆点，切记。
2. 访问github仓库，在`settings`>`GitHub Pages`>`Custom domain`下的输入框里输入要绑定的域名，然后点击`save`保存；

待域名解析生效后，访问你刚才绑定的域名就OK了；效果和本站类似


###  将代码部署到coding并绑定域名

1. coding 新建仓库`username`（这里的`username`是你自己的coding账号，下同）；
2. 将上面创建的仓库添加为本地仓库远程，并push本地仓库到coding
    
    ```shell
    git remote add coding git@git.coding.net:username/username.git
    git push coding master
    ```

3. 访问coding仓库，在`Page服务`选项卡下选择部署分支并点击开始部署
稍等片刻，访问`username.coding.me`就可以看到博客已经OK了；
    
    发布新文章后再次执行一下命名即可推送到coding
    
    ```
    git add --all
    git commit -m "提交jekyll页面" 
    git push coding master
    ```

4. 到域名服务商增加你的`CNAME`记录。 添加一条记录和`www`的主机记录，记录类型为`CNAME`类型，线路为`国内`。 记录值请写`username.github.io.`,值得注意的是io后面还有一个圆点，切记。
5. 访问coding仓库，在`Page服务`选项卡下要绑定的域名，然后点击`添加域名绑定`即可；
   
待域名解析生效后，访问你刚才绑定的域名就OK了。


## 注意事项

> 稍后补充


