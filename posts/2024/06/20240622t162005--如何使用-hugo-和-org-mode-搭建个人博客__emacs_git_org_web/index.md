# 如何使用 hugo 和 org-mode 搭建个人博客


## 关于本文 {#关于本文}

这篇文章介绍使用 `hugo` 和 `github pages` 来搭建一个在线的个人静态博客。

另外，本文也会介绍一个叫做 `ox-hugo` 的插件，可以将 `org-mode` 文件转换成 `markdown` 文件，这样我们就能用 `org-mode` 来写博客了。

本文使用的 `Hugo` 主题是 [DoIt](https://github.com/HEIGE-PCloud/DoIt)


## 1. 在 emacs 上安装 ox-hugo {#1-dot-在-emacs-上安装-ox-hugo}


### 关于 [ox-hugo](https://github.com/kaushalmodi/ox-hugo) {#关于-ox-hugo}

ox-hugo 是个 emacs 插件, 他将 org 内容转换成 Hugo 兼容的 Markdown 格式, 并将一些 org 头部文件转换成 Hugo 识别的前置参数.


### 安装 ox-hugo {#安装-ox-hugo}


#### 普通安装 {#普通安装}

```emacs-lisp
(with-eval-after-load 'ox

  (require 'ox-hugo))
```


#### 通过 `use-package` 安装 {#通过-use-package-安装}

```emacs-lisp
(use-package ox-hugo

  :ensure t   ;Auto-install the package from Melpa

  :pin melpa  ;`package-archives' should already have ("melpa" . "https://melpa.org/packages/")

  :after ox)
```


#### 在 doom emacs 中安装 {#在-doom-emacs-中安装}

```emacs-lisp
(package! ox-hugo)
```


### 如何使用 ox-hugo {#如何使用-ox-hugo}

在写完一篇 `org` 文章之后按快捷键 `C-c C-e` 就会呼出交互式的导出选项了，导出选项中的 `Export to Hugo-compatible Markdown` 就是我们要使用的。

{{< figure src="/ox-hugo/img_20240622_163356.png" caption="<span class=\"figure-number\">&#22270;1&nbsp; </span>org-export-dispatch" width="800" >}}

这时候我们按 `H` 再按 `h` 就可以了。


## 2. 安装 hugo 并做初始化 {#2-dot-安装-hugo-并做初始化}


### 安装 Hugo {#安装-hugo}

```shell
brew install hugo
hugo version
```


### 使用 Hugo 创建一个新的网站 {#使用-hugo-创建一个新的网站}

```shell
hugo new site blog
cd blog
```

这里我们在家目录新建了一个 blog 文件夹, 这个文件夹就是我们的博客的全部内容.

初始化目录结构下如下：

```shell
./

├── archetypes

│   └── default.md

├── config.toml

├── content

├── data

├── layouts

├── static

└── themes
```


## 3. 选择一个自己喜欢的 Hugo 主题安装 {#3-dot-选择一个自己喜欢的-hugo-主题安装}


### 安装 DoIt {#安装-doit}

```shell
git submodule add https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```

如果以后需要升级的话：

```shell
git submodule update --remote --merge
```


### 网站配置 {#网站配置}

我们直接修改 `config.toml` 这个文件，它决定了网站的默认配置。

```shell
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "我的全新 Hugo 网站"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "DoIt"

[params]
  # DoIt 主题版本
  version = "0.2.X"

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
```

详细配置可以查看 `DoIt` 的配置文档 [基本配置文档](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)


## 4. 创建一篇博客文章 {#4-dot-创建一篇博客文章}


### 使用 Org-mode 写作 {#使用-org-mode-写作}

`org` 文件的开头应该像下面这样：

```org
#+OPTIONS: author:nil ^:{}
#+HUGO_FRONT_MATTER_FORMAT: YAML
#+HUGO_BASE_DIR: /Users/vicar/blog
#+HUGO_SECTION: posts/2022/04
#+DATE: [2022-04-24 Sun 20:29]
#+HUGO_CUSTOM_FRONT_MATTER: :toc true
#+HUGO_AUTO_SET_LASTMOD: t
#+HUGO_TAGS: 标签1 标签2
#+HUGO_CATEGORIES: 类别
#+HUGO_DRAFT: false
#+TITLE: 我的第一篇博客


This is my first blog!
```


#### 头部参数说明 {#头部参数说明}

-   OPTION: 这里我们在 config.toml 中如果已经配置了, 就可以把作者给关掉
-   HUGO_FRONT_MATTER_FORMAT: ox-hugo 将 org 文件中的头文件转换成 Markdown 文件的 头部参数时的指定格式, 这里我们指定的是 YAML 格式
-   HUGO_BASE_DIR: 我们博客的根目录的位置, 这里我们的位置是上一层目录 ../
-   HUGO_SECTION: 生成 Markdown 文件的位置, 这里的路径是相对博客根目录下的 content 文件夹的, 所以我们这里生成的 Markdown 文件就去了 content/post/2022/04 这个 文件夹下
-   HUGO_CUSTOM_FRONT_MATTER: Markdown 文件头部参数设置, 我们这里把目录 toc 打 开
-   HUGO_AUTO_SET_LASTMOD: 最后修改时间, t 表示自动生成
-   HUGO_TAGS/CATEGORIES: 标签和类别
-   HUGO_DRAFT: 默认情况下所有文章和页面均作为草稿创建, 我们让文章立即可以渲染

我们可以使用模版工具 `yasnippet` 快速生成这个开头。

```org
# -*- mode: snippet -*-
# name: hugo_blog
# key: <hugo
# --
#+OPTIONS: author:nil ^:{}
#+hugo_front_matter_format: yaml
#+HUGO_BASE_DIR: ../
#+HUGO_SECTION: posts/`(format-time-string "%Y/%m")`
#+DATE: `(format-time-string "[%Y-%m-%d %a %H:%M]")`
#+HUGO_CUSTOM_FRONT_MATTER: :toc true
#+HUGO_AUTO_SET_LASTMOD: t
#+HUGO_TAGS: $1
#+HUGO_CATEGORIES: $2
#+HUGO_DRAFT: false
#+TITLE: $3

