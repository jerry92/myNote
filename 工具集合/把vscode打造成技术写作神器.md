作为技术开发，大家平时肯定需要记录技术笔记。甚至有的同学还开通可自己的技术博客或者技术公众号进行创作。

这个时候有套趁手的写作工具尤为重要，节省下时间好好休息一下，对于咱们程序员来说更加重要。因为最近在自己学习golang，为了找个顺手的IDE尝试了一下VScode，用后总结两个字：“真香”。集编码、写作、~~划水~~ 于一身。

话不多说，我们今天先说说写作这部分。

### 文字内容

文字写作推荐大家使用markdown。大家的经历应该主要放在文字内容上，可以节省大家的排版时间。虽然marakdown的语法非常简单，但是一款趁手的markdown编辑器也非常重要。因为今天的主角的VScode，所以大家可以直接在安装VScode中安装一下markdown插件即可，比如：

+ Markdown All in One
+ markdownlint

 ![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/markdownplugin.png)

现在大部分博客平台都支持markdown，可以直接讲我们写的内容复制过去，可惜的是公众号现在还不支持。所以要借助Md2all在线渲染网站，可以一键复制到公众号，并且还可以美化一些样式，比较简约美观一些。还可以选择不同主题或者自己改一些CSS样式，像下面这样：

![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/md2all.png)

### 图片内容

技术写作经常会画一些流程图，架构图。尤其图解会然让复杂的问题更加直观，更容易理解。所以有个方便的画图软件很重要。原来一直用processon在线画图，但是因为个人免费用户有文件个数限制，所以不太友好（还是因为穷，哈哈哈）。最近一直用draw.io，并且支持将文件自动存储到github，用起来很方便。尤其现在VScode还有对应的插件，写字、画图都在一个编辑器里，沉浸感更强了，防止思路再来回切软件时被打断。下面时再VScode里创建一个drawio文件：

![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/drawio.png)

画完图导出为图片，然后借助PicGo插件+github+jsDelivr（不了解的同学可以搜索一下，网上很多详细文档），自动上传图片到github图床并自动插入到markdown文件中。VScode也有PicGo对应的插件，所以这些操作也全再VScode中完成，很方便。

### 代码

> Talk is cheap. Show me the code.

对于文章中的代码，有人习惯用markdown本身的代码片段，也有人习惯放代码截图。但是截图很难截的统一大小，导致不太美观。这里推荐用插件Polacode-2020。

![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/polacode.png)

可以把代码保存为统一大小的图片，然后使用图片部分的插件，自动将代码图片插入到文章中。

最后我们再把上面提到的插件汇总一下，希望对你的写作有所帮助：

+ Markdown All in One
+ markdownlint
+ Draw.io intergration
+ PicGo
+ Polacode-2020

---

大家如果喜欢，记得关注哦。

![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/qrcode_for_gh_9c4eb6c9e91b_258.jpg)
