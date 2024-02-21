# KDE Plasma 桌面 "KOrganizer" 等软件关闭后仍然占用约 800 M 内存


## 问题描述 {#问题描述}

{{< figure src="/ox-hugo/img_20240221_163427.png" width="800" >}}

我在使用 KDE Plasma 桌面的时候，打开资源监视器发现 KOrganizer 这个软件明明已经退出了，但还是占用了我约 800 MB 的内存，我很疑惑，是不是出现什么漏洞了。

当我仔细看了一下它的进程后发现，原来是调用了一个 mysql 服务器在后台运行。还有一个叫做 akonadi 的进程。

查了一下发现 [Akonadi](https://userbase.kde.org/Akonadi#ApplicationTable) 框架是给 [KDE PIM applications](https://kontact.kde.org/) 提供数据支持的，退出软件之后，Akonadi 仍然运行应该属于一个 Bug 没有修复。


## 解决方法 {#解决方法}

我们只需要在终端中用 `akonadictl stop` 将 Akonadi 服务关闭即可回收这 800 MB 的内存了。

也可以一劳永逸地将 Akonadi 框架关闭：

```bash
 sed -i 's/StartServer.*/StartServer=false/' ~/.config/akonadi/akonadiserverrc
```

接着，我们还需要在设置里面点击 Workspace &gt; Search &gt; Plasma Search 取消勾选 PIM Contacts Search 搜索项并应用。

注意，当你关闭了 Akonadi 框架之后，所有的 KDE PIM 套件（包括 Kontact KMail KOriganizer）都不能正常使用了，如果你还安装了一些第三方的桌面时钟挂件，它们也有可能出问题。

你可以使用第三方邮件或是日程软件来替代 KDE 提供的 PIM 套件。

