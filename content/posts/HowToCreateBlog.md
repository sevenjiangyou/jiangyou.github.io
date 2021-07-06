---
title: "使用Hugo创建个人博客"
date: 2021-07-04T21:04:32+08:00
lastmod: 2021-07-04T21:04:32+08:00
draft: false
keywords: []
description: ""
tags: ["Hugo","firebase"]
categories: ["Hugo"]
author: "Seven"


# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: false
hiddenFromHomePage: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: true
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
mathjaxEnableAutoNumber: false

# You unlisted posts you might want not want the header or footer to show
hideHeaderAndFooter: false

# You can enable or disable out-of-date content warning for individual post.
# Comment this out to use the global config.
#enableOutdatedInfoWarning: false

flowchartDiagrams:
  enable: false
  options: ""

sequenceDiagrams: 
  enable: false
  options: ""
---

## Hugo的安装

不同版本下载地址: https://github.com/gohugoio/hugo/releases

1.**解压后的目录即是安装目录**，根据需求设定解压位置。
2.建议将安装目录添加到系统环境变量中以方便日后调用。

安装结束后，在 **cmd** 中运行如下命令检查是否安装成功。

```
# 进入安装目录，若添加到系统环境变量中则不需要进入安装文件夹中
cd /install dir path/ 
# 检查是否安装成功
hugo version
```

## 3.创建 Hugo 网站

```
# 进入想要存放网站文件夹的目录
cd /save dir path/ 
# 新建一个 Hugo 网站（blog-name是名字）
hugo new site blog-name
```

## 4.修改主题

