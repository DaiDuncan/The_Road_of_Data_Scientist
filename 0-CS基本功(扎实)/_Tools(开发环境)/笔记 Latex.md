[link: Latex 公式换行问题|等号对齐](https://blog.csdn.net/leichaoaizhaojie/article/details/53463598)

```latex
\begin{equation}
\begin{aligned}
......
\end{aligned}
\end{equation}
```

- 每个等号前有个`&`
- 换行处有个`\\`

$$
\DeclareMathOperator*{\argmin}{argmin}
\DeclareMathOperator*{\argmax}{argmax}

\begin{equation}
\begin{aligned}

\theta^{*},\theta ^{'*} 
&=  \argmin\limits_{\theta,\theta^{'}}\frac{1}{n}\sum_{n}^{i=1}L\left (\textbf{x}^{(i)},\textbf{x}^{'(i)}  \right )\\

&=\argmin\limits_{\theta,\theta^{'}}\frac{1}{n}\sum_{n}^{i=1}L\left (\textbf{x}^{(i)},g_{\theta ^{'}}\left ( f_{\theta }\left ( \textbf{x}^{i}\right )\right )\right )


\end{aligned}
\label{f2}
\end{equation}
$$







[link: argmax或者argmin中正下方参数的编写](https://blog.csdn.net/leichaoaizhaojie/article/details/53463730)

首先添加`\DeclareMathOperator*{\argmin}{argmin}`或者`\DeclareMathOperator*{\argmax}{argmax}`声明最小和最大问题。

之后在你公式中添加`\argmin\limits_{\theta,\theta^{‘}}`

- 其中\argmin就是显示argmin效果，
- 然后limits_{a,b}中a，b就是在argmin正下方要显示的参数

$$
\DeclareMathOperator*{\argmin}{argmin}
\DeclareMathOperator*{\argmax}{argmax}

\argmin\limits_{\theta,\theta^{'}}
$$



# 文献相关

[link: CSDN](https://blog.csdn.net/leichaoaizhaojie/category_6556882.html)

- 用bib文件添加参考文献
- 最后一行文献不能对齐
- bib参考文献的编译