# denote 使用方法


## 关于 {#关于}

自己随便记录的关于 denote 的一些使用方法，绝大部分都是直接翻译的官方文档。


## 参考文档 {#参考文档}

[denote 文档](https://protesilaos.com/emacs/denote)


## 标准笔记创建 {#标准笔记创建}

提示用户输入以创建笔记的命令包括但不限于：

-   `denote`
-   `denote-signature`
-   `denote-type`
-   `denote-date`
-   `denote-subdirectory`
-   `denote-rename-file`
-   `denote-dired-rename-files`


### The denote-templates 选项 {#the-denote-templates-选项}

用户可以以模版开始创建一个新的笔记。

创建模版的配置：

```elisp
(setq denote-templates
      '((report . "* Some heading\n\n* Another heading")
        (memo . "* Some heading\n\n* Another heading)))
```

更好的方法是用 `concat` 函数来将字符串连接起来：

```emacs-lisp
(setq denote-templates
      `((report . "* Some heading\n\n* Another heading")
        (memo . ,(concat "* Some heading"
                         "\n\n"
                         "* Another heading"
                         "\n\n"))))
```


### 维护单独的笔记目录 silos {#维护单独的笔记目录-silos}

`silos` 这个概念就是笔记存放在不同地方，目录与目录之间是分离的，我们把这样的目录叫做 `silos` 。

要创建一个 `silos` ，首先我们需要在笔记目录下创建一个 `.dir-locals.el` 文件，在创建的时候 Emacs 会提醒我们是否存储这个路径变量，我们选择是即可。

我们向 `.dir-locals.el` 文件中写入：

```emacs-lisp
;;; Directory Local Variables.  For more information evaluate:
;;;
;;;     (info "(emacs) Directory Variables")

((nil . ((denote-directory . "/path/to/silo/")
         (denote-infer-keywords . nil))))
```

下面是我的笔记目录（ `01Index` 是我的 `denote-directory` ）：

```bash
./01Index/
├── .DS_Store
├── 20240228T160157--mastering-eshell__emacs_linux_shell.assets
├── 20240228T160157--mastering-eshell__emacs_linux_shell.org
├── 20240315T225352--c语言程序设计现代方法__c.org
├── 20240317T201104--cpp-primer__cpp.org
├── 20240407T230222--西方哲学史__reading.org
└── ltximg
./02Area/
├── .dir-locals.el
├── 20240201T021341--emacs-manual-note__emacs.assets
├── 20240201T021341--emacs-manual-note__emacs.org
├── 20240302T153745--计算机网络原理__network.assets
├── 20240302T153745--计算机网络原理__network.org
├── 20240320T175109--操作系统__operatingsystem.assets
├── 20240320T175109--操作系统__operatingsystem.org
├── 20240328T141613--计算机系统结构__computerarchitecture.assets
└── 20240328T141613--计算机系统结构__computerarchitecture.org
./03Resource/
├── .dir-locals.el
├── 20240221T162109--kde-plasma-桌面-korganizer-等软件关闭后仍然占用约-800-m-内存__bug_kde.assets
├── 20240221T162109--kde-plasma-桌面-korganizer-等软件关闭后仍然占用约-800-m-内存__bug_kde.org
└── 20240222T000349--opensuse-tumbleweed-和-kde-的个人安装过程__kde_linux_opensuse.org
./04Project/
├── .dir-locals.el
└── 20240417T220159--denote-使用方法__emacs.org
```

之后我们还要用自定义用户配置命令自动选择 `silo` ，这样我们平常使用 `denote` 命令的时候默认将笔记放到 `01Index` 中，
当我们切换到 `02Area` 时，我们这时使用 `denote` 相关命令，笔记就会自动存放到当前的 `silo` 中了。

我的用户配置如下：

```emacs-lisp
(defvar my-denote-silo-directories
    `("/home/MEGA/02Area"
      "/home/MEGA/03Resource"
      "/home/MEGA/04Project"
      ;; You don't actually need to include the `denote-directory' here
      ;; if you use the regular commands in their global context.  I am
      ;; including it for completeness.
      ,denote-directory)
    "List of file paths pointing to my Denote silos.
  This is a list of strings.")

  (defvar my-denote-commands-for-silos
    '(denote
      denote-date
      denote-subdirectory
      denote-template
      denote-type)
    "List of Denote commands to call after selecting a silo.
  This is a list of symbols that specify the note-creating
  interactive functions that Denote provides.")

  (defun my-denote-pick-silo-then-command (silo command)
    "Select SILO and run Denote COMMAND in it.
  SILO is a file path from `my-denote-silo-directories', while
  COMMAND is one among `my-denote-commands-for-silos'."
    (interactive
     (list (completing-read "Select a silo: " my-denote-silo-directories nil t)
           (intern (completing-read
                    "Run command in silo: "
                    my-denote-commands-for-silos nil t))))
    (let ((denote-directory silo))
      (call-interactively command)))
