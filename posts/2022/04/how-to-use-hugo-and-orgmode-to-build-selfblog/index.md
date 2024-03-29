# 如何用 hugo 和 org-mode 搭建个人博客


## 关于本文 {#关于本文}

这篇文章主要介绍怎么用 `orgmode`, `hugo`, `github pages` 来搭建一个个人博客.

如果你不是 `emacs` 用户, 你可以不看 `org-mode` 的部分, 直接用 `markdown` 来写就
好了.

如果你怕折腾的话, 这篇文章也不适合你.


## 用到的工具和操作流程 {#用到的工具和操作流程}


### 工具 {#工具}

| 工具           | 介绍                       |
|--------------|--------------------------|
| `org-mode`     | 一个强大的文本语言, 主要在 `emacs` 上使用 |
| `ox-hugo`      | 一个 `emacs` 的插件        |
| `hugo`         | 开源的静态站点生成器, 用于构建个人博客 |
| `LoveIt`       | hugo 的一个主题            |
| `github pages` | 我们可以把博客托管到这上面来 |
| `git`          | 用来发布/托管博客          |


### 操作流程 {#操作流程}

1.  在 `emacs` 上安装 [ox-hugo](https://github.com/kaushalmodi/ox-hugo), 并做适当配置
2.  安装 [hugo](https://gohugo.io/getting-started/quick-start/), 初始化, 创建一个新的网站
3.  安装 `hugo` 主题 [LoveIt](https://github.com/dillonzq/LoveIt), 做些配置
4.  创建你的文章并用 `ox-hugo` 转换成 `markdown` 格式
5.  在本地预览你的网站
6.  使用 `git` 托管你的网站到 `github pages`


## 第一步 在 `emacs` 上安装 `ox-hugo` {#第一步-在-emacs-上安装-ox-hugo}

> 如果你使用 `Markdown` 写博客, 可以跳过这第一步的内容.


### 关于 [ox-hugo](https://github.com/kaushalmodi/ox-hugo) {#关于-ox-hugo}

`ox-hugo` 是个 `emacs` 插件, 他将 `org` 内容转换成 `hugo` 兼容的 `Markdown` 格式,
并将一些 `org` 头部文件转换成 `hugo` 识别的前置参数.


### 安装 `ox-hugo` {#安装-ox-hugo}


#### 普通安装 {#普通安装}

你可以在你的配置中加上如下代码

```elisp
(with-eval-after-load 'ox
  (require 'ox-hugo))
```


#### 通过使用 [use-package](https://github.com/jwiegley/use-package) 来安装 {#通过使用-use-package-来安装}

将下面这段代码放到你的配置中

```elisp
(use-package ox-hugo
  :ensure t   ;Auto-install the package from Melpa
  :pin melpa  ;`package-archives' should already have ("melpa" . "https://melpa.org/packages/")
  :after ox)
```


#### 在 [doom-emacs](https://github.com/hlissner/doom-emacs) 中安装 {#在-doom-emacs-中安装}

在 `doom` 配置文件 `init.el` 中, 取消 `org` 一行的注释, 在下面添加 `hugo` 支持即
可. 或者在 `package.el` 配置文件中写下

```elisp
(package! ox-hugo)
```

详细内容可以参考 [doom-emacs 文档中关于安装插件的这部分](https://github.com/hlissner/doom-emacs/blob/master/docs/getting_started.org#modules)


### `ox-hugo` 的使用与配置 {#ox-hugo-的使用与配置}


#### 使用 `ox-hugo` {#使用-ox-hugo}

如果你对 `emacs` 很熟悉的话, 想必安装不难. 之后在写一篇 `org` 文章的时候按快捷键
`C-c+C-e` 就会呼出交互式的导出选项了, 你也可以 `M-x org-export-dispatch`, 导出选项中有一
项是 `Export to Hugo-compatible Markdown`, 就是我们要用的.

{{< figure src="/attchments/001.png" >}}

这时候, 如果我们按 `H` 再按 `h` 即可将 `org` 文件导出为 `Markdown` 文件了.


#### 配置 `ox-hugo` {#配置-ox-hugo}

基本的功能无须配置即可使用, 更多个性化配置详见 [ox-hugo](https://ox-hugo.scripter.co/)


## 第二步 安装 `hugo`, 并做初始化 {#第二步-安装-hugo-并做初始化}


### 安装 `hugo` {#安装-hugo}

我这里以 `Mac` 的 [Homebrew](https://brew.sh/) 为例, 其他设备参考 [如何安装 hugo](https://gohugo.io/getting-started/installing/)

```bash
brew install hugo
```

之后验证下是否安装成功

```bash
hugo version
```

> 如果你不理解上面这些代码的意义的话, 你就不应该浪费时间继续看这篇文章了
>
> 如果你安装失败的话, 大概率是网络问题, 你需要想办法解决.


### 初始化 `hugo` (创建一个新的网站) {#初始化-hugo--创建一个新的网站}

`Hugo` 提供了一个 `new` 命令来创建一个新的网站, 切换到家目录(你也可以自己选择) 后, 执行下面代码

```bash
hugo new site blog
cd blog
```

这里我们在家目录新建了一个 `blog` 文件夹, 这个文件夹就是我们的博客的全部内容.

初始化目录结构如下

```bash
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


## 第三步 安装一个自己喜欢的 `hugo` 主题(以 `LoveIt` 为例) {#第三步-安装一个自己喜欢的-hugo-主题--以-loveit-为例}

你可以选择一个自己喜欢的 `hugo` 主题安装, 见 [hugo 主题列表](https://themes.gohugo.io/)

这里我以 [LoveIt](https://github.com/dillonzq/LoveIt) 这款主题为例, 对于我这种新手, 它有很详实的文档和很棒的中文支持.


### 安装主题 `LoveIt` {#安装主题-loveit}


#### 手动安装 {#手动安装}

`LoveIt` 的主题仓库是: <https://github.com/dillonzq/LoveIt>

下载 [最新的 .zip 文件](https://github.com/dillonzq/LoveIt/releases) 并解压放到 `themes` 目录下.


#### git clone {#git-clone}

```bash
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```


#### 初始化你的项目目录为 git 仓库, 并且把主题仓库作为网站目录的子模块 {#初始化你的项目目录为-git-仓库-并且把主题仓库作为网站目录的子模块}

> 这种方法我比较推荐, 关于 git 子模块的介绍见 [git 子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

```bash
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```


### `LoveIt` 的配置 {#loveit-的配置}

这里我们直接修改 `config.toml` 这个文件, 它决定了网站的默认配置.

```toml
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
theme = "LoveIt"

[params]
  # LoveIt 主题版本
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

关于更详细的完整配置可以查看 `LoveIt` 的配置文档 [完整配置](https://hugoloveit.com/zh-cn/theme-documentation-basics/#3-%E9%85%8D%E7%BD%AE)


## 第四步 创建一篇你的文章 {#第四步-创建一篇你的文章}


### 使用 `ox-hugo` 插件, 将你写的 `org` 文档转换成 `Markdown` 文档 {#使用-ox-hugo-插件-将你写的-org-文档转换成-markdown-文档}

> 如果你使用 `Markdown` 写作可以跳过这部分

我们先在博客目录下建立一个文件夹来存放我们的 `org` 文件

```bash
cd blog && mkdir org
```

切换到 `./org` 目录下, 新建 `org` 文件, 如 `我的第一篇博客.org`. 这里我给出一个
`org` 文件的模板示例.

```org
#+OPTIONS: author:nil ^:{}
#+HUGO_FRONT_MATTER_FORMAT: YAML
#+HUGO_BASE_DIR: ../
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


#### 头部文件说明 {#头部文件说明}

-   OPTION: 这里我们在 `config.toml` 中如果已经配置了, 就可以把作者给关掉
-   HUGO_FRONT_MATTER_FORMAT: `ox-hugo` 将 `org` 文件中的头文件转换成 `Markdown` 文件的
    头部参数时的指定格式, 这里我们指定的是 `YAML` 格式
-   HUGO_BASE_DIR: 我们博客的根目录的位置, 这里我们的位置是上一层目录 `../`
-   HUGO_SECTION: 生成 Markdown 文件的位置, 这里的路径是相对博客根目录下的 `content`
    文件夹的, 所以我们这里生成的 `Markdown` 文件就去了 `content/post/2022/04` 这个
    文件夹下
-   HUGO_CUSTOM_FRONT_MATTER: `Markdown` 文件头部参数设置, 我们这里把目录 `toc` 打
    开
-   HUGO_AUTO_SET_LASTMOD: 最后修改时间, `t` 表示自动生成
-   HUGO_TAGS/CATEGORIES: 标签和类别
-   HUGO_DRAFT: 默认情况下所有文章和页面均作为草稿创建, 我们让文章立即可以渲染

这些头部文件难道需要我每次都手动输入? 不会的啦, 我们可以使用 Emacs 的模板工具
[yasnippet](https://github.com/joaotavora/yasnippet) 快速生成. 这里我分享下我的模板

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


### 使用 `Markdown` 创建你的第一篇文章 {#使用-markdown-创建你的第一篇文章}

输入如下命令创建:

```bash
hugo new posts/我的第一篇文章.md
```

接下来我们用编辑器编辑这个文件就可以了, 也可以加上适当的头部参数, 下面是一个示范:

```md
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


## 第五步 在本地预览你的网站 {#第五步-在本地预览你的网站}

我们的文章写好了, 想看看最终渲染的效果如何, 我们可以先在本地启动网站:

```bash
hugo serve
```

如果渲染成功, 这时候命令行会返回一个预览网址, 复制到游览器打开即可预览你的文章.

一般为: <http://localhost:1313>

示意图如下:

{{< figure src="/attchments/002.png" >}}


## 第六步 将你的博客部署到 Github {#第六步-将你的博客部署到-github}


### 创建一个个人的 `Github Pages` 仓库 {#创建一个个人的-github-pages-仓库}

如下图所示, 要注意的就是仓库名要和你的 `Github` 用户名一致

{{< figure src="/attchments/003.png" >}}


### 构建博客网站 {#构建博客网站}

你准备好部署你的博客时, 运行一下命令:

```bash
hugo
```

会生成一个 `public` 目录, 其中包含你网站的所有静态内容和资源, 这个 `public` 目录
就是你要部署到 `Github` 上的目录.


### 部署到 `Github` {#部署到-github}

切换到 `public` 目录下, 我们执行一些熟悉的 `git` 操作

```bash
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

> 其实远程第一次提交的时候, 远程仓库有一个默认的 `README.md` 文件, 本地是没有的,
> 我们可以先 `git pull --rebase` 一下再提交, 但是对于第一次提交, 由于没有什么有效
> 文件, 其实强制提交 `git push -f origin main` 也是可以的

部署的过程有些繁琐, 我们可以写一个 `Shell` 脚本来简化一下我们的操作, 脚本
`deploy.sh` 如下:

```bash
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

执行脚本的话先更改一下权限再执行:

```bash
chmod 700 ./deploy.sh

# 执行脚本
./deploy.sh
```


### 在 Github 上等待 `Git Action` 完成, 访问你的博客 {#在-github-上等待-git-action-完成-访问你的博客}

是的, 你把 `public` 上传后, 需要等待 `Git Action` 完成 `Github pages` 的构建.

{{< figure src="/attchments/004.png" >}}

之后访问你的 `Github pages` 网址即可看到你的博客啦.

这个网址是 <https://your_github_name.github.io>


## 结尾 {#结尾}

可以考虑使用 DoIt 替代 LoveIt


## 参考 {#参考}

1.  [Hugo Quick Start](https://gohugo.io/getting-started/quick-start/)
2.  [Hugo 主题 LoveIt 的文档](https://hugoloveit.com/zh-cn/)
3.  [把博客从 Hexo 迁移到 Hugo - jdhao's blog](https://jdhao.github.io/2018/10/10/hexo_to_hugo/)
4.  [Github Pages + Hugo 搭建个人博客](https://zz2summer.github.io/github-pages-hugo-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
5.  [使用org-mode 和 ox-hugo 写博客](https://www.wenhui.space/docs/04-build-blog-site/org_mode_and_ox_hugo/)
6.  [用Org Mode + Hugo写博客，并通过Github Action自动部署到Github Pages](https://superbear.github.io/post/2021/11/use-org-mode-and-hugo-to-write-blog/)

