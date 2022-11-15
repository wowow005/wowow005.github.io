# mushi 的 zsh 配置 (不用 ohmyzsh 而用 zim)


## ohmyzsh or zim？ {#ohmyzsh-or-zim}

[ohmyzsh](https://ohmyz.sh/)（简称omz） 是一个很流行的 zsh 框架，用于管理 zsh 的配置文件。

[zim](https://zimfw.sh/) 和 omz 发挥同样的作用，只是它比 omz 快很多，通过下面这张图可以看出 zim 的速度优势。

{{< figure src="/ox-hugo/zimspeed.svg" >}}


## 准备阶段 {#准备阶段}


### 卸载 ohmyzsh {#卸载-ohmyzsh}

如果你想卸载 omz，可以这样做：

```sh { linenos=true, linenostart=1 }
sudo chmod u+x ~/.oh-my-zsh/tools/uninstall.sh
~/.oh-my-zsh/tools/uninstall.sh
```


### 安装 zsh {#安装-zsh}

我们需要在设备上安装 zsh, macOS 自从 macOS 10.15 Catalina 开始 zsh 就成为了默认的 shell，
所以你无需更改。而一般 Linux 发行版默认使用的仍是 bash，你可以查看 `/etc/shells` 确认你使用的
Linux 发行版安装了哪些 shells。

如果没有安装的话，最好通过一些包管理器安装 zsh。

比如在 Ubuntu 上通过 apt 安装 zsh，接着将默认 Shell 切换至 zsh：

```sh { linenos=true, linenostart=1 }
chsh -s /bin/zsh
```

重新登录后，运行 `echo $SHELL` 检查一下输出是不是 `/bin/zsh`


### 安装 Nerd Fonts {#安装-nerd-fonts}

[Nerd Fonts](https://www.nerdfonts.com/) 是一个为开发人员准备的字体集合，里面的字体增添了各种实用的图标，让命令行可以更加美观。

虽然 Nerd Fonts 不是必须的，但经常用命令行的肯定会喜欢 Nerd Fonts 这种打包了许多图标的字体。

在 macOS 上安装 Nerd Fonts 很方便，你可以通过 [Homebrew](https://brew.sh/) 这个包管理器来安装：

```sh { linenos=true, linenostart=1 }
brew tap homebrew/cask-fonts &&
brew install --cask font-<FONT NAME>-nerd-font
```

在其他 Linux 发行版中，如 ArchLinux 中你可以通过 `pacman` 在 AUR 这个用户打包上传的仓库中安装 Nerd Fonts。
有些 Linux 发行版使用的 Gnome 字体管理器可能并不好用，你最好手动安装字体，一般来说我的做法如下：

先挑选自己中意的字体压缩包，在游览器中下载到本地并解压得到一个字体文件夹。
在用户目录中创建一个 `.fonts` 文件夹，把解压得到的字体文件夹放到这个文件夹下。
运行 `fc-chache -fv` 刷新字体缓存文件。

安装后在使用的 Terminal 中设置字体即可。

我用过的一些 Nerd Fonts 如下

```sh
Caskaydia Cove Nerd Font
FiraCode Nerd Font
Hack Nerd Font
JetBrainsMono Nerd Font
Meslo Nerd Font
Noto Nerd Font
Sauce Code Nerd Font
```


## 安装 zim {#安装-zim}

我们可以自动安装或者手动安装，自动安装如下：

```sh { linenos=true, linenostart=1 }
# curl
curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
# wget
wget -nv -O - https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
```

手动安装请参考：<https://github.com/zimfw/zimfw#manual-installation>


## 使用 zim {#使用-zim}

我们通过修改 `~/.zimrc` 这个文件来启动 zim 的功能。

想要修改主题就在 `~/.zimrc` 中写入 `zmodule <theme>` 即可

想要引入新的功能就在 `~/.zimrc` 中写入 `zmodule <modules>` 即可

主题列表和模组列表可以在 [Themes](https://zimfw.sh/docs/themes/) 和 [Modules](https://zimfw.sh/docs/modules/) 上查看。

可以通过 `zimfw` 这个命令执行 zim 的一些操作，我将命令帮助列在下面：

```nil
Usage: zimfw <action> [-q|-v]

Actions:
  build           Build /Users/mushi007/.zim/init.zsh and /Users/mushi007/.zim/login_init.zsh.
                  Also does check-dumpfile and compile. Use -v to also see their output.
  check-dumpfile  Does clean-dumpfile if new completion configuration needs to be dumped.
  clean           Clean all. Does both clean-compiled and clean-dumpfile.
  clean-compiled  Clean Zsh compiled files.
  clean-dumpfile  Clean completion dumpfile.
  compile         Compile Zsh files.
  help            Print this help.
  info            Print Zim and system info.
  list            List all modules currently defined in /Users/mushi007/.zimrc.
                  Use -v to also see the modules details.
  init            Same as install, but with output tailored to be used at terminal startup.
  install         Install new modules. Also does build, check-dumpfile and compile. Use -v to
                  also see their output, any on-pull output, and see skipped modules.
  uninstall       Delete unused modules. Prompts for confirmation. Use -q for quiet uninstall.
  update          Update current modules. Also does build, check-dumpfile and compile. Use -v
                  to also see their output, any on-pull output, and see skipped modules.
  upgrade         Upgrade zimfw. Also does compile. Use -v to also see its output.
  version         Print zimfw version.

Options:
  -q              Quiet (yes to prompts, and only outputs errors)
  -v              Verbose (outputs more details)
```

我们在 `.zimrc` 这个文件中写好了想要安装的插件后，就可以使用 `zimfw install` 命令安装插件。
冗余的插件可以用 `zimfw uninstall` 指令删除。

此外 `zimfw update` 和 `zimfw upgrade` 对应着升级所有插件或者升级 zim 本身。


## 卸载 zim {#卸载-zim}

手动删除 `~/.zim` 文件夹和 `~/.zimrc` 文件，再删除掉 zim 往 `~/.zshrc` 中添加的内容即可。


## 一个安装 zim 的例子 {#一个安装-zim-的例子}

我在 Fedora 中安装了 zsh，切换到 zsh 后它会自动生成一些 zsh 的启动文件如下图：

{{< figure src="/ox-hugo/zsh-startfiles.png" >}}

由于我是在 docker 中演示的，所以 `.zshrc` 文件中没什么重要的设置，我可以把 `.zshrc` 文件删除，
让 zim 给我创建一个新的 `.zshrc` 文件，如果你没有删除的话，安装后 zim 将把它的设置写入在 `.zshrc`
这个文件的前面。

你可能已经在 `.zshrc` 中写入了一些修改 Shell 环境变量的一些设置，或者是一些 Alias 设置，这些都可以保留。
其他一些修改 Shell Prompt 的设置或者是修改 History 的设置都可以移去，让 zim 去接管这些设置工作。

我使用自动安装脚本帮我安装：

```sh { linenos=true, linenostart=1 }
curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
```

安装完成后，我修改 `.zimrc` 的内容，

```sh
# Start configuration added by Zim install {{{
#
# This is not sourced during shell startup, and it's only used to configure the
# zimfw plugin manager.
#

#
# Modules
#

# Sets sane Zsh built-in environment options.
zmodule environment
# Provides handy git aliases and functions.
zmodule git
# Applies correct bindkeys for input events.
zmodule input
# Sets a custom terminal title.
zmodule termtitle
# Utility aliases and functions. Adds colour to ls, grep and less.
zmodule utility

#
# Prompt
#

# Exposes to prompts how long the last command took to execute, used by asciiship.
zmodule duration-info
# Exposes git repository status information to prompts, used by asciiship.
zmodule git-info
# Formats the current working directory to be used by prompts.
zmodule prompt-pwd

#
# Completion
#

# Additional completion definitions for Zsh.
zmodule zsh-users/zsh-completions --fpath src
# Enables and configures smart and extensive tab completion.
# completion must be sourced after all modules that add completion definitions.
zmodule completion

#
# Modules that must be initialized last
#

# Fish-like syntax highlighting for Zsh.
# zsh-users/zsh-syntax-highlighting must be sourced after completion
zmodule zsh-users/zsh-syntax-highlighting
# Fish-like history search (up arrow) for Zsh.
# zsh-users/zsh-history-substring-search must be sourced after zsh-users/zsh-syntax-highlighting
zmodule zsh-users/zsh-history-substring-search
# Fish-like autosuggestions for Zsh.
zmodule zsh-users/zsh-autosuggestions
# }}} End configuration added by Zim install

#
# Add by myself
#

# Exa
zmodule exa

# Theme
zmodule minimal
```

通过 `exec zsh` 重启 zsh，我的 zsh 就设置好了。


## 参考链接 {#参考链接}

-   [zim 文档](https://zimfw.sh/docs/)
-   [Nerd Fonts](https://www.nerdfonts.com/)

