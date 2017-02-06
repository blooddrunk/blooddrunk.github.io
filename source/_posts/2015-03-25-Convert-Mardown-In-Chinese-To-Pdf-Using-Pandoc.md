---
title: 利用Pandoc将中文编码Markdown转换为PDF
date: 2015-03-25 21:15:15
tags:
  - Markdown
---

[Pandoc](http://johnmacfarlane.net/pandoc/)是一款著名的文档格式的转换器。借助Pandoc可以使用markdown或textile等简单的标记语言写作，再转换成各种主流的文本格式。不过想把中文编码的markdown转换成的格式良好的PDF还需要一些额外的帮助。

<!-- more -->

### 安装(Windows为例)

[下载](http://johnmacfarlane.net/pandoc/installing.html)对应的操作系统版本并安装。确定安装成功：

```
pandoc --version
```

为了输出PDF还需要安装LaTex。Pandoc推荐[MiKTex](http://miktex.org/download)。转换时会自动获取需要的LaTex依赖包。

> Mac OS请在[下载页面](http://johnmacfarlane.net/pandoc/installing.html)获得帮助，我没有试过。Macbook空间太宝贵了。。

### 转换

现在已经可以像下面这样进行转换了(注意，Pandoc输入和输出都使用`UTF-8`编码):

```
pandoc test.markdown -o test.pdf
```

不过对于中文并没有这么简单。默认的LaText引擎pdflatex并不支持中文，我们需要在命令行中指定引擎为xelatex：`--latex-engine=xelatex`。此外还需要各种字体等配置才能正常的看到PDF。好在[tzengyuxio](https://github.com/tzengyuxio)提供了Latex[转换模板](https://github.com/tzengyuxio/pages/blob/gh-pages/pandoc/pm-template.latex)。我们也可以编辑此模板，改变字体和各种边距行距等。最终的命令如下：

```
pandoc resume.markdown -o resume.pdf --latex-engine=xelatex --template=pm-template.latex
```

这是这篇博文的PDF效果：

![poi](http://ww2.sinaimg.cn/mw690/6d325a28jw1eqhs9bhtqlj20ia0h8n21.jpg "md-to-pdf")