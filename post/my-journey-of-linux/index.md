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

Kubuntu的分区方案一般是，500MB的`EFI系统分区`（现在改成1GB的boot空间，防止之后更新驱动时存储空间不够），8GB的`swap也就是交换空间`，40~60GB的根目录（**尽可能的放在固态硬盘**），剩下的home空间能留多少留多少，二者都用于Ext4日志文件系统。

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

### 安装 flux[^1]

>需要python3
>
>安装必要的依赖`sudo apt-get install python3-pexpect python3-distutils python3-xdg gir1.2-ayatanaappindicator3-0.1 gir1.2-gtk-3.0 redshift`

```
# Download fluxgui
cd /tmp
git clone "https://github.com/xflux-gui/fluxgui.git"
cd fluxgui
./download-xflux.py

# EITHER install system wide
sudo ./setup.py install --record installed.txt

# EXCLUSIVE OR, install in your home directory
#
# The fluxgui program installs
# into ~/.local/bin, so be sure to add that to your PATH if installing
# locally. In particular, autostarting fluxgui in Gnome will not work
# if the locally installed fluxgui is not on your PATH.
./setup.py install --user --record installed.txt
       
# Run flux
fluxgui
```
进入首选项输入经纬坐标，[查找定位](https://justgetflux.com/map.html)。

将flux小程序设置成开机自启。

### 安装 Anki

>首先安装依赖`sudo apt install libxcb-xinerama0 libxcb-cursor0 libnss3`

从[官网](https://apps.ankiweb.net)下载Anki（一般选择「-qt5」），如果`zstd`还没有安装，则`sudo apt install zstd`

接着在命令行中输入
```
tar xaf Downloads/anki-2XXX-linux-qt6.tar.zst
cd anki-2XXX-linux-qt6
sudo ./install.sh
```

最后你就可以键入`anki`运行即可。其他信息（比如升级等）可参考[这篇文章](https://docs.ankiweb.net/platform/linux/installing.html)。

### 安装 PicGo + Gitee 配置
通过Gitee与PicGo搭建图床。

PicGo的[下载地址](https://github.com/Molunerfinn/PicGo/releases/tag/v2.3.1)，下载后缀为「AppImage」文件，点击文件「属性」，授予权限，可作为运行文件运行。

**安装Nodejs**：首先下载[nodejs](https://nodejs.org/en)，并解压。

将文件夹*node-vxxx-linux-x64*拷贝到`/usr/local/lib/nodejs`下，并将Nodejs添加到环境变量。
```
sudo cp -r node-vxxx-linux-x64 /usr/local/lib/nodejs
echo "export export PATH=/usr/local/lib/nodejs/bin:$PATH" >> ~/.bashrc
echo "export export PATH=/usr/local/lib/nodejs/bin:$PATH" >> ~/.bashrc
. ~/.profile
. ~/.bashrc
// 测试下，是否安装成功，如果成功会打印相应的版本号
node -v
npm -v
// 为了在sudo 执行命令时也能找到相应的指令，可创建一个软连接在/usr/bin中
sudo ln -s /usr/local/lib/nodejs/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/nodejs/bin/npm /usr/bin/npm
sudo ln -s /usr/local/lib/nodejs/bin/npx /usr/bin/npx
```

---

{{% admonition info "提示" %}}
感觉之后的内容并不需要进行
{{% /admonition %}}

**安装PicGO-core**：
```
sudo npm install -g cnpm     // 配置cnpm
sudo cnpm install picgo -g   // 安装PicGo-core
picgo install gitee-uploader  // 安装插件picgo-plugin-gitee-uploader
```
修改「PicGo-core」配置文件：`xed ~/.picgo/config.json`，文件内容如下：
```
{
  "picBed": {
    "uploader": "gitee",
    "current": "gitee",
    "gitee": {
      "message": null,
      "owner": "自己任意填一个就行",
      "path": "img",
      "repo": "创库路径",
      "token": "令牌"
    }
  },
  "picgoPlugins": {
    "picgo-plugin-gitee-uploader": true,
    "picgo-plugin-super-prefix": true
  },
  "picgo-plugin-super-prefix": {
    "fileFormat": "YYYYMMDDHHmmss"
  },
  "picgo-plugin-gitee-uploader": {
    "lastSync": "2021-06-26 11:47:30"
  }
}
```

### AppImage 桌面快捷的建立

AppImage是Linux系统中的单文件版，点击即可使用。但是不能像Windows那样直接创建桌面快捷图标。

首先赋予单文件运行权限`sudo chmod +x XXX.AppImage`

所有的信息都已被打包压缩到了单文件中，通过命令`./XXX.AppImage --appimage-extract`解压，解压后文件夹中的Desktop文件和Png文件分别移动到`/usr/share/applications/`和`~/图片/`中。

<center>
  <img src="https://raw.githubusercontent.com/xmling2017/lm-graph/raw/master/img/202405231751119.png" alt="" width="350" height=""></img>
  <br>
  <font size="2"><strong>▲ 以PicGo为例</strong></font>
</center>

### 安装 JAVA[^2]

*OpenJDK*和*OracleJDK*是最主要的两个Java版本，除了Oracle拥有极少的一些额外特性之外，两个之间基本没有什么不同。

如果不确定是否已经安装了Java，可以通过命令`java -version`来查询版本号。如果没有安装，则有「OpenJDK 11」和「OpenJDK 8」两个长期支持版本可供选择。

**终端输入**：
```
sudo apt update
sudo apt install openjdk-11-jdk #或者openjdk-8-jdk
```

**设置默认版本**：

如果系统上安装了多个版本，可以输入如下命令`java -version`，检测哪个版本被设置成了默认值。如果想要修改默认的版本，使用`sudo update-alternatives --config java`命令，输出结果如下：
```
here are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

Press <enter> to keep the current choice[*], or type selection number:
```
所有已经安装的Java版本将会列出，输入你想要设置为默认值的需要，同时按`Enter`即可。

**设置环境变量**：

查看、修改并保存环境变量
```
vim ~/.bashrc
source ~/.bashrc

#将下列四行字符串添加至合适位置
export JAVA_HOME=/usr/lib/jvm/jdkXXX
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=$PATH:${JAVA_HOME}/bin
```

更新完系统后，输入`jps -l`查看命令是否已成功加载。

**卸载Java**：

就像卸载任意软件包那样，输入`sudo apt remove openjdk-11-jdk`即可


### 安装 pycharm 并激活

以Pycharm 2022.1为例，到[官网](https://www.jetbrains.com/pycharm/download/)上下载合适的安装包

<center>
  <img src="https://raw.githubusercontent.com/xmling2017/lm-graph/raw/master/img/202405241210548.png" alt="" width="350" height=""></img>
  <br>
  <font size="2"><strong>▲ 选择合适的版本下载</strong></font>
</center>

将下载好的pycharm安装包解压至~/下载目录，并将解压后的文件夹移到`/usr/local`目录下、重命名：
```
tar -xvf pycharm-XXX.tar.gz
sudo mv ./pycharm-XXX /usr/local/pycharm
```

下载补丁[^网盘链接1]，将补丁文件夹移动到与Pycharm同一目录下`sudo  mv ja-netfilter /usr/local`，如图：

<center>
  <img src="https://raw.githubusercontent.com/xmling2017/lm-graph/raw/master/img/202405241224982.png" alt="" width="350" height=""></img>
  <br>
  <font size="2"><strong>▲ 两个文件夹放在同一目录下面</strong></font>
</center>

**修改配置文件**，进入`/usr/local/pycharm/bin`目录下，修改文件`pycharm64.vmoptions`
```
# 引用补丁，开头必须以 -javaagent: 开头，后面跟着补丁的绝对路径（可根据你实际的位置进行修改）,注意路径一定要填写正确，且不能包含中文，否则会导致 Pycharm 无法启动
-javaagent:/usr/local/ja-netfilter/ja-netfilter.jar
 
# 最新 Pycharm 版本需要添加下面两行，否则会报 key valid
--add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED
--add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED
```

{{% admonition warning "警告" %}}
2018版本及一下不需要在配置文件中输入最后两行字符串，以上的版本输入时切勿添加空格，否则会在激活时报错找不到或无法加载主类A！！！（当然所有的注释也没必要写入）
{{% /admonition %}}

**更改环境变量**，然后`source ~/.bashrc`
```
export PyCharm_HOME=/usr/local/pycharm
export PATH=${PyCharm_HOME}/bin:$PATH
```

{{% admonition tip "报错" %}}
若配置完环境变量后，在已经安装Java的情况下使用jps时提示：找不到命令“jps”，可能是环境变量配置顺序影响，可参考[该文章](https://blog.csdn.net/qq_67822268/article/details/136889908)解决。
{{% /admonition %}}

**启动并激活Pycharm**：

```
pycharm.sh
```

注意启动后不能关闭终端，勾选后点击继续：

<center>
  <img src="https://raw.githubusercontent.com/xmling2017/lm-graph/raw/master/img/202405241238439.png" alt="" width="350" height=""></img>
  <br>
  <font size="2"><strong>▲ 按顺序勾选</strong></font>
</center>

选择不发送

<center>
  <img src="https://raw.githubusercontent.com/xmling2017/lm-graph/raw/master/img/202405241241518.png" alt="" width="350" height=""></img>
  <br>
  <font size="2"><strong>▲ 选择不发送</strong></font>
</center>

选择输入激活码（激活码在文末[^3]）激活：

<center>
  <img src="https://raw.githubusercontent.com/xmling2017/lm-graph/raw/master/img/202405241242262.png" alt="" width="350" height=""></img>
  <br>
  <font size="2"><strong>▲ 按顺序填入</strong></font>
</center>

至此，Pycharm安装成功！！

**设置Pycharm的桌面快捷方式**：

安装目录下的`./pycharm.sh`是Pycharm的启动程序。首先在`/usr/share/applications/`目录下*touch*一个新的桌面文件「pycharm.desktop」，以root权限修改该文件，添加文本如下：
```
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec="/XXX/bin/pycharm.sh" %f
Icon=/XXX/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;
```
只需要修改`Exec`和`Icon`两个属性。

最后添加可执行权限`sudo chmod +x pycharm.desktop`

[^1]: Better lighting for Linux.
[^2]: Java是世界上最流行的编程语言之一，被用来构建各种不同的应用和系统
[^网盘链接1]: 链接: https://pan.baidu.com/s/1oWe8PMUHIYbYLeZNEsHLMg 提取码: ehjc
[^3]: VAE9B0CRYZ-eyJsaWNlbnNlSWQiOiJWQUU5QjBDUllaIiwibGljZW5zZWVOYW1lIjoiZnV6emVzIGFsbHkiLCJhc3NpZ25lZU5hbWUiOiIiLCJhc3NpZ25lZUVtYWlsIjoiIiwibGljZW5zZVJlc3RyaWN0aW9uIjoiIiwiY2hlY2tDb25jdXJyZW50VXNlIjpmYWxzZSwicHJvZHVjdHMiOlt7ImNvZGUiOiJQU0kiLCJmYWxsYmFja0RhdGUiOiIyMDIzLTA3LTAxIiwicGFpZFVwVG8iOiIyMDIzLTA3LTAxIiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBDIiwiZmFsbGJhY2tEYXRlIjoiMjAyMy0wNy0wMSIsInBhaWRVcFRvIjoiMjAyMy0wNy0wMSIsImV4dGVuZGVkIjpmYWxzZX0seyJjb2RlIjoiUFBDIiwiZmFsbGJhY2tEYXRlIjoiMjAyMy0wNy0wMSIsInBhaWRVcFRvIjoiMjAyMy0wNy0wMSIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQQ1dNUCIsImZhbGxiYWNrRGF0ZSI6IjIwMjMtMDctMDEiLCJwYWlkVXBUbyI6IjIwMjMtMDctMDEiLCJleHRlbmRlZCI6dHJ1ZX0seyJjb2RlIjoiUFdTIiwiZmFsbGJhY2tEYXRlIjoiMjAyMy0wNy0wMSIsInBhaWRVcFRvIjoiMjAyMy0wNy0wMSIsImV4dGVuZGVkIjp0cnVlfV0sIm1ldGFkYXRhIjoiMDEyMDIyMDcwMVBTQU4wMDAwMDUiLCJoYXNoIjoiVFJJQUw6MTMxNzYyODYxMCIsImdyYWNlUGVyaW9kRGF5cyI6NywiYXV0b1Byb2xvbmdhdGVkIjpmYWxzZSwiaXNBdXRvUHJvbG9uZ2F0ZWQiOmZhbHNlfQ==-YxAJSVk5XIZkkI6vH33zgb/hRmCdqia89zpsVHp2x52PY0XgOOiAlcR3/BVhm0qRYLBYBBHMpPcz0+ZWr2diKy0QexfbtVIVsCRkVaRgl67Tbw9MKb5jVNqpqth2yEoW/gmm2bZC5RS0qiGcPQpjD7AdRo66P78Vb2TrJ5hz055polMwR0hMxm9ECDedLnqKQXyzmcjkucStFNYYHbF0Gnn0I/xrxnVoIDeHMdlsRiBXYPb6TGIVgOIh8ynuGwvP/svLVPCI1dYPYF1V3ndDbOOQskOJaC+7K1/80xVEb3TT7Orb7PJJDX1AiIjg0gsSctPulz3r1xLHIZNcZJcV0A==-MIIETDCCAjSgAwIBAgIBDTANBgkqhkiG9w0BAQsFADAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBMB4XDTIwMTAxOTA5MDU1M1oXDTIyMTAyMTA5MDU1M1owHzEdMBsGA1UEAwwUcHJvZDJ5LWZyb20tMjAyMDEwMTkwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCUlaUFc1wf+CfY9wzFWEL2euKQ5nswqb57V8QZG7d7RoR6rwYUIXseTOAFq210oMEe++LCjzKDuqwDfsyhgDNTgZBPAaC4vUU2oy+XR+Fq8nBixWIsH668HeOnRK6RRhsr0rJzRB95aZ3EAPzBuQ2qPaNGm17pAX0Rd6MPRgjp75IWwI9eA6aMEdPQEVN7uyOtM5zSsjoj79Lbu1fjShOnQZuJcsV8tqnayeFkNzv2LTOlofU/Tbx502Ro073gGjoeRzNvrynAP03pL486P3KCAyiNPhDs2z8/COMrxRlZW5mfzo0xsK0dQGNH3UoG/9RVwHG4eS8LFpMTR9oetHZBAgMBAAGjgZkwgZYwCQYDVR0TBAIwADAdBgNVHQ4EFgQUJNoRIpb1hUHAk0foMSNM9MCEAv8wSAYDVR0jBEEwP4AUo562SGdCEjZBvW3gubSgUouX8bOhHKQaMBgxFjAUBgNVBAMMDUpldFByb2ZpbGUgQ0GCCQDSbLGDsoN54TATBgNVHSUEDDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBaAwDQYJKoZIhvcNAQELBQADggIBABqRoNGxAQct9dQUFK8xqhiZaYPd30TlmCmSAaGJ0eBpvkVeqA2jGYhAQRqFiAlFC63JKvWvRZO1iRuWCEfUMkdqQ9VQPXziE/BlsOIgrL6RlJfuFcEZ8TK3syIfIGQZNCxYhLLUuet2HE6LJYPQ5c0jH4kDooRpcVZ4rBxNwddpctUO2te9UU5/FjhioZQsPvd92qOTsV+8Cyl2fvNhNKD1Uu9ff5AkVIQn4JU23ozdB/R5oUlebwaTE6WZNBs+TA/qPj+5/we9NH71WRB0hqUoLI2AKKyiPw++FtN4Su1vsdDlrAzDj9ILjpjJKA1ImuVcG329/WTYIKysZ1CWK3zATg9BeCUPAV1pQy8ToXOq+RSYen6winZ2OO93eyHv2Iw5kbn1dqfBw1BuTE29V2FJKicJSu8iEOpfoafwJISXmz1wnnWL3V/0NxTulfWsXugOoLfv0ZIBP1xH9kmf22jjQ2JiHhQZP7ZDsreRrOeIQ/c4yR8IQvMLfC0WKQqrHu5ZzXTH4NO3CwGWSlTY74kE91zXB5mwWAx1jig+UXYc2w4RkVhy0//lOmVya/PEepuuTTI4+UJwC7qbVlh5zfhj8oTNUXgN0AOc+Q0/WFPl1aw5VV/VrO8FCoB15lFVlpKaQ1Yh+DVU8ke+rt9Th0BCHXe0uZOEmH0nOnH/0onD