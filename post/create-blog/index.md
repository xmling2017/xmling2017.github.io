原本想一直用博客园就好，简简单单，不需要过多的折腾。但是因为博客园的编辑窗口实在是太丑，且我还是喜欢文章放在本地就好，只存在网路上实在是不放心。另外就是，像hugo、hexo这类静态博客只需要几个命令就可以发布文章，基于Go语言的hugo部署会更加便捷，因此我就选择了hugo
<!--more-->

{{% admonition tip "note" %}}
所有的操作都是基于win10系统，若读者用的是其他系统，可以不用浪费时间了
{{% /admonition %}}

## hugo的安装
网上有两种方法

### 包管理器安装(方法一)
进入[chocolatet官网](https://chocolatey.org/)、[scool官网](https://scoop.sh/)或直接复制以下代码`Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`键入命令行，完成安装。命令行键入`choco --version`，若显示版本号则证明chocolatey已经安装成功

然后直接使用命令安装hugo：
- `choco install hugo-extended -confirm`
- `hugo version`检查是否安装成功

### 直接安装(方法二)
直接到[源代码](https://github.com/gohugoio/hugo/releases)处下载hugo，下载后解压缩至指定路径下[^1]，并将该路径添加至**环境变量**

[^1]: 路径最好全是英文组成

## 建站及选择主题
- 首先，在指定路径下启动命令窗口，键入`hugo new site mysite`，这里的*mysite*可自行修改。此时`cd mysite`进入新站根目录
- 到[主题官网](https://themes.gohugo.io/)上选择合适的主题，在这里我以[huho-theme-even](https://themes.gohugo.io/themes/hugo-theme-even/)为例。

{{% admonition note "注意" %}}
以下操作都是在根目录下进行，除非另外强调
{{% /admonition %}}

```
git init

git clone https://github.com/olOwOlo/hugo-theme-even themes/even #克隆到本地

cp themes/even/exampleSite/config.toml ./  #使用even主题自带的全局配置config.toml覆盖Hugo初始安装的config.toml

cp themes/even/archetypes/default.md ./archetypes/  #使用even主题自带的博客文章默认配置themes/even/archetypes/default.md覆盖Hugo初始安装的archetypes/default.md

cp themes/even/exampleSite/ ./content/  # 配置默认文章

hugo server #部署本地网址(http://localhost:1314/)，点击查看效果
```

{{% admonition note "注意" %}}
highlightInClient配置项和pygments开头的配置项不能都启用，都启用会导致markdown里插入代码块的时候样式错误。highlightInClient使用默认的false即可，不用修改。

修改archetypes/default.md里的默认配置项，把comment和toc都设置为true，用于开启文章评论功能和文章自动生成目录功能。
{{% /admonition %}}

## Github Page
这一步是将新站目录下的public文件夹上传到你的github或gitee(以github为例)

建立新的仓库，注意仓库名为**你的用户名.github.io**

### 部署到github page
因为现在github分支默认是`main`，而git默认是`master`，所以对于github，因此推送时会在仓库中新增一个分支。我只推荐将自己的仓库克隆进本站，合并到`public`文件夹中，然后将该文件夹push同步即可

{{% admonition tip "建议" %}}
另外，不推荐gitee，由于我们这的严格审核正策导致了从注册到申请非常麻烦，有这时间吧倒不如去阿里云申请免费域名挂接你的网址链接
{{% /admonition %}}

## 更新文章->提交更新
```
hugo -D
hugo #这两步仍然是在站点根目录，目的是将文章由.md格式转换为.html格式
cd public
git init
git add .
git commit -m "提交"
git push -u origin main #首次使用这个命令，后面直接用git push即可
```
到这里，你的博客已经更新完毕。

## hugo的缺点
虽然hugo是基于Go语言设计的，但是麻烦的命令仍然是个问题，建议大家写一份批处理文件，一步到位。

还有就是，当你将文章hugo成html后，你之后的增删都不会改变`public/post/`文件夹里的文件，我的做法是直接删除`public/post/`内的所有文件和文件夹，重新hugo即可

## 关联国内域名
这里我推荐阿里云域名

首先要提示的是，当你更换网络时，需要重新解析域名，如`Ping xxxx.github.io`获得解析码


## REF.
1. http://t.csdn.cn/Yb6wW
2. https://segmentfault.com/a/1190000041156732
3. https://zhuanlan.zhihu.com/p/396669087