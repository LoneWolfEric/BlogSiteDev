---
title: "Latex小技巧"
date: 2018-10-10
categories:  Others
tag: 
	- latex
	- 排版
	- 论文
thumbnail: /img/pexels/book-glass.jpeg
toc: true
---

解决表格过宽，超过纸张边缘：

> \resizebox{\textwidth}{12mm}{ tabular环境 }

实例：

```latex
\begin{table}[htbp]
    \centering
    \resizebox{\textwidth}{12mm}{
        \begin{tabular}{|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|}
        \toprule
             变量 & $x_{11}$     & $x_{12}$     & $x_{13}$     & $x_{14}$     & $x_{21}$     & $x_{22}$     & $x_{23}$& $x_{24}$     & $d_{1}^{-}$     & $d_{1}^{+}$     & $d_{2}^{-}$     & $d_{2}^{+}$     & $d_{3}^{-}$     & $d_{3}^{+}$     & $d_{4}^{-}$     & $d_{4}^{+}$     & $d_{5}^{-}$     & $d_{5}^{+}$     & $d_{6}^{-}$     & $d_{6}^{+}$     & $d_{7}^{-}$     & $d_{7}^{+}$ \\
        \midrule
             计算结果 & 3    & 37    & 3     & 4     & 37    & 3    & 6     & 1     & 0     & 7    &0      & 50    & 0     & 23453 & 0     & 0     & 0     & 0     & 3     & 0     & 2     & 0 \\
        \bottomrule
        \end{tabular}}
    \caption{普通老板计算结果}
    \label{tab:addlabel}%
\end{table}%
```

---

两张小图并排

```latex
\begin{figure}[h]
	\begin{minipage}[t]{0.5\linewidth}
		\centering
		\includegraphics[width=2.2in]{../DIP/edge.jpeg}
		\caption{边缘检测后提取的部分圆周}
		\label{fig:side:a}
	\end{minipage}%
	\begin{minipage}[t]{0.5\linewidth}
		\centering
		\includegraphics[width=2.2in]{../DIP/final.jpeg}
		\caption{标注残缺的部分}
		\label{fig:side:b}
	\end{minipage}
\end{figure}
```

---

