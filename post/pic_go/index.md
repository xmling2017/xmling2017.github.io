
<!--more-->
## PicGo+阿里云OSS

### 购买阿里云OSS服务

访问[阿里云](https://www.aliyun.com), 登录成功后, 点击右上角的「控制台」.

展开左侧导航栏, 点击「对象存储OSS」

在对象存储栏的概述一栏的右边有一个「创建Bucket」, 但是在点击「确定」之前, 需要访问[购买链接]( https://common-buy.aliyun.com/?spm=5176.8465980.0.0.4e701450E6303q&commodityCode=ossbag&request=%7B%22region%22%3A%22china-common%22%7D#/buy), 购买存储空间. 

相应选项:
- `资源包类型`选择**标准(LRS)存储包**
- `地域`一般选择**中国大陆通用**
- `存储包规格`我选择的是**40 GB**
- `购买时长`我选择**一年**

以上价格只需**9元/每年**, 购买完毕后, 再回到「配制Bucket」

首先Bucket名称无所谓, 随便填；地域根据自己所在位置, 选择一个近些的即可；需要注意的是: 「读写权限」应选择**公共读**, 其余的全部按照默认即可
<div align="center" ><img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202207141749646.jpg" alt="" width="450" height=""></img></div>



注意, 除了存储包的费用, 还需要**支付流量费用**, 如果你安装了**阿里云APP**, 它会提醒你

创建完Bucket, 下一步是添加用户, 点击网站头像, 下拉点击「访问控制」, 点击「用户」, 点击「创建用户」, 填写登录名称和显示名称, 并勾选「编程访问」, 点击确定.

此时会出现最为重要的**AccessKey ID**和**AccessKey Secret**, 建议复制到本地, 在后面配制PicGo是会用到

勾选用户名, 点击「添加权限」, 选择「管理对象存储服务(OSS)权限」, 并点击确定. 

如此便完成了阿里云OSS服务的购买和配制

### 配制PicGo

免费下载安装[PicGo](https://github.com/Molunerfinn/PicGo/releases), 也可以关注「**青柠学术**」公众号, 后台回复**PicGo**获取安装包

安装完毕后, 点击「打开详细窗口」, 选择阿里云OSS, 进行图床配制, 然后点击确定即可

配制如图
<div align="center" ><img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202207141830531.png" alt="" width="450" height=""></img></div>

说明如图
<div align="center" ><img src="https://pullup.oss-cn-shanghai.aliyuncs.com/img/202207141831943.png" alt="" width="450" height=""></img></div>

## PicGo设置

进入软件后, 点击「PicGo设置」

1. 开启时间戳重命名
2. 快捷键设置, 这个根据自己情况而定
3. 链接格式, PicGo支持Markdow, HTML, URL, UBB, Custom
4. 插件镜像地址设置为`https://registry.npm.taobao.org/`

<!-- ## PICgo+Gitee

建议**大于1MB**的图片采用上面阿里云OSS图床, **小于1MB**采用Gitee图床

首先进入Picgo的`设置->插件设置`, 搜索`Gitee`

选择第三个`gitee-uploader 1.1.2`进行安装, 这是会弹出安装`node.js`, 安装即可

进入网站后选择下载左边的版本, 接着重新打开PicGo, 再次搜索`gitee-uploader 1.1.2`, 即可顺利安装

### Gitee图床创建

前提是你需要先注册Gitee账号, 登录后, 在右上角的+号处, 点击新建仓库, 如图
<img src="https://gitee.com/thankson_2017/fighome/raw/master/img/1.jpg" style="zoom:67%;" />

仓库名称随便, 选择开源, 勾选`设置模板->readme文件`, 勾选`选择分支模型->单分支模型(只创建master分支)`, 然后点击创建即可

再次点击Gitee头像, 进入「设置」, 点击左侧的「私人令牌」, 点击「生成新令牌」, 在提示页面处, 先取消全选, 再勾选`projects`, 然后提交. 最后将私人令牌复制到本地即可, 注意一定要复制, 该窗口关闭后, 无法查看该令牌

### 图床配制

进入PicGo设置界面, 在左边找到Gitee, 配制如图
<img src="https://gitee.com/thankson_2017/fighome/raw/master/img/20220128194211.png" style="zoom:67%;" />

说明如图
![](https://gitee.com/thankson_2017/fighome/raw/master/img/20220215165911.png)
gitee账户名/仓库名可以在你的Gitee仓库的网页地址中复制

填写完毕后, 就可以点击确定即可, 最好设置为默认图床 -->

## REF.
1. https://mp.weixin.qq.com/s/VpUOFw1PME-iEm9ToFP1Zw
2. https://mp.weixin.qq.com/s?__biz=MzAxNzgyMDg0MQ==&mid=2650466987&idx=1&sn=9ded00ede1bd6dc3aace8cf0f497d526&scene=21#wechat_redirect
3. https://mp.weixin.qq.com/s?__biz=MzAxNzgyMDg0MQ==&mid=2650465902&idx=1&sn=50c39a2043888f865ab0fd20466540be&scene=21#wechat_redirect
