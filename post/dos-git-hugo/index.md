前文所提到的解决hugo更新文章的操作命令繁琐的问题，笔者提出写一个批处理文件，一键提交更新。文件内容如下
<!--more-->
{{% admonition warning "警告" %}}
首先申明，我的博客根目录是`D:\Users\ThankSon\Documents\hugo\MyBlog\`，需要修改成你的博客路径
{{% /admonition %}}

```
@ECHO OFF
TITLE 博客部署

ECHO "Hello World"

rd /s /q D:\Users\ThankSon\Documents\hugo\MyBlog\public\post\ 
mkdir D:\Users\ThankSon\Documents\hugo\MyBlog\public\post #这两步主要是解决hugo增删文章不会改变`.\public\post\`内容的问题
cd D:\Users\ThankSon\Documents\hugo\MyBlog
hugo -D #将草稿也一起部署，不需要的可以删去
hugo
cd public
git init
git  add .
git commit -m "修改"
git push

PAUSE>NUL
```

{{% admonition info "提醒一下" %}}
文件创建完毕最好将文件的编码格式由`UTF-8`改为`ANSI`，否则命令窗标题会出现乱码，但不会影响输出结果
{{% /admonition %}}