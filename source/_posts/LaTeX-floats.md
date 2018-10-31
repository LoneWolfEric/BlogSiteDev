---
title: "LaTeX交叉引用&浮动体"
date: 2018-10-31
categories:  Others
tag: 
	- latex
	- 排版
	- 论文
thumbnail: /img/pexels/antique-book-encyclopedia-24576.jpg
toc: true
---

# 交叉引用

---

在书本中，我们都会看到：

由公式(3)可知，balabalabala，作图结果如图8所示。

![交叉引用](latex-floats/交叉引用.PNG)

当时惊艳于排版的人竟然有这等毅力给每个公式标号，还写是那个公式的号码。要是是我就写：如下面的公式所示，balabalabala。

现在学了$\LaTeX$之后，知道了这都是交叉引用的功劳。

代码：

```latex
首先观察这里的图 \ref{fig:raw}，肉眼可以发现左下角有一个残缺，在下方有一个更微小的残缺。

\begin{figure}[!hbtp]
	\centering
	\includegraphics[width=2.8in]{../result/raw.png}
	\caption{原图}\label{fig:raw}
\end{figure}
```

交叉引用的方式主要是使用`\label{}`、`\ref{}`和`\pageref{}`。`\label{}`给图片公式贴上标签，$\LaTeX$在编译的时候自动给予编号，`\ref{}`自动会被替换成和标签内容一样的图表公式的编号。`\pageref{}`会被替换图表公式所在的页码。四不四很棒！相比手动输入的方式，在之后编辑文档的时候如果突然插入或者产出一个公式，我们对于公式的标号并不需要手动更改，因为标签依然没变，$\LaTeX$会帮我们自动编号。

# 浮动体

---

$\LaTeX$一个强大的功能就是它的浮动体排版功能（然而我因为太菜了并不能体会到它的强大）。

刚开始的时候，很不习惯浮动体把我的图片乱排版，觉得表格应该直接放在图片后面。当拍了大量图片公式的文章之后，发现了浮动体的好还有自己对于科技文献排版的无知。

浮动体的哲学：文字应该和文字在一起，图片表格应该和图片表格在一起。大段的文字中间不应该随意突然出现图片，同样的，大量的图片中间不应该随意突然出现小段文字。

有的时候，表格和图片对于排版的分页也会造成很大的麻烦。所以，为了文字和图片表格的美观和排版分页的美观，避免不合理的分页或者大块的空白。我们需要将大块的内容移至别的地方。

在 $\LaTeX$ 中，默认有` figure` 和 `table` 两种浮动体，分别用于图片和表格。

主要的选项有！h t b p，分别对应功能：

- ! 表示忽略一些严格的限制条件
- h 表示如有可能，则放在当前位置
- t 表示该浮动体允许置于栏的顶部
- b 表示该浮动体允许置于栏的底部
- p 表示该浮动体允许置于浮动栏或浮动页

效果：

![不使用浮动体自动排序](latex-floats/不适用浮动体自动排序.PNG)

![使用浮动体自动排序](latex-floats/使用浮动体自动排序.PNG)

可以明显看出，不使用浮动体自动排序导致三张图片中间多了一句很小的话，不符合科技文献的排版美学和$\LaTeX$的美观哲学。

---

更加详细的介绍可以去看本博客的参考文档：

[浮动体：基础篇](http://www.latexstudio.net/archives/9786.html)

[浮动体：处理超宽问题](http://www.latexstudio.net/archives/9842.html)

[浮动体：浮动算法](http://www.latexstudio.net/archives/10043.html)

[在 LaTeX 中使用交叉引用](http://www.latexstudio.net/archives/3297.html)

[lshort](http://www.latexstudio.net/archives/5876.html)