在 **blog-name** 中新建一个 **themes** 的文件夹用于存储主题。
Hugo Themes: [https://themes.gohugo.io](https://themes.gohugo.io/)

　　在上面链接中选择合适的主题，点击后进入主题介绍界面，其中包含安装说明，以**Mainroad**为例，分为以下两种情况：

### 4.1 已安装Git

使用 `git` 下载主题，主题说明界面会看到如下安装说明。

```
# 进入themes目录中
cd .\yang-blog\themes
# 下载 mainroad 主题
git clone https://github.com/vimux/mainroad
```

#### 下载主题

**注意，执行命令时你应该在博客根目录下！**

首先我们下载主题到theme目录下

```
git clone https://github.com/olOwOlo/hugo-theme-even themes/even 
```



#### 使用其配置文件，把config.toml复制到根目录

```
mv themes/even/exampleSite/config.toml . 
```



### 编写文章

首先创建post文件夹，它将保存我们所有文章

```
mkdir -p content/post 
```

`mkdir -p content/post `

在post文件夹下编写我们的第一篇文章，名称为`demo.md`

```
hugo new post/demo.md 
```

查看此文件我们可以发现，Hugo会自动在文件上加上一些信息．**此信息很重要，确保了它是否能正确渲染，请不要乱删！**

```
---
title: "Demo" 
date: 2021-03-01T11:29:55+08:00 
draft: true 
--- 
```

除了默认的参数，Hugo还提供了其它选项供我们使用

- author 表示作者
- categories 表示类别
- tags 表示所属标签
- draft 表示是否为草稿，可删

举例来说，下面是我的一篇文章的头信息

```
--- title: "Hugo & firebase" author : "Jun"date: 2021-03-01T11:06:32+08:00categories : ["hugo"]tags : ["hugo","firebase"]--- 
```



### 测试运行

```
hugo serve
```

首先，Hugo会创建一个public目录，其中也就是它渲染的静态网页． 此后，Hugo会在本地开启一个服务，默认是`http://localhost:1313/`，浏览器访问这个链接即可，每次正式发布文章时可以首先在本地运行下．防止出错．

## Firebase的使用

[Firebase](https://firebase.google.com/)是谷歌开发的一个用于创建移动和网络应用的平台，通过它我们可以轻易地构建服务而不需要管后端的架构．

我们本次博客便部署在Firebase上面．为什么不使用Github Pages呢？我认为Firebase至少有以下几个优点：

- 快．Firebase使用了谷歌云的CDN，相比与Github Pages国内缓慢的访问速度，我这里实测网站延迟不到90ms
- 隐私．我们使用Github Pages需要新建一个公共的仓库，任何人都可以查看，而Firebase则可以直接上传后端
- 廉价．Firebase本身提供了免费计划，对于个人站点绰绰有余，如果不够，官方也提供弹性收费，即用即付

### 注册帐号

由于Firebase为谷歌旗下产品，所以可以直接用谷歌帐号登录即可．接着我们创建一个新项目。

注意，创建过程中页面会默认生成一个 Project ID，我们也可以自己填写，它将决定我们的网站托管在 firebase 上的子域名，而且创建后就不能再修改。

### 命令行工具

Firebase提供了基于`nodejs`的命令行工具，我们将通过它使用Firebase的功能

#### 安装

```
npm install firebase-tools或者npm install -g firebase-tools（这将安装全局可用的firebase命令，不然下面firebase命令要加npx）
```

这里我们使用了[npm](https://en.wikipedia.org/wiki/Npm_(software))，需要先安装nodejs，如果你没有安装，请看[这里](https://nodejs.org/en/download/package-manager/)

### npm 下载速度慢

npm 默认使用的是国外的服务器，在国内推荐使用淘宝的 npm 镜像，使用起来非常简单，只需要在安装命令后加上`registry` 参数就行了：

```
npm install -g firebase-tools --registry=https://registry.npm.taobao.org
```

#### Firebase授权

我们安装好了命令行工具后需要授权认证，这也好理解，不然Firebase怎么知道你是你呢？

```
firebase login
```

如果提示命令没有找到，请执行

```
npx firebase login
```

此后会打开一个链接，按照提示操作即可

**如果显示`authorization Error`，这主要是因为你的终端没有走代理．要使用sstap全局代理**，

- 在站点的根目录执行 `firebase init` 或者`npx firebase init`，接下来会有几个选项：
  - `Are you ready to proceed?(Y/n)`,这里直接输入 `y`，然后回车
  - 接下来会跳出选项选择项目的类型，使用上下箭头选择 `Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys`，然后按空格键确定，回车进入下一个选项。
  - 下面选择 `Use an existing project` ，使用一个已经存在的项目。
  - 然后应该就可以看到你刚刚创建的项目了，选择之后会询问是否使用 `public` 文件夹，这里直接回车
  - 最后会询问 `Configure as a single-page app (rewrite all urls to /index.html)? (y/N)` 这里输入 `n`，然后回车。
- 在站点的根目录执行 `hugo` 命令构建站点。构建成功后使用 `firebase deploy` 命令将文件提交，成功后会给出网站的网站，在浏览器访问就可以看到效果了。`hugo && firebase deploy`

**经过上述操作Firebase会生成一个json文件，它应该在你的博客根目录下，也就是和config.toml同级，如果不是则说明你的操作有误，没有在博客根目录下init**

#### 部署上传项目

```
hugo && firebase deploy  //或者 hugo && npx firebase deploy 
```

此后便会上传我们的项目至Firebase，我们按照提示便可以访问

### 自定义域名

Firebase 默认会给我们分配一个二级域名，但我们可能会想使用自定义的域名，操作如下

#### 添加域名

点击[这里](https://console.firebase.google.com/project/_/hosting/main?hl=zh-cn)，根据提示添加你的域名，我的建议是先添加一个`www.example.com`，再添加一个顶级域名`example.com`，接着让顶级域名重定向到`www`上，这对我们的SEO有好处

#### 验证所有权

我们需要为我们的域名做一个TXT记录，通过它，Firebase可以验证我们对域名的所有权，再此不做详细说明，按照提示在DNS提供商添加记录即可．

#### 上线

返回域名提供商的 DNS 管理网站，创建 DNS A 记录，从而将网站指向 Firebase 托管．

对于我们来说，要分别为顶级域名和一个`www`二级域名做两条A记录

- 对于`www`二级域名，第一栏填`www`，值填FIrebase为我们分配的IP

- 对于顶级域名，第一栏不填或填`@`，值填FIrebase为我们分配的IP

  最后等待解析生效，这个时间根据DNS提供商而异．此外FIrebase还会为自动为我们分配一个[ssl证书](https://www.junz.org/post/https_ssl)，这个过程可能比较漫长，需要我们耐心等待．在生效前访问网站均会显示隐私错误，忽略即可．
