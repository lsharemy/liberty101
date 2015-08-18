title: 'JM代码阅读之四残差变换系数块的CAVLC编码 | JM Code Notes 4 – CAVLC'
tags:
  - codec
  - H.264/AVC
  - JM
id: 294
categories:
  - wordpress
  - index.php
  - csit
date: 2011-09-05 18:59:51
---

开始研究<span style="color: #4d90fe;">CAVLC</span>编码，参考：<span style="color: #ff0000;">JM16.0</span>代码、标准文档的<span style="color: #ff0000;">9.2</span>小节

还是使用上篇Notes（[JM代码阅读之三帧内预测 | JM Code Notes 3 – Intra Prediction](http://lsharemy.com/wordpress/index.php/csit/jm-code-notes-3-intra-prediction/ "Permalink to JM代码阅读之三帧内预测 | JM Code Notes 3 – Intra Prediction")）中的那个宏块来进行研究。

先讲<span style="color: #4d90fe;">CAVLC</span>编码变换系数块的过程。再通过分析之前那个宏块来加深理解。

<!--more-->

<span style="color: #4d90fe;">CAVLC</span>编码变换系数块（4x4的残差块）的过程如下：

1.编码<span style="color: #4d90fe;">coeff_token</span>（<span style="color: #4d90fe;">Totalcoeff</span> &amp; <span style="color: #4d90fe;">TrailingOnes</span>），对应的JM函数为<span style="color: #f18200;">writeSyntaxElement_NumCoeffTrailingOnes</span>

<span style="color: #4d90fe;">Totalcoeff</span>表示非零系数的总数，<span style="color: #4d90fe;">TrailingOnes</span>表示非零系数中±1的个数。4x4块中一共有16个系数，所以<span style="color: #4d90fe;">Totalcoeff</span> 的范围是0-16，而<span style="color: #4d90fe;">TrailingOnes</span>的范围是0-3，如果±1的个数大于3，则只有最后的3个作为特殊情况处理，其他的则作为正常系数编码。

而<span style="color: #4d90fe;">coeff_token</span>则是使用<span style="color: #4d90fe;">Totalcoeff</span> 和<span style="color: #4d90fe;">TrailingOnes</span>当索引通过查表得到的。查找表的选择还依赖一个系数<span style="color: #4d90fe;">nC</span>，它取决于左边和上边已编码块（分别称为nA和nB）的<span style="color: #4d90fe;">Totalcoeff</span>。nC计算如下：如果上边和左边块nB和nA都可以得到，则<span style="color: #4d90fe;">nC</span> = round((nA+nB)/2)；如果之后上边块可以得到，<span style="color: #4d90fe;">nC</span> = nB；如果只有左边块可以得到，则<span style="color: #4d90fe;">nC</span> = nA；如果两个块都得不到，则nC = 0。

有了<span style="color: #4d90fe;">Totalcoeff</span> 、<span style="color: #4d90fe;">TrailingOnes</span>以及<span style="color: #4d90fe;">nC</span>，就可以通过查表得到<span style="color: #4d90fe;">coeff_token</span>了。见标准文档的表9-5。

2.编码每个<span style="color: #4d90fe;">TrailingOnes</span>的符号，对应的JM函数为<span style="color: #f18200;">writeSyntaxElement_VLC</span>

对于步骤1中的每个<span style="color: #4d90fe;">TrailingOne</span>（±1），分别编码它们的符号（0代表正，1代表负），从最高频的<span style="color: #4d90fe;">TrailingOne</span>开始。

3.编码余下非零系数的级别（符号和幅度），也按倒序进行，即从高频开始，对应的JM函数为<span style="color: #f18200;">writeSyntaxElement_Level_VLC1</span>和<span style="color: #f18200;">writeSyntaxElement_Level_VLCN</span>，前者用来编码第一个非零系数，后者则是编码除第一个以外的非零系数。

每个级别的码字包含一个前缀（<span style="color: #4d90fe;">prefix</span>）和后缀（<span style="color: #4d90fe;">suffix</span>）。后缀的长度（<span style="color: #4d90fe;">suffixLength</span>）可以是0到6比特，而<span style="color: #4d90fe;">suffixLength</span>随着每个连续编码级别的幅度自适应变化。较小的<span style="color: #4d90fe;">suffixLength</span>使用于较低幅度的级别，而较大的<span style="color: #4d90fe;">suffixLength</span>适用于较大幅度的级别。

具体的编码算法比较复杂，可以参考标准文档的<span style="color: #ff0000;">9.2.2</span>小节表述的解析过程，从而推出其编码过程。

4.最高频非零系数前零的总数，对应的JM函数为<span style="color: #f18200;">writeSyntaxElement_TotalZeros</span>

5.编码每个零游程，即每个非零系数前零的个数（<span style="color: #4d90fe;">run_before</span>），也是按倒序进行。对应的JM函数为<span style="color: #f18200;">writeSyntaxElement_Run</span>

该步骤有2个例外，1.如果余下的已经没有零需要被编码，则没有必要编码任何其他的<span style="color: #4d90fe;">run_before</span>；2.没有必要为最后一个非零系数编码<span style="color: #4d90fe;">run_before</span>值。

--------------------------------------------------------------------------------------------------------------------

下面是实例了：

该宏块（地址为12）的第一个4x4残差变换系数块经过<span style="color: #4d90fe;">Zigzag</span>顺序扫描之后：

<span style="color: #f18200;">1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0</span>

根据上面的步骤

1.可以得到<span style="color: #4d90fe;">Totalcoeff</span> 为1，<span style="color: #4d90fe;">TrailingOnes</span>为1，nA和nB要从邻居得到分别为0和1，所以计算得<span style="color: #4d90fe;">nC</span>为1，查表得<span style="color: #4d90fe;">coeff_token</span>为<span style="color: #ff0000;">01</span>。

2.<span style="color: #4d90fe;">TrailingOnes</span>只有一个，而且是正的，所以该步骤的编码为<span style="color: #ff0000;">0</span>

3.没有其他非零系数，所以该步骤不产生编码

4.最高频非零系数前零的总数为0，VCL编码结果为<span style="color: #ff0000;">1</span>

5.因为只有一个非零系数，而最后一个非零系数不需要编码<span style="color: #4d90fe;">run_before</span>值，所以该步骤也不产生编码。

所以该4x4块的编码为<span style="color: #ff0000;">0101</span>

--------------------------------------------------------------------------------------------------------------------

第二个4x4残差变换系数块：

<span style="color: #f18200;">0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0</span>

同样的步骤，<span style="color: #4d90fe;">Totalcoeff</span> 为1，<span style="color: #4d90fe;">TrailingOnes</span>为1，<span style="color: #4d90fe;">nC</span>为1，步骤1编码为<span style="color: #ff0000;">01</span>，步骤2编码为<span style="color: #ff0000;">0</span>，步骤3跳过，最高频非零系数前零的总数为1，所以步骤4编码为<span style="color: #ff0000;">011</span>，步骤5跳过，所以最终的编码为<span style="color: #ff0000;">011011</span>

--------------------------------------------------------------------------------------------------------------------

第三个4x4残差变换系数块：

<span style="color: #f18200;">0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0</span>

<span style="color: #4d90fe;">Totalcoeff</span> 为0，<span style="color: #4d90fe;">TrailingOnes</span>为0，nC为1，步骤1编码为<span style="color: #ff0000;">1</span>，之后的步骤均不产生编码，所以最终编码为<span style="color: #ff0000;">1</span>

--------------------------------------------------------------------------------------------------------------------

第四个4x4残差变换系数块：

<span style="color: #f18200;">-5, 2, 5, -2, -2, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0</span>

<span style="color: #4d90fe;">Totalcoeff</span> 为8，<span style="color: #4d90fe;">TrailingOnes</span>为3，nC为1，步骤1编码为<span style="color: #ff0000;">0000 0001 00</span>，步骤2编码为<span style="color: #ff0000;">000</span>（三个都为正），步骤3中，第一个-2编码为<span style="color: #ff0000;">0001</span>，第二个-2编码为<span style="color: #ff0000;">01 1</span>，5编码为<span style="color: #ff0000;">00001 0</span>，2编码为<span style="color: #ff0000;">1 10</span>，-5编码为<span style="color: #ff0000;">001 01</span>。最高频非零系数前零的总数为4，步骤4编码为<span style="color: #ff0000;">11</span>。步骤5中，最后一个1前面有0个0，编码为<span style="color: #ff0000;">11</span>，倒数第二个1前面也是0个0，编码为<span style="color: #ff0000;">11</span>，倒数第三个1前面有4个零，编码为<span style="color: #ff0000;">000</span>，由于前面不再含零，编码结束。最终编码为<span style="color: #ff0000;">0000 0001 0000 0000 1011 0000 1011 0001 0111 1111 000</span>。一共43bits

--------------------------------------------------------------------------------------------------------------------

之后的12个4x4块也是类似的编码，色度块的编码也是类似，最终使用的比特数为<span style="color: #ff0000;">295</span>（亮度）+<span style="color: #ff0000;">7</span>（色度）=<span style="color: #ff0000;">302</span>。这个数跟Notes 4中用Elecard StreamEye分析工具看到的宏块所占的比特数<span style="color: #ff0000;">351</span>还有一点差异，不过正如Notes 3中分析的，在写入残差数据之前，还写入了一些其他数据，包括<span style="color: #4d90fe;">MBType</span>、<span style="color: #4d90fe;">CBP</span>、<span style="color: #4d90fe;">DQUANT</span>、以及每个块的<span style="color: #4d90fe;">预测模式</span>等信息。

那就再做个加法验证一下吧，<span style="color: #4d90fe;">MBType</span>占<span style="color: #ff0000;">1</span>bit，<span style="color: #4d90fe;">预测模式</span>占<span style="color: #ff0000;">44</span>bits（亮度43bits，色度1bit），<span style="color: #4d90fe;">CBP</span>占<span style="color: #ff0000;">3</span>bits，<span style="color: #4d90fe;">DQUANT</span>占<span style="color: #ff0000;">1</span>bit，302+1+44+3+1=<span style="color: #ff0000;">351</span>bits，完全符合，嘿嘿，验证完毕。