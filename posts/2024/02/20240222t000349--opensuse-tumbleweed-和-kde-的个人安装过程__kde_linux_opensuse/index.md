# openSUSE Tumbleweed 和 KDE 的个人安装过程


## 安装之前 {#安装之前}

没什么特别好说的，我喜欢使用 [ventoy](https://www.ventoy.net/en/index.html) 制作启动 U 盘，之后将 [openSUSE 风滚草](https://get.opensuse.org/tumbleweed/) 的镜像拷贝进去就可以了。


## KDE Plasma 桌面配置 {#kde-plasma-桌面配置}

我对于 Plasma 的配置非常简单，有以下几个步骤：

1.  将任务栏移到上面，并缩小到 40
2.  设置 &gt; Appearance &gt; Icons 换一个喜欢的图标包，默认的图标包有一点丑，我选择的是 Papirus
3.  设置 &gt; Workspace Behavior &gt; Virtual Desktops 调整虚拟桌面的排列，我将行数设置为 1 行，默认 2 个虚拟桌面即可
4.  设置 &gt; Window Management &gt; Task Switcher 将任务切换效果从 Breeze 切换成 Compact，最简单的切换效果，顺便把 `` Alt + ` `` 的快捷键删除了
5.  设置 &gt; Input Devices &gt; Keyboard &gt; Advanced 勾选 Configure keyboard options 和下面的 Make Caps Lock an additional Ctrl 我们当然要将大写锁定键完全换成 Ctrl

KDE Plasma 用户在登录系统后，首次链接无线网络时会遇到 KDE 钱包的报错提醒（缺少密钥）。

为了避免重复输入验证密码。打开 KDE 钱包，选择 新建钱包 ，然选择传统，blowfish 加密文件，然后不输入密码，直接确认保存。这样你就有了一个空（无验证密码）的 KDE 钱包。联网就无需验证身份了。


## Yast 配置和软件源更改 {#yast-配置和软件源更改}

首先我们需要安装一下简体中文语言包，我们在 Yast &gt; Language &gt; Seconday Language 中选中 Simplified Chinese 就可以安装上中文语言包了。

接着我们打开 Software Repositories 把我们安装后自带的一个本地仓库（类似于 `openSUSE-20240216-0` ）删除。

我们再将官方源换成 [中科大 openSUSE 源](https://mirrors.ustc.edu.cn/help/opensuse.html) ，由于 openSUSE 使用了 MirrorBrain 技术，中央服务器 (download.opensuse.org) 会按照 IP 地理位置中转下载请求到附近的镜像服务器（但刷新软件源时仍从中央服务器获取 元数据），所以更改软件源通常只会加快刷新软件源的速度，而对下载速度影响不大。

我们再添加一下 Packman 源，直接在终端中：

```bash
sudo zypper ar -cfp 90 https://mirrors.ustc.edu.cn/packman/suse/openSUSE_Tumbleweed/ USTC:PACKMAN
```

之后我们刷新并更新风滚草即可：

```bash
sudo zypper ref && sudo zypper dup
sudo reboot
```

接着我们安装一些多媒体编码器：

```bash
sudo zypper dist-upgrade --from USTC:PACKMAN --allow-vendor-change
sudo zypper install --from USTC:PACKMAN ffmpeg gstreamer-plugins-{good,bad,ugly,libav} libavcodec-full vlc-codecs
```


## 网络代理配置 {#网络代理配置}

我们使用 v2raya 来进行网络代理，先安装 v2ray 内核和 v2raya

```bash
sudo zypper in v2ray
opi v2raya
```

打开 v2raya 服务后按照 [快速上手](https://v2raya.org/docs/prologue/quick-start/) 即可使用网络代理，使用透明代理可以为几乎所有的程序进行网络代理。

```bash
sudo systemctl enable --now v2raya.service
```

更多代理使用方法可以参考 [配置代理](https://zh.opensuse.org/SDB:%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86) 。


## 字体与中文输入法 {#字体与中文输入法}

在 KDE 中安装字体很简单，只需要把字体往 Font Management 里面拖动就行了（最好是放在 Personal Fonts 分组里面），之后你可以使用 `fc-cache -fv` 刷新字体。

比较传统的方法是放到下面的文件夹中，按照优先级排序是：

```bash
Font directories:
        /usr/X11R6/lib/X11/fonts
        /opt/kde3/share/fonts
        ~/.local/share/fonts
        ~/.local/share/flatpak/exports/share/fonts
        /var/lib/flatpak/exports/share/fonts
        /usr/local/share/fonts
        /usr/share/fonts
        ~/.fonts
        /usr/share/fonts/100dpi
        /usr/share/fonts/75dpi
        /usr/share/fonts/Type1
        /usr/share/fonts/cyrillic
        /usr/share/fonts/encodings
        /usr/share/fonts/ghostscript
        /usr/share/fonts/misc
        /usr/share/fonts/texlive-lm
        /usr/share/fonts/truetype
        /usr/share/fonts/xscreensaver
        /usr/share/fonts/encodings/large
```

推荐放在 `~/.fonts` 或是 `~/.local/share/fonts` 文件夹中，然后同样用 `fc-cache -fv` 刷新字体即可。

我个人推荐的字体是 [Sarasa Term SC Nerd](https://github.com/laishulu/Sarasa-Term-SC-Nerd) ，因为它在 Emacs 中使用可以处理表格对齐的问题，同时包含 Nerd Fonts，比较省心。

输入法推荐安装 [Fcitx5](https://zh.opensuse.org/Fcitx5) ：

```bash
sudo zypper in fcitx5 fcitx5-chinese-addons
sudo zypper in fcitx5-configtool #  安装配置工具（可选）
```

在安装 Fcitx5 的时候，会默认卸载替换掉旧的 Fcitx，我们跟随系统提示操作即可。

安装后重新登陆，我们在设置 &gt; Reginonal Settings &gt; Input Method 中点击 Add Input Method 添加 Pinyin 即可，这里的一些环境变量使用 X11 的 KDE 的话系统已经自动帮我们设置好了。

我们还可以点击 Configure addons 设置一些输入法 UI 等。

词典可以帮助改善输入法的使用体验，以下是两个受欢迎的词库：

-   [felixonmars/fcitx5-pinyin-zhwiki](https://github.com/felixonmars/fcitx5-pinyin-zhwiki)
-   [wuhgit/CustomPinyinDictionary](https://github.com/wuhgit/CustomPinyinDictionary)

请根据仓库的 README 文件将下载或下载解压后的 .dict 文件放置到 `~/.local/share/fcitx5/pinyin/dictionaries/` 文件夹中（如果文件夹不存在，你需要手动新建此文件夹），再重启一下 fcitx5 即可。


## 安装软件 {#安装软件}

我将我一开始安装的一些软件列下来（我随便写了一些，可以按需下载安装）：

```bash
通过 zypper 安装：
git
emacs
opi
keepassxc
mpv
telegram-desktop
qbittorrent
filezilla
wireshark
flameshot # 安装指南 (https://zh.opensuse.org/Flameshot)
cheat
nodejs
syncthing

通过 opi 安装
megasync
google-chrome-stable
v2raya
```


## 电源管理（针对笔记本电脑） {#电源管理-针对笔记本电脑}

我们使用默认的 TLP 配置即可，你可以使用 [TLPUI](https://github.com/d4nj1/TLPUI) 来更简单地配置。

```bash
sudo zypper in tlp
sudo systemctl disable tlp && sudo systemctl stop tlp
sudo systemctl enable tlp
```

我们在 TLPUI 中简单设置一下充电阈值即可，这样可以延长一些笔记本电池的寿命

```bash
START_CHARGE_THRESH_BAT0=75
# 开始充电阈值
STOP_CHARGE_THRESH_BAT0=85
# 停止充电阈值
```

我们可以用下面这条命令检查电池状态：

```bash
sudo tlp-stat -b
```


## 终端模拟器 {#终端模拟器}

这个选择自己称手的就行了，以前我比较喜欢折腾 shell，现在懒得去配置了，就用默认的 bash 就行了，安装一个 Ohmybash 能让它好看一些，
目前我最喜欢的 shell 居然是 emacs 里面的 eshell 吧（笑）。

此外，如果我使用 zsh，我个人很讨厌 Ohmyzsh，我会换成 [zim](https://github.com/zimfw/zimfw) 这种轻量化的配置框架。

终端模拟器的话我一直在用 [kitty](https://sw.kovidgoyal.net/kitty/conf/) ，它对于颜色支持的比较好，而且还自带 kitten 插件，每次我到新机器上直接用 kitten 就能把主题啥的配置好，性能也非常快。

我最喜欢的终端字体是 [CaskaydiaMono Nerd Font](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.1.1/CascadiaMono.zip) 。

此外，新的终端模拟器 [Warp](https://www.warp.dev/blog/warp-for-linux) 在我写这篇文章的时候，已经可以在 Linux 上使用了，我在 MacOS 上一直很喜欢使用这个非常现代的终端。

终端模拟器的配置随便换个字体就行了，还有一点是我比较讨厌透明背景色的终端配置。


## 参考链接 {#参考链接}

-   [openSUSE WIKI 快速配置指南](https://zh.opensuse.org/SDB:%E5%BF%AB%E9%80%9F%E9%85%8D%E7%BD%AE%E6%8C%87%E5%8D%97)
-   [openSUSE WIKI 电池管理](https://zh.opensuse.org/SDB:%E7%94%B5%E6%B1%A0%E7%AE%A1%E7%90%86)
-   [v2raya 用户文档](https://v2raya.org/docs/prologue/introduction/)

