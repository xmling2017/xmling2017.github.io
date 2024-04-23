
<!-- more -->

> 具体操作请观看b站up主[老湿基](https://www.bilibili.com/video/BV11y4y147y4?share_source=copy_web)

## 步骤

1. 先下载最新版[PowerShell](https://github.com/PowerShell/PowerShell)并安装，下载[配制文件](https://caiyun.139.com/m/i?185C7GiB2VOiN)[^1]

2. 到Microsoft store安装windows terminal，

3. 打开所下载的配置文件，选择FiraCode文件夹，选中**Fura Code Regular Nerd Font Complete Windows Compatible.otf**文件，右击点击安装。

4. 打开所下载的windows terminal，点击下拉菜单上的设置按钮，进入settings.js文件，将配制文件中的代码拷贝覆盖即可

5. 在命令行中输入`notepad $Profile`，在弹出的文件中粘贴配置文件中的代码

6. 完成以上步骤后，用管理员身份打开powershell，依次输入以下命令

   ```
   Install-Module -Name PSReadLine  -Scope CurrentUser #安装 PSReadline 包，该插件可以让命令行很好用，类似 zsh

   Install-Module posh-git  -Scope CurrentUser #安装 posh-git 包，让你的 git 更好用

   Install-Module oh-my-posh -Scope CurrentUser #安装 oh-my-posh 包，让你的命令行更酷炫、优雅
   ```

7. 解压配制文件中的一个压缩包，在/open_windows_terminal_here/src/目录下以管理员身份运行powershell，运行`./install.ps1`

## REF.

[Windows Terminal 完美配置 PowerShell 7.1 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/137595941)

<iframe height=189 width=336 src="//player.bilibili.com/player.html?aid=802219507&bvid=BV11y4y147y4&cid=311976075&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

[^1]: 密码是**i8LN**