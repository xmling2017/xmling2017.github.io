因为笔者经常使用Markdown和LaTeX两种排版工具，所需要的编辑软件主要是**sublime**和*VS Code*，因为sublime更加的轻量化，对于短文章的编辑十分方便，而且sublime同样支持插件，只不过安装比较麻烦。官网上下载的sublime仍可当作免费使用，但时不时会跳出购买提示，比较恼人，笔者浏览网站，找到破解购买提示的方法，在此与大家分享，希望各位不要用于商业用途。如果失效，也请告知笔者。
<!--more-->
## sublime text3 的安装

1. [官网下载](sublimetext.com/3)
2. `install package`的安装. 建议手动安装, 安装包见qq文件, 也可以到百度云(137...)下载. 打开默认设置文件, 删除原来的channels链接, 重新将准备好的文件[^1]放到目录文件夹中,  并输入该文件的根目录[^2]
3. 汉化---点击`ctrl+shift+p->install package->chineselocalization`
4. 安装**terminus**, 方式如上. 快捷键设置, 打开[网址](packagecontrol.io/packages/terminus), 复制`toggle terminal panel`中的代码到快捷键设置文件中粘贴即可
5. 关于latex的插件, 推荐latextool latexyz latex-cwl

基本的默认设置
```
{
	"auto_complete_triggers":
	[
		{
			"characters": "<",
			"selector": "text.html"
		},
		{
			"characters": "\\",
			"selector": "text.tex.latex"
		}
	],
	"color_scheme": "Packages/Color Scheme - Default/Monokai.sublime-color-scheme",
	"font_face": "Bookman Old Style",
	"font_options":
	[
		"gray_antialias",
		"subpixel_antialias"
	],
	"font_size": 13,
	"hide_minimap": true,
	"ignored_packages":
	[
		"Vintage"
	],
	"theme": "Adaptive.sublime-theme",
	"update_check": false,
	"word_wrap": true
}
```

关于sublime 主题的设置
推荐ayu, 在install键入`ayu`即可安装, 默认下载icon图标. [图标网址](packagecontrol.io/packages/A_File_icon)

### 快捷键的介绍
|      快捷键      |             功能             |
| :--------------: | :--------------------------: |
|      ctrl+p      |   文件夹内搜索, +@功能如下   |
|      ctrl+r      |         文章结构搜索         |
|   ctrl+g:行号    |          功能:跳转           |
|    shift+f11     | 专注模式(类似于vscode禅模式) |
|  alt+shift+数字  |             分栏             |
| ctrl+shift+enter |    光标所在行上方添加一行    |


### 关于latextool快捷键的介绍

|      快捷键      |             功能             |
| :--------------: | :--------------------------: |
|     ctrl+l,v     |           打开pdf            |
|     ctrl+l,j     |           正向搜索           |
| ctrl+l,backspace |         清除辅助文件         |
|     ctrl+l,c     |          扩展呈命令          |
|     ctrl+l,e     |          扩展成环境          |
|  ctrl+l,ctrl+e   |             强调             |
|  ctrl+l,ctrl+b   |             加粗             |
|  ctrl+l,ctrl+n   | 选中的文本被包含在命令环境下 |
|     ctrl+l,w     |           字数统计           |


## sublime的注册码的更新
1. notepad++的下载
2. 在notepad++ 点击`插件管理`下载`hex`
3. 打开目录`C:\Windows\System32\drivers\etc`, 修改**hosts**文件(建议保存复件)
4. 在hosts最下方添加
```
127.0.0.1 www.sublimetext.com
127.0.0.1 license.sublimehq.com
```
5. 修改sublime_text3.exe文件(建议保存复件)
6. 将文件移到notepad++, 使用hex插件的`view in hex`, 搜索`97 94 0d`, 点击修改成`00 00 00`
7. 打开sublime, 将下列注册码复制, 粘贴. 即可注册成功

```
----- BEGIN LICENSE -----
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
------ END LICENSE ------
```

[^1]: [channel文件的网址](packagecontrol.io/channel_v3.json), ctrl+s 保存即可
[^2]: 在`.json`文件中, 目录的划线只能是左滑线`/`或是双右划线`\\`