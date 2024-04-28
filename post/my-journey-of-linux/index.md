给十年前的老式电脑安装的Kubuntu220.04 LST系统，仅用来满足日常的LaTeX内容输出和一些基础的本文处理，因此本文涉及到的内容十分基础（甚至错误），安装的软件也仅仅是所需要的那几款。
<!--more-->
## 安装Kubuntu220.04LST系统双系统

这部分内容参考[Windows 和 Ubuntu 双系统的安装和卸载](https://www.bilibili.com/video/BV1554y1n7zv/?share_source=copy_web&vd_source=74f7aedcc2240cf73120aa58afdcb5b7)。

### 下载系统镜像文件并制作启动盘

打开[Kubuntu 官网](https://kubuntu.org/getkubuntu/)，点击所需要的版本下载。

下载完镜像文件后，需要用软件制作USB启动盘，这里推荐[balenaETcher](https://www.balena.io/etcher)，操作简单，不多逼逼。

### 磁盘分区和格式

现在的电脑磁盘格式一般是GPT格式，但不排除一些旧电脑是MBR。这里仅介绍GPT格式的安装方式。

安装之前需要到电脑的BIOS界面（我的电脑是按F7进入），设置EFI优先启动。

Kubuntu的分区方案一般是，500MB的`EFI系统分区`，8GB的`swap也就是交换空间`，40~60GB的根目录（**尽可能的放在固态硬盘**），剩下的home空间能留多少留多少，二者都用于Ext4日志文件系统。

### 更换软件源

安装后默认的软件源是国外的，如果不更换的话，下载软件包会很慢，有些驱动的下载也会报错。

这里统一换成中科大的镜像软件源。打开`/etc/apt/sources.list`，将文件内所有的`cn.archive.ubuntu.com`换成`mirrors.ustc.edu.cn`。保存后连接网络刷新一下软件源，命令`sudo apt update`（这里的操作步骤也可以参考[Linuxmint21.3使用体验分享](https://www.bilibili.com/video/BV1AC411L7ey/)）

### 驱动及时间同步问题

在软件源更新后，就可以安装无线网卡驱动`sudo apt-get install --reinstall bcmwl-kernel-source`（该驱动是根据我的旧电脑的型号决定的）。

关于双系统的时间冲突，是因为两系统的时间机制不同，需要将Kubuntu的时间机制改成同Windows一样的就行。
```
#重新设定时间
sudo apt install ntpdate
sudo ntpdate time-windows.com
#转换时间机制，并同步BIOS的时间
sudo hwclock --localtime --systohc
```

{{% admonition info "提示" %}}
由于我们电脑开机的默认启动项都是Windows，这里只需要`sudo kate /etc/default/grub`，将文件中的启动数字改成对应的Windows的启动数字即可，然后`sudo update-grub`
{{% /admonition %}}

### 小鹤音形挂接Rime输入法的安装

首先是安装Rime，一般分为ibus框架和fcitx框架两种选择，以fcitx框架为例，Linux mint 可以点击`字体`, 下载简体中文框架即可.

1. 安装中州韻`sudo apt install fcitx-rime`
2. 挂载小鹤音形
   - [官网](http://flypy.ysepan.com/)下载码表，点击「挂接——音形码」，选择`小鹤音形“鼠须管”for macOS.zip`
   - 解压后，进入`/home/你的用户名/.config/fcitx`，把`rime`文件删除，然后把解压包中的`rime`文件夹复制进去。

### Windows 字体的安装

```
#先安装
sudo apt install -y font-manager ttf-mscorefonts-installer fontconfig
#创建目录
sudo mkdir /usr/share/fonts/myfonts
sudo chmod 755 -R /usr/share/fonts/myfonts
#复制准备好的字体
sudo cp ~/* /usr/share/fonts/myfonts/
sudo chmod 755 -R /usr/share/fonts/myfonts
```

然后转到新建的字体目录
```
cd /usr/share/fonts/myfonts/
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -fv
```

查看已经安装的字体`fc-list`，查看已经安装的中文字体`fc-list :lang=zh`

{{% admonition info "小插曲" %}}
在安装时，出现“正在设定ttf-mscorefonts-installer“，需要下拉到底部按Tab才能选中它，再按回车。
{{% /admonition %}}

---

## 常用软件的安装

### 安装LaTeX

>具体安装过程参考王然的《[一份简短的关于LaTeX 安装的介绍](https://github.com/OsbertWang/install-latex-guide-zh-cn)》

这里只阐述如何用镜像安装，镜像的获取方式可以通过清华镜像源。

将下载的光盘镜像进行装载
```
sudo mkdir /mnt/texlive
sudo mount ./texlive2022.iso /mnt/texlive
```
接下来执行
```
sudo /mnt/texlive/install-tl
```
进行安装，在屏幕上应该能看见以下内容
```
======================> TeX Live installation procedure <=====================

======> Letters/digits in <angle brackets> indicate <=======
======> menu items for actions or customizations <=======
= help> https://tug.org/texlive/doc/install-tl.html <=======

Detected platform: GNU/Linux on x86_64
。
。
。
<V> set up for portable installation

Actions:
<I> start installation to hard disk
<P> save installation profile to 'texlive.profile' and exit
<Q> quit

Enter command:
```
键入「I」进行默认安装（小白建议默认安装）。安装完毕后，将装载的光盘镜像弹出并删除文件夹
```
sudo umount /mnt/texlive
sudo rm -r /mnt/texlive
```
安装完成后，用户需要设置环境变量，
```
kate ~/.bashrc
# 在文末添加
export PATH=/usr/local/texlive/2022/bin/x86_64-linux:$PATH
export MANPATH=/usr/local/texlive/2022/texmf-dist/doc/man:$MANPATH
export INFOPATH=/usr/local/texlive/2022/texmf-dist/doc/info:$INFOPATH
```
保存退出，打开终端执行`tex -v`，若显示
```
TeX 3.141592653 (TeX Live 2022)
kpathsea version 6.3.4
Copyright 2022 D.E. Knuth.
There is NO warranty. Redistribution of this software is
covered by the terms of both the TeX copyright and
the Lesser GNU General Public License.
For more information about these matters, see the file
named COPYING and the TeX source.
Primary author of TeX: D.E. Knuth.
```
则安装成功，有时需要重启才能显示。

接下来，需要处理字体，在终端中执行 
```
sudo cp /usr/local/texlive/2022/texmf-var/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.config
```
将配置文件复制到系统，然后继续执行`sudo fc-cache -fsv`，刷新字体缓存，如此，TEX Live中的字体才能被正确调用

### 卸载TEX Live 
如果是从光盘镜像安装，直接删除文件夹即可，在终端中执行：
```
kpsewhich -var-value TEXMFROOT
```
来查询安装路径，进而通过`sudo rm -rf`命令进行删除，默认安装的用户直接运行
```
sudo rm -rf /usr/local/texlive/2022
rm -rf ~/.texlive2022
```
卸载完成后，再移除之前在`~/.bashrc`文件中设置的环境变量，接着在z终端执行`sudo visudo`，在`secure_path`中删除`/usr/local/texlive/2022/bin/x86_64-linux:`

### 安装 Zotero

[官网](https://www.zotero.org/download/)下载安装包，所下载的版本一般是`xxx.tar.bz2`文件。

解压在当前，生成了`Zotero_linux-x86_64`这个文件夹

创建 zotero 目录 ，这里选择`/opt/`这个目录下创建，因为这个目录通常放着浏览器。
```
sudo mkdir /opt/zotero
```
复制解压文件到`/opt/zotero`目录下
```
sudo mv Zotero_linux-x86_64/* /opt/zotero/
```
<span id="jump1">更新 zotero 的桌面位置</span>
```
cd /opt/zotero
sudo ./set_launcher_icon
```
创软连接到应用程序桌面
```
ln -s /opt/zotero/zotero.desktop ~/.local/applications/zotero.desktop
```
上面可能无法成功，请参考[【包学包会】【Zotero篇】Linux安装、重装和备份，换新电脑再也不用麻烦啦！不同系统的配置文件是通用的，香。](https://zhuanlan.zhihu.com/p/436241013
)

接着[更新zotero的桌面位置](#jump1)之后，将zotero安装目录下的`zotero.desktop`复制到`/usr/share/applications/`目录下，命令如下：
```
sudo cp -r zotero.desktop /usr/share/applications/
```
以管理员权限编辑该文件
```
cd /usr/share/applications/
sudo vi zotero.desktop
```
<center>
  <img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202404282003596.jpg" alt="" width="" height=""></img>
  <br>
  <font size="2"><strong>只需要修改红色箭头标出的位置即可</strong></font>
</center>

### 安装 Firefox 

安装 KUbuntu 桌面操作系统后，Firefox 默认是集成在系统里的，但是系统所自带的 Firefox ，属于国际版，与中国版所用的服务器不同。

到[中国版 Firefox 官网](http://www.firefox.com.cn/)下载最新版本。

卸载 KUbuntu 自带的 Firefox `sudo apt remove firefox `

将 Firefox `tar`包解压，然后移动到`/opt/`目录下
```
cd ~/Downloads
tar -jxvf Firefox-latest-x86_64.tar.bz2
sudo mv firefox /opt
```

然后需要将 Firefox 添加为 KUbuntu 桌面软链接
```
cd /usr/share/applications
# 创建 firefox.desktop 文件
sudo vi firefox.desktop 
```
>`firefox.desktop` 文件内容示例如下：
```
[Desktop Entry]
Name=firefox
Name[zh_CN]=火狐浏览器
Comment=火狐浏览器
Exec=/opt/firefox/firefox
Icon=/opt/firefox/browser/chrome/icons/default/default128.png
Terminal=false
Type=Application
Categories=Application;
Encoding=UTF-8
StartupNotify=true
```
文件保存后，就可以从桌面侧边栏看到 Firefox 图标。