```

关于 silo 的实现并不优雅，这是一个比较新的功能。我们现在只能凑合用了，我觉得作者应该将 silo 这种分散仓库概念集合到对于 `denote-directory` 的配置中。

因为毕竟我不可能总是将笔记放在一个文件夹中，总是用 `denote-subdirectory` 也不是办法。


## 笔记文件命名规范 {#笔记文件命名规范}

笔记被存放在 `denote-directory` 中，默认是 `~/Documents/notes` 。

默认遵循下面这个命名规范：

```text
DATE==SIGNATURE--TITLE__KEYWORDS.EXTENSION
```

`DATE` 字段以 年-月-日 的格式表示日期，后跟大写字母 T 表示当前时间，同时 `DATE` 也作为每个笔记文件的唯一标识符。

`SIGNATURE` 字段中包含一串字母数字字符。签名没有明确定义的用途，由用户定义。一个用例是使用它们在文件之间建立顺序关系（例如 1、1a、1b、1b1、1b2 等）。

`TITEL` 字段默认小写，并且带有连字符。

`KEYWORDS` 字段由一个或多个条目组成，这些条目用下划线分隔，输入 `KEYWORDS` 时用逗号分隔。

Examples:

```text
20220610T043241--initial-thoughts-on-the-zettelkasten-method__notetaking.org
20220610T062201--define-custom-org-hyperlink-type__denote_emacs_package.md
20220610T162327--on-hierarchy-and-taxis__notetaking_philosophy.txt
```

`denote-prompts` 可以按照如下方式来配置：

```text
DATE.EXT
DATE--TITLE.EXT
DATE__KEYWORDS.EXT
DATE==SIGNATURE.EXT
DATE==SIGNATURE--TITLE.EXT
DATE==SIGNATURE--TITLE__KEYWORDS.EXT
DATE==SIGNATURE__KEYWORDS.EXT
```


## 链接笔记 {#链接笔记}

`denote` 提供了不同的命令在笔记之间创建链接。

所有的被链接的文件都必须是 denote 文件，无法识别其他文件。


### 添加一个单独的链接 {#添加一个单独的链接}

`denote-link` 命令在 minibuffer 中选择一个文件插入到当前文件中作为链接。链接的格式取决于当前的文件是 Org 还是 Markdown。

我们也可以再 Org 文件中支持链接到标题，使用 `denote-org-store-link-to-heading` 命令，其他文件格式不支持。

当使用前缀键（默认是 `C-u` ）调用 `denote-link` ，无论文件类型如何，他都会设置链接为 `[[denote:IDENTIFIER]]` 的格式。

默认情况下，链接的描述是这样确定的：


### denote-org-store-link-to-heading 用户选项 {#denote-org-store-link-to-heading-用户选项}

选项 `denote-org-store-link-to-heading` 确定 `org-store-link` 是否链接到当前 Org 文件的标题。

`org-store` 就是把当前的链接存储起来，之后用 `org-insert-link` 插入，而 `denote-org-store-link-to-heading` 选项会影响到这个命令。

当该选项为非 `nil` 时， `org-store-link` 就是将当前标题的链接存储起来，而不是存储 `denote` 文件的链接。

如果标题没有 `CUSTOM_ID` ，则创建 `PROPERTIES` 它并将其包含在标题的抽屉中。如果存在 `CUSTOM_ID` ， `org-store-link` 按原样使用。


### 插入执行 org 文件中标题的链接 {#插入执行-org-文件中标题的链接}

作为 `denote-org-extras.el` 拓展的一部分， `denote-org-extras-link-to-heading` 会提供执行 Org 文件标题的链接。

链接形式如下：

```text
[[denote:IDENTIFIER::#ORG-HEADING-CUSTOM-ID]][Description::Heading text]]
```


### 插入一个符合正则表达式的链接 {#插入一个符合正则表达式的链接}

命令 `denote-add-links` 添加数个链接，链接匹配正则表达式。这些链接作为列表排版插入，例如：

```text
- link1
- link2
- link3
```

可以使用的正则表达式示例：

-   `journal` 匹配所有包含 `journal` 标题名字的笔记。
-   `_journal` 匹配所有关键字为 `journal` 的笔记。
-   `^2022.*_journal` 匹配所有在 `2022` 开始的笔记，并且含有关键字 `journal` 。
-   `\.txt` 匹配拓展名为 `txt` 的文件。


### 链接到一个已存在的笔记或者创建一个新的笔记 {#链接到一个已存在的笔记或者创建一个新的笔记}

在一个人的笔记工作流程中，他们可能在阐述某个主题时，但他们想要连接到另一个主题时。 Denote 提供了几个方便的命令：

-   `denote-link-after-creating`

    在后台创建一个新笔记并且直接链接到这里。

-   `denote-link-after-creating-with-command`

    这个命令和上面一个类似，只是提示输入创建笔记的命令。

-   `denote-link-or-create`

    使用 `denote-link` 在目标文件，必须时创建它。


### 反向链接的 buffer {#反向链接的-buffer}

`denote-backlinks` 会生成一个自定义的缓冲区，其中显示指向当前笔记的反向链接。 “反向链接”是返回当前条目的链接。

默认情况下，反向链接的 buffer 设计为显示链接到当前条目的注释的文件名。每个文件名都显示在自己的行上。

```text
Backlinks to "On being honest" (20220614T130812)
------------------------------------------------

20220614T145606--let-this-glance-become-a-stare__journal.txt
20220616T182958--feeling-butterflies-in-your-stomach__journal.txtx
```

反向链接缓冲区使用的是 Emacs 内置的 Xref 基础设施。在某些操作系统上，用户可能需要将某些可执行文件添加到相关的环境变量中。

