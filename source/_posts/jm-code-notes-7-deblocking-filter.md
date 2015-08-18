title: 'JM代码阅读之七去块效应滤波器 | JM Code Notes 7 – Deblocking Filter'
tags:
  - codec
  - H.264/AVC
  - JM
id: 327
categories:
  - wordpress
  - index.php
  - csit
date: 2011-09-15 11:45:28
---

![](http://localhostr.com/file/CJaZIzL/filter.jpg "filter")

参考：<span style="color: #ff0000;">标准文档8.7小节</span>，<span style="color: #ff0000;">经典论文</span>[Adaptive deblocking filter](http://ieeexplore.ieee.org/xpls/abs_all.jsp?arnumber=1218194)，<span style="color: #ff0000;">JM代码</span>

<!--more-->

滤波器被应用到每个解码块以减少由块效应所引起的失真。在编码端，进行反变换之后，再应用去块效应滤波器（在重建和存储宏块用于将来预测之前）；在解码端，在重建和显示之前，应用去块效应滤波器。<span style="color: #4d90fe;">滤波器平滑了块的边界，改善了解码帧的质量。</span>滤波后的宏块被用于将来帧的运动补偿预测，这能提高压缩性能，因为<span style="color: #4d90fe;">滤波过的图像比一个有块效应的未滤波的图像更接近原始图像</span>。滤波器的默认操作如下，编码端可以选择滤波器的强度或禁止使用滤波器。

滤波器被应用到每个宏块的4x4块的<span style="color: #4d90fe;">垂直</span>和<span style="color: #4d90fe;">水平</span>边界（除了块边界），如上图所示，顺序如下：

滤波亮度分量的4个垂直边界（顺序从左到右）
滤波亮度分量的4个水平边界（顺序从上到下）
滤波色度分量的2个垂直边界
滤波色度分量的2个水平边界

每个滤波操作影响边界两边的三个像素。把<span style="color: #4d90fe;">相邻块p和q</span>的垂直或水平边界两边的四个像素分别称为<span style="color: #4d90fe;">p3, p2, p1, p0, q0, q1, q2, q3</span>。滤波强度依赖于当前的量化器、相邻块的编码方式以及边界上图像像素的梯度。

<span style="color: #f18200;">JM代码中，DeblockFrame函数完成一帧图像的滤波，而它又调用DeblockMb对每一个宏块滤波（按光栅顺序进行）。</span>

<span style="color: #76b900;">边界强度Boundary-Strength (BS)：</span>

滤波输出的选择依赖于边界强度和边界上图像像素的梯度。<span style="color: #4d90fe;">边界强度BS依据下列原则进行选择</span>：
1.p和q有一个是Intra块，同时边界是宏块边界，则BS=4（最强的滤波强度)
2.p和q有一个是Intra块，同时边界不是宏块边界，则BS=3
3.p和q都不是intra块，同时其中一个块包含残差
4.p和q都不是intra块，p和q都不包含残差，同时，p和q使用不同的参考帧或两者的运动矢量差值大于一个亮度像素，则BS=1
5.否则BS=0
<span style="color: #4d90fe;">应用这些原则的结果是在可能有明显块效应失真的地方滤波强度很大。</span>

<span style="color: #f18200;">JM代码中，GetStrengthNormal用来获取一个宏块的垂直或水平边界的16个4x4块边界的BS。</span>

<span style="color: #76b900;">滤波器判决：</span>

集合<span style="color: #4d90fe;">(p2, p1, p0, q0, q1, q2)</span>的一组像素值在下列情况下才进行滤波：
1.<span style="color: #4d90fe;">BS &gt; 0</span>
2.<span style="color: #4d90fe;">|p0 -q0| &lt; α &amp;&amp; |p1 -p0| &lt; β &amp;&amp; |q1-q0| &lt; β</span>
α和β是标准中定义的阀值，它们随着两个块p和q的量化参数QP的增长而增长。<span style="color: #4d90fe;">滤波器选择的效果就是当在原始图像块边界有明显的变化（梯度）时，关掉滤波器。</span>当QP很小的时候，除了边界上的很小梯度，其他都应该是图像本身的特征（而不是块效应），所以应该保留，因此阀值α和β很小。当QP很大的时候，块效应可能很明显，所以α和β很大，需要滤波更多的边界像素点。

<span style="color: #76b900;">滤波器的实现：</span>

可以将滤波的模式按BS来分为两种，一种为<span style="color: #4d90fe;">普通的</span>（BS为1到3），一种的是<span style="color: #4d90fe;">强滤波</span>（BS = 4）

<span style="color: #4d90fe;">1.BS ∈ {1, 2, 3}</span>
输入是p1, p0, q0, q1，使用一个四阶滤波器，产生输出结果<span style="color: #4d90fe;">p'0</span>和<span style="color: #4d90fe;">q'0</span>。如果|p2 - p0| &lt; β，则使用另外一个四阶滤波器，即输入是p2, p1, p0, q0，输出是<span style="color: #4d90fe;">p'1</span>；如果|q2 - q0| &lt; β，使用一个四阶滤波器，即输入是q2, q1, q0, p0，输出是<span style="color: #4d90fe;">q'1</span>。

<span style="color: #4d90fe;">2.BS = 4</span>
如果|p2 - p0| &lt; β，|p0 - q0| &lt; round(α/4)，并且是亮度块，则：
<span style="color: #4d90fe;">p'0</span>是对p2, p1, p0, q0, q1进行五阶滤波得到的；<span style="color: #4d90fe;">p'1</span>是对p2 p1, q0, q0进行四阶滤波得到的；<span style="color: #4d90fe;">p'2</span>是对p3, p2, p1, p0, q0进行五阶滤波得到的。
其余情况下：
<span style="color: #4d90fe;">p'0</span>是对p1, p0, q1进行三阶滤波得到的。
边界的另外一边，即q块那边，滤波过程与p块是对称的，不再累赘。

<span style="color: #f18200;">JM代码中，EdgeLoopLumaNormal和EdgeLoopChromaNormal分别完成亮度块和色度块的滤波，具体实现请见代码，对照着[Adaptive deblocking filter](http://ieeexplore.ieee.org/xpls/abs_all.jsp?arnumber=1218194)中的设计一起看，会更容易理解:-D</span>

<span style="color: #76b900;">--------------------------------------------------------------------------------------------------------------------</span>

<span style="color: #ff0000;">滤波相关的语法元素：</span>

<span style="color: #4d90fe;">PPS</span>中的<span style="color: #4d90fe;">deblocking_filter_control_present_flag</span>，指示在<span style="color: #4d90fe;">slice header</span>中是否存在控制滤波的语法元素存在，1表示存在，0表示不存在。<span style="color: #f18200;">该实例中为0。</span>

<span style="color: #4d90fe;">slice header</span>中的<span style="color: #4d90fe;">disable_deblocking_filter_idc</span>，只有<span style="color: #4d90fe;">PPS</span>中的<span style="color: #4d90fe;">deblocking_filter_control_present_flag</span>为1的时候，该语法元素才存在，1表示不进行滤波，0表示进行frame级滤波，2表示进行slice级滤波（slice边界不进行滤波，这样各个slice的滤波可以并行），如果该语法元素不存在，则它为0。