$0
```

上述头部参数中的有些选项根据实际情况可能有增减改动，接着我们使用 `ox-hugo` 将 `Org` 文件转换成 `Markdown` 即可。


### 使用 Markdown 写作 {#使用-markdown-写作}

使用如下命令创建一个 `md` 文件

```shell
hugo new posts/我的第一篇文章.md
```

接下来我们用编辑器编辑这个文件就可以了, 也可以加上适当的头部参数, 下面是一个示范:

```markdown
---
title: "我的第一篇博客"
date: 2022-04-23T02:24:00+08:00
lastmod: 2022-04-24T21:00:18+08:00
tags: ["标签1", "标签2"]
categories: ["类别"]
draft: false
toc: true
---

This is my first blog!
```


## 5. 在本地预览你的网站 {#5-dot-在本地预览你的网站}

文章写好之后，想看看最终渲染效果如何，我们可以先再本地启动该网站：

```shell
hugo server
```

如果渲染成功, 这时候命令行会返回一个预览网址, 复制到游览器打开即可预览你的文章.

一般为：[http://localhost:1313/](http://localhost:1313/)

示意图如下：

{{< figure src="/ox-hugo/img_20240622_170419.png" caption="<span class=\"figure-number\">&#22270;2&nbsp; </span>博客示意图" width="800" >}}


## 6. 将你的博客部署到 Github {#6-dot-将你的博客部署到-github}


### 创建一个个人的 Github Pages 仓库 {#创建一个个人的-github-pages-仓库}

如下图所示, 要注意的就是仓库名要和你的 Github 用户名一致

{{< figure src="/ox-hugo/img_20240622_170543.png" caption="<span class=\"figure-number\">&#22270;3&nbsp; </span>Github Pages 仓库" width="800" >}}


### 构建博客网站 {#构建博客网站}

当你准备好部署你的博客时，运行：

```shell
hugo
```

会生成一个 public 目录, 其中包含你网站的所有静态内容和资源, 这个 public 目录 就是你要部署到 Github 上的目录.


### 将 public 目录上传到 Github Pages 仓库 {#将-public-目录上传到-github-pages-仓库}

切换到 public 目录：

```shell
# 将 public 目录初始化为一个 git 本地仓库
git init

# 将所有内容添加到本地仓库
git add .

# 提交到本地仓库
git commit -m "My blog's first commit"

# 关联到个人的远程仓库
git remote add origin <https://github.com/.../>

# 推送到远程的 git 仓库
git push origin main
```

> 其实远程第一次提交的时候, 远程仓库有一个默认的 `README.md` 文件, 本地是没有的, 我们可以先 `git pull --rebase` 一下再提交, 但是对于第一次提交, 由于没有什么有效文件, 其实强制提交 `git push -f origin main` 也是可以的

每次部署的过程有些繁琐，我们可以使用一个 `Shell` 脚本来帮助我们：

```shell
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# build the project
hugo

cd public

git add .

msg="rebuilding site `date`"

if [ $# -eq 1 ]
  then msg="$1"
fi

git commit -m "$msg"

# push source to github

git push upstream main

# come back to blog root

cd ..
```

执行脚本：

```shell
chmod 700 ./deploy.sh

# 执行脚本
./deploy.sh
```


### 在 Github 仓库上等待 `Git Action` 完成，访问你的博客 {#在-github-仓库上等待-git-action-完成-访问你的博客}

是的, 你把 public 上传后, 需要等待 Git Action 完成 Github pages 的构建.

之后访问你的 Github pages 网址即可看到你的博客啦.

这个网址是 <https://your_github_name.github.io>


## 参考链接 {#参考链接}

-   [Hugo Quick Start](https://gohugo.io/getting-started/quick-start/)
-   [DoIt 文档](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)
-   [把博客从 Hexo 迁移到 Hugo - jdhao's blog](https://jdhao.github.io/2018/10/10/hexo_to_hugo/)
-   [Github Pages + Hugo 搭建个人博客](https://zz2summer.github.io/github-pages-hugo-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
-   [使用org-mode 和 ox-hugo 写博客](https://www.wenhui.space/docs/04-build-blog-site/org_mode_and_ox_hugo/)
-   [用Org Mode + Hugo写博客，并通过Github Action自动部署到Github Pages](https://superbear.github.io/post/2021/11/use-org-mode-and-hugo-to-write-blog/)

