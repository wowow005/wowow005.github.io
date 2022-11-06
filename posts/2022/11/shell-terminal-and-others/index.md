# Shell Terminal Console 词语解释


## Shell {#shell}

Shell 的中文应该叫做 “壳” 程序，区别于 “核”。它的主要工作就是解析命令 (command interpreter)。
Shell 为操作系统和用户之间传递命令，如果操作系统没有运行图形界面 Shell 会作为默认的进程运行，
这时你只能通过 Shell 操控操作系统去干事。一般来说这里的图形界面也是一个软件。

常见的 Shell 有：

-   sh
-   bash
-   csh
-   ksh
-   zsh
-   fish


## Terminal {#terminal}

Terminal 的中文应该叫做计算机 “终端”，早期的计算机终端一般使用机电的电传打字机也叫 TTY (Teletypewriter)。
电传打字机一开始是作为电报的终端使用的，后来股票电报机大规模使用。一开始计算机并不能和 tty 相连，
直到 1959 年 DEC 公司生产的 PDP-1 电脑， tty 才可以通过串口线连接计算机。tty输入靠键盘，输出靠的是
打印的纸张或纸带。

那时使用的计算机如下图所示：

{{< figure src="/ox-hugo/_20221105_162909screenshot.png" >}}

后来的终端逐渐发展为键盘和显示器。

目前来说我们已经在使用各种图形化界面的操作系统，“终端” 已经成为操作系统中安装的一个程序了，这种所谓的 “终端”
我们可以叫它 “终端模拟程序” (Terminal Emulators)。

常见的 Terminal Emulators 有：

-   Windows 上的 cmd.exe 和 powershell.exe 这样的应用程序，cmd 和 powershell 本身还是 Shell。
-   macOS 上的 Terminal 或是自己安装的 iTerm2 这样的应用程序，还有我个人最喜欢的 kitty。
-   Linux 上的就更多了，比如 Gnome 桌面环境 的 gnome-terminal，KDE 桌面环境的 konsole, 开源的 Alacritty、Hyper 等等。


## Console {#console}

在以前指的是主机 MainFrame 的控制面板，上面有显示寄存器的指示灯和操作寄存器的开关，它与电脑主机紧密结合，无法远程操作。

Console 和 Terminal 如下图所示：

{{< figure src="/ox-hugo/_20221106_145358screenshot.png" >}}

现在我们一般所说的 Console 大概指的是个人电脑中的设置相关的程序。

