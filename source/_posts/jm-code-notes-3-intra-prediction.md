title: 'JM代码阅读之三帧内预测 | JM Code Notes 3 – Intra Prediction'
tags:
  - codec
  - H.264/AVC
  - JM
id: 277
categories:
  - wordpress
  - index.php
  - csit
date: 2011-09-03 12:40:55
---

![](http://localhostr.com/file/uIz9e3u/intra.png "intra")

帧内预测依据先前已经编码并重建好的块（左上、上、右上、左）形成一个预测块P，当前块减去这个预测块，将差值进行编码。对于<span style="color: #4d90fe;">4x4</span>亮度块，有九种可选的预测模式；对于<span style="color: #4d90fe;">16x16</span>亮度块，有四种可选模式；对于色度块，有四种可选预测模式。最终选择使得P和当前块的差值最小的预测模式作为当前块的预测模式。

<!--more-->

还有一种帧内编码模式，即<span style="color: #4d90fe;">I_PCM</span>，使得编码器能够直接传输图像像素值（没有预测和变换）。在某些情况下（例如不规则的图像内容或很低的量化参数），这种方式可能比通常的帧间预测、变换、量化和熵编码过程更加有效。

JM代码中，默认会调用<span style="color: #f18200;">encode_one_macroblock_high</span>来对一个宏块编码，在该函数中：

首先，调用<span style="color: #f18200;">SetChromaPredMode</span>来进行色度块的模式选择，先做四种模式的预测，然后计算各模式的cost值（可先简单理解为预测块与当前块的差值），选择cost值最小的模式，这里给出一个实例，<span style="color: #ff0000;">使用JM16.0对foreman_part_qcif.yuv编码</span>（baseline），第一帧（I帧）的第1行第1列（从0开始计数）的宏块（见上图的选中宏块，<span style="color: #ff0000;">图中可以看出，该帧有4个宏块使用的16x16编码模式，其他都采用的4x4编码模式</span>），它的色度块的四种模式的cost值分别为：<span style="color: #4d90fe;">DC_PRED_8 - 392、HOR_PRED_8 - 404、VERT_PRED_8 - 416、PLANE_8 - 440</span>。DC模式的cost值最小，所以选择DC模式。

其次，会进行亮度块的模式选择，由于是Intra帧，要分别进行16x16、4x4、I_PCM三种编码模式的计算，选出一个最优编码模式。

<span style="color: #ff0000;">先进行16x16</span>，实现函数为<span style="color: #f18200;">Intra16x16_Mode_Decision_SAD</span>，过程与色度块类似，也是对4种模式进行预测，然后选择cost值最小的模式，最终的计算结果：<span style="color: #4d90fe;">VERT_PRED_16 - 10135、HOR_PRED_16 - 10405、DC_PRED_16 - 9523、PLANE_16 - 10292</span>。还的DC模式的cost模式最小，所以选DC模式作为16x16块的最优模式。然后，使用该模式对该宏块进行编码和重建，算出重建块和原始图像的差值平方和Sum of Squared Error（SSE），称为distortion，再算出编码该宏块需要的比特数，称为rate，并利用公式rdcost = (double)distortion + lambda * dmax(0.5,(double)rate)来计算一个rdcost，该值用来衡量该编码模式的优劣。这个实例中<span style="color: #4d90fe;">distortion为4984，rate为510，lambda 为34.269852557140545，rdcost 为22461.624804141677</span>。

<span style="color: #ff0000;">再进行4x4</span>，实现函数为<span style="color: #f18200;">Mode_Decision_for_4x4IntraBlocks_JM_High</span>，先对每个4x4块进行9种模式的预测，然后选择出一个cost值最小的模式，作为该4x4块的最优模式，完成所有16个4x4块的模式选择后，重建、编码、计算rdcost，这个实例中，<span style="color: #4d90fe;">distortion为4869，rate为351，lambda 为34.269852557140545，rdcost 为16897.718247556331</span>。由于该rdcost 比16x16的小，所以4x4取代16x16成为最佳编码模式。

<span style="color: #ff0000;">最后进行I_PCM</span>，仅仅是图像数据的复制，所以计算出来的distortion为0，而rate为3081，所以计算出<span style="color: #4d90fe;">rdcost 为</span>105585<span style="color: #4d90fe;">.41572855003</span>。

最终正如图像中显示的，4x4凭借其最低的rdcost 成为了该宏块的编码模式，最后就剩下把宏块数据写到NAL中去的过程了。实现函数为<span style="color: #f18200;">write_macroblock</span>。关于码流的语法规则，留到下次再更新。

哦，还忘了介绍一个工具<span style="color: #ff0000;">Elecard StreamEye</span>，上面的图就是用这个工具分析编码后的码流得到的。

一下是通过该工具分析得到的该宏块的信息：

position       : 1x1 (16x16) <span style="color: #4d90fe;">位置，第1行第1列（从0开始计数，所以左上角宏块为0x0）</span>
mb_addr        : 12 <span style="color: #4d90fe;">每行11个宏块，所以该宏块的地址为12（也是从0开始）</span>
size (in bits) : 351 <span style="color: #4d90fe;">该宏块所占的比特数</span>
mb_type        : 0 <span style="color: #4d90fe;">宏块类型，与slice的类型有关，对照表见标准文档7.4.5小节</span>
pmode          : 0 <span style="color: #4d90fe;">编码模式</span>
mb_type        : Intra(I_4x4) <span style="color: #4d90fe;">宏块类型的名称</span>
slice_number   : 0 <span style="color: #4d90fe;">slice的编号</span>
transform_8x8  : 0 <span style="color: #4d90fe;">是否8x8变换</span>
field\frame    : frame <span style="color: #4d90fe;">帧编码</span>
cbp bits       : 0 1101 1 00 0 00 <span style="color: #4d90fe;">这个有待研究</span>
:   0111    00    00
:   1111
:   1011
quant_param    : 28 <span style="color: #4d90fe;">QP值</span>
pmode          : Intra_4x4 <span style="color: #4d90fe;">编码模式的名称</span>
ipred Intra_4x4: <span style="color: #4d90fe;">每个4x4块的预测模式</span>
HorzUp       DiagDwnLeft  VertLeft     DiagDwnLeft
DC           DC           HorzUp       DiagDwnLeft
HorzUp       DC           HorzUp       HorzUp
HorzUp       HorzUp       HorzUp       HorzUp
ipred chroma   : DC <span style="color: #4d90fe;">色度块的预测模式</span>

<span style="color: #76b900;"> -------------------------------------------------------------------------------------------------------------------</span>

下面继续<span style="color: #f18200;">write_macroblock</span>的分析，里面最重要的一个函数为<span style="color: #f18200;">write_MB_layer</span>，这其实是一个函数指针，因为该宏块为Intra，所以最终调用的是<span style="color: #f18200;">writeMBLayerISlice</span>。该函数的作用就是填充<span style="color: #4d90fe;">Macroblock layer</span>语法结构了，关于该语法，请看标准文档的7.3.5小节。

先写入的是<span style="color: #4d90fe;">MBType</span>，表示该宏块的类型，这里为<span style="color: #4d90fe;">I4MB</span>。

然后写入每个块的预测模式，先是16个4x4亮度块的预测模式，再是色度块的预测模式，因为两个色度块的预测模式总是一样的，所以只写一次。

再写入<span style="color: #4d90fe;">CBP</span>和<span style="color: #4d90fe;">DQUANT</span>，<span style="color: #4d90fe;">CBP</span>即<span style="color: #4d90fe;">coded_block_pattern</span>，具体含义见7.4.5小节，<span style="color: #4d90fe;">DQUANT</span>即<span style="color: #4d90fe;">mb_qp_delta</span>，是当前QP和之前QP的一个差值，如果一样，则差值为0，好像没这么简单，想知道的还是看文档吧，嘿嘿，也在7.4.5小节。

最后写入的就是真正的残差数据了，残差数据是采用CAVLC编码的，该编码方法还有待研究。

<span style="color: #76b900;">-------------------------------------------------------------------------------------------------------------------</span>

CAVLC编码见[JM代码阅读之四残差变换系数块的CAVLC编码 | JM Code Notes 4 – CAVLC](http://lsharemy.com/wordpress/index.php/csit/jm-code-notes-4-cavlc/ "Permalink to JM代码阅读之四残差变换系数块的CAVLC编码 | JM Code Notes 4 – CAVLC")