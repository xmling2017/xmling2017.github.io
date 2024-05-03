由于一些字体方面的问题，LaTeX在Windows下速度会比Linux慢一些，对于不希望只因为这个而整体迁移到Linux的用户，使用Windows提供的Linux子系统不失为一个不错的选择。
<!--more-->

## VSCode下载

  ### 国内镜像下载

因为VsCode官方下载速度实在另人捉急，使用国内镜像下载速度可以直接起飞，废话不多说：

1. 进入[VsCode官方网站](https://code.visualstudio.com/)选择对应版本下载
2. 复制下载链接，如下面的链接：`https://vscode.cdn.azure.cn/stable/b4c1bd0a9b03c749ea011b06c6d2676c8091a70c/VSCodeUserSetup-x64-1.57.0.exe`
3. 把地址链接中「/stable」前替换成`vscode.cdn.azure.cn`

这样就可以实现超速下载

## WSL(windows subsystem for Linux)非C盘（系统盘）下载

WSL让我们可以像普通软件一样直接使用Linux的功能。配合**Windows Terminal**，拥有比虚拟机更方便的启动方式。

但是有一个另人困扰的问题：在Microsoft Store下载的WSL发行版会自动安装到C盘，不能手动修改安装位置。

微软提供了一个手动下载WSL发行版的地址：[手动下载适用于Linux的Windows子系统发行版包](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)

<center>
  <img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202301161507842.png" alt="" width="300" height=""></img>
  <br>
  <font size="2"><strong>从这里下载WSL发行版，可以绕开MS Store的自动安装</strong></font>
</center>

选择想要的发行版下载后，解压得到一系列后缀为`.appx`的文件，把它的后缀改为`.zip`，然后解压到想要安装WSL的目录下，进入解压后的文件夹，双击「ubuntu.exe」（或者其他的发行版，这里只是一个例子），等待一段时间后就成功安装到当前目录。


需要注意的是安装目录的磁盘不能开**压缩内容以便节省磁盘空间**选项，否则会报错`0xc03a001a`，可以右键`磁盘-->属性`，找到并关闭这个选项

<center>
  <img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202301162113434.png" alt="" width="300" height=""></img>
  <br>
  <font size="2"><strong>该选项所在位置</strong></font>
</center>

## 在WSL中安装LaTeX

### 下载LaTeX

打开[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)，到图中所示路径下载合适的iso镜像文件

<center>
  <img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202301162120541.png" alt="" width="300" height=""></img>
  <br>
  <font size="2"><strong>镜像网站下载路径</strong></font>
</center>

### 通过iso镜像文件安装
在线安装速度相对会比较慢，可以通过加载虚拟光驱进行安装，在Windows中加载虚拟光驱，并记住光驱盘符（下面假设光驱盘符为F:）。

在WSL中创建文件夹，挂载虚拟光驱到新创建的文件夹，然后进入安装程序：
```
sudo mkdir /mnt/texlive
sudo mount -t drvfs F: /mnt/texlive
sudo mnt/texlive/install-tl
```
设置好后，键入*i*开始安装

安装好后，需要解除挂载状态，并删除安装包，命令如下：
```
sudo umount /mnt/texlive
sudo rm -r /mnt/texlive/
```

### 设置环境变量

设置环境变量有两种方法，一种是通过编辑`~/.bashrc`文件，在该文件最后添加`export PATH=/.../:$PATH`，具体见下面代码。但是这样设置的变量在Windows的cmd或PowerShell里无法通过`wsl tex`来使用
```
export PATH=/usr/local/texlive/2022/bin/x86_64-linux:$PATH
export MANPATH=/usr/local/texlive/2022/texmf-dist/doc/man:$MANPATH.
export INFOPATH=/usr/local/texlive/2022/texmf-dist/doc/info:$INFOPATH.
```

另一种方式是，当通过apt安装的texlive可以通过`wsl`命令调用，而这样安装的texlive可执行文件是放在`/usr/local/bin`目录下，那么创建一个软链接放到该文件夹下，就可以直接撞见texlive所有的可执行文件的软链接，然后就可以直接在PowerShell下使用。
```
#需要提前了解texlive的安装目录
sudo /usr/local/texlive/2020/bin/x86_64-linux/tlmgr path add
```

第二种方式下，在PowerShell下输入`wsl tex -v`即可得到安装的texlive的安装信息。而第一种方法下，只能在wsl命令界面输入`tex -v`才能得到。

### 在WSL里使用Windows字体

安装「fontconfig」：
```
sudo apt install fontconfig
```
在`/etc/fonts/`新建一个文件`local.conf`，添加一下内容：
```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <dir>/mnt/c/Windows/Fonts</dir>
</fontconfig>
```
然后使用`sudo fc-cache -fv`刷新一下字体缓存，就可以使用Windows中的字体。

**提示：**如果出现无权限保存所编辑的文件时，建议先在桌面上新建需要保存的文件，然后用`sudo`来复制到目标文件夹下

## 在VSCode中使用

### 使用Remote插件

要在VSCode下使用WSL的texlive，可直接安装WSL插件

## 卸载

虽然Linux发行版可以通过Microsoft Store安装，但不能通过Microsoft Store卸载。

需要操作的步骤如下：
1. 查看当前环境安装的wsl

   ```
   wsl --list
   ```

<center>
  <img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202301240954024.png" alt="" width="300" height=""></img>
  <br>
  <font size="2"><strong>1</strong></font>
</center>

1. 注销（卸载）当前安装的Linux的Windows子系统

   ```
   wsl --unregister Ubuntu
   ```


**注意：**一旦取消注册，与该发行版相关的所有数据、设置和软件都将永久丢失。可以从商店重新安装干净的副本。

### 完全卸载texlive

```
sudo apt-get purge texlive*
rm -rf /usr/local/texlive/2022
rm -rf ~/.texlive2022
rm -rf /usr/local/share/texmf
rm -rf /var/lib/texmf
rm -rf /etc/texmf
sudo apt-get remove tex-common --purge
rm -rf ~/.texlive
```



## REF.

1. [windows 10卸载(注销)WSL，注销（卸载）当前安装的Linux的Windows子系统](https://blog.csdn.net/l_fly_j/article/details/121769346)
2. [linux完全卸载texlive](https://blog.csdn.net/qq_40199232/article/details/106505730)
3. [自定义WSL的安装位置，别再装到C盘啦 - Locietta的文章 - 知乎](https://zhuanlan.zhihu.com/p/263089007)
4. [在WSL中安装LaTeX - Marvey的文章 - 知乎](https://zhuanlan.zhihu.com/p/202865739)

<center><iframe height=189 width=336 src="//player.bilibili.com/player.html?aid=819894603&bvid=BV1CG4y1L7p9&cid=956367893&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe></center>