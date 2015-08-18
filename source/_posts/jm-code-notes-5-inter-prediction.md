title: 'JM代码阅读之五帧间预测 | JM Code Notes 5 – Inter Prediction'
tags:
  - codec
  - H.264/AVC
  - JM
id: 305
categories:
  - wordpress
  - index.php
  - csit
date: 2011-09-07 15:02:00
---

![](http://localhostr.com/file/EnTMvWX/inter.png "inter")

<span style="color: #ff0000;">老规矩，先原理，再实例</span>

帧间预测是采用基于块的运动补偿从一个或多个先前编码的图像帧中产生一个预测模型的。H.264与早起标准的主要不同之处在于支持不同的块尺寸（从<span style="color: #4d90fe;">16x16</span>到<span style="color: #4d90fe;">4x4</span>）以及支持精细子像素精度的运动矢量（亮度成分是<span style="color: #4d90fe;">1/4</span>像素精度）

<!--more-->

每个宏块（<span style="color: #4d90fe;">16x16</span>）的亮度分量可以按四种方式划分，即按一个<span style="color: #4d90fe;">16x16</span>块，或两个<span style="color: #4d90fe;">16x8</span>块，或两个<span style="color: #4d90fe;">8x16</span>块，或者4个<span style="color: #4d90fe;">8x8</span>块的划分进行运动补偿。如果选择<span style="color: #4d90fe;">8x8</span>模式，宏块中的4个<span style="color: #4d90fe;">8x8</span>子宏块可以用另一种方式进一步划分，或者作为一个<span style="color: #4d90fe;">8x8</span>块，或作为两个<span style="color: #4d90fe;">8x4</span>块，或作为两个<span style="color: #4d90fe;">4x8</span>块，或者作为四个<span style="color: #4d90fe;">4x4</span>块。

<span style="color: #f18200;">每个分块或者子宏块都产生一个单独的运动矢量。</span>每个运动矢量均需要编码和传输，同时分块模式信息需要进行编码并放在压缩比特流中。

<span style="color: #f18200;">每个色度块按照与亮度分量同样的分块方式进行划分。</span>

编码每个分块的运动矢量需要大量比特位。由于相邻块的运动矢量高度相关，所以每个块的运动矢量都是从邻近的先前编码块中进行预测得到的。<span style="color: #f18200;">当前运动矢量与预测运动矢量MVp的差值MVD被编码和传输。</span>

<span style="color: #4d90fe;">MVp的预测规则</span>如下：

<span style="color: #f18200;">假设E是当前宏块、子宏块或子宏块分块，A是E左边的分块或子分块，B是E上边的分块或子分块，C是E右上的分块或子分块。如果E左边的分块数大于1，则最上边的分块被选为A。如果E上边的分块数大于1，则最左边的分块被选为B。</span>

1.<span style="color: #f18200;">除了16x8和8x16</span>两种分块尺寸的其余传输块，MVp是分块A、B、C的运动矢量的中值（不是平均值）
2.对于<span style="color: #f18200;">16x8</span>分块，上边16x8分块的MVp是从B预测得到的，下边16x8分块的MVp是从A预测得到的。
3.对于<span style="color: #f18200;">8x16</span>分块，左边8x16分块的MVp是从A预测得到的，右边8x16分块的MVp是从C预测得到的。
4.对于<span style="color: #f18200;">skip</span>宏块，产生一个16x16块的MVp，和第1种情况一样。MVp的形成规则相应修改。

<span style="color: #f18200;">如果得不到一个或多个先前传输块的话（如，它在当前条带之外），则MVp的形成原则相应修改。</span>

--------------------------------------------------------------------------------------------------------------------

<span style="color: #ff0000;">Yeah! 又可以看实例了：</span>

这里对foreman_part_qcif.yuv的第二帧中地址为40的宏块（白色框框住，图贴在文章开头）进行分析，关键代码还是在<span style="color: #f18200;">encode_one_macroblock_high</span>中，由于该帧是P帧，所以会进行帧间预测。其中最重要的函数为<span style="color: #f18200;">BlockMotionSearch</span>，该函数为所有大小的分块完成运动搜索的过程，得到最优的MV。

1.进行<span style="color: #4d90fe;">skip模式</span>，实现函数<span style="color: #f18200;">FindSkipModeMotionVector</span>，该函数只是从周围块的MV来预测当前宏块的MVp，获得MVp的函数<span style="color: #f18200;">GetMotionVectorPredictorNormal</span>。由于skip宏块是没有MVD的，它把MVp作为运动矢量并得到运动补偿宏块。实例中获得的MVp为<span style="color: #ff0000;">(-17, 3)</span>

2.<span style="color: #4d90fe;">16x16模式</span>，也需要先获得MVp(-17, 3)，于是将(-16, 4)定为搜索中心（最近的整数像素），在一定的搜索范围（32）之内进行整像素搜索，需要搜索的位置有(32*2+1)*(32*2+1)=4225个。对于每个位置，都要计算一个motion cost（block_sad+mv_cost），最终找到使cost最小（2311）的MV(-16, 8)。然后在该点周围9个点（包括该点）再进行半像素搜索，找到一个cost最小（3639）的MV(-16, 6)，这里采用的误差度量不再是<span style="color: #ff0000;">SAD（Sum of Absolute Difference）</span>，而是<span style="color: #ff0000;">SATD（Sum of Absolute Transformed Difference）</span>，所以与之前的2311没可比性，这个配置文件里面可以配置，只不过默认的配置是整像素采用SAD，半像素和四分之一像素采用SATD。进行完半像素搜索后，再在cost最小的半象素点周围的9个点进行四分之一像素搜索，最后找到一个cost值最小（3523）的MV<span style="color: #ff0000;">(-17, 7)</span>。

3.<span style="color: #4d90fe;">16x8模式</span>，要对上下两个16x8块进行运动搜索，先是上面的块，MVp为(-17, 5)，于是从(-16, 4)开始进行整像素搜索，得到SAD最小的是(-24, 4)，半像素搜索(-26, 2)，四分之一像素搜索(-27,2)，最小cost为1262。然后是下面的块，类似得到最小cost（2020）的MV<span style="color: #ff0000;">(-15, 5)</span>。两个cost的和为1262+2020=3282。

4.<span style="color: #4d90fe;">8x16模式</span>，和16x8的区别就是现在的分块为左右两个，分别得到左块和右块的MV为<span style="color: #ff0000;">(-32, 3)</span>和<span style="color: #ff0000;">(-15, 5)</span>，cost为761+1825=2586。

5.<span style="color: #4d90fe;">8x8模式</span>，如上面所说，如果选择8x8模式，四个8x8子宏块可以用另外四种方式进行划分，所以需要对4个子宏块进行运动估计和模式选择。需要用RDO技术来选择。

先是<span style="color: #4d90fe;">第一个8x8子宏块</span>。SMB8x8模式，cost最小（483）的MV<span style="color: #ff0000;">(-30, 2)</span>，<span style="color: #ff0000;">rdcost = 1827.9051343856379</span>；8x4模式，上块和下块最优MV分别为(-32, 2)和(-32, 3)，cost为323+146=469，rdcost = 1839.9845446142001；4x8模式rdcost = 2071.8257241570759；4x4模式rdcost = 1906.9051343856379。选择rdcost最小的模式，也就是8x8模式，见图中高亮宏块的左上角8x8块。

<span style="color: #4d90fe;">第二个8x8子宏块</span>。8X8 rdcost = 2192.8257241570759；8x4 rdcost = 2337.8735125141839；<span style="color: #ff0000;"> 4x8 rdcost = 1486.6669036999515</span>，左块和右块MV分别为<span style="color: #ff0000;">(-21, 5)</span>、<span style="color: #ff0000;">(-15, 3)</span>，SATD cost = 413；4x4 rdcost = 1927.9051343856379。选择rdcost最小的模式，也就是4x8模式模式，见图中高亮宏块的右上角8x8块。

<span style="color: #4d90fe;">第三块和第四块</span>采用类似的方法，分别选择了SMB8x8模式和SMB4x4模式。见图中高亮宏块的左下和右上8x8块。

--------------------------------------------------------------------------------------------------------------------

<span style="color: #f18200;">所有帧间预测模式都做完了，下面就是要通过各个模式的rdcost来选择最佳模式了。</span>

skip模式，48032.539705114279

P16x16模式，13120.288152342357

P16x8模式，12449.192575628142

P8x16模式，10033.668325899660

<span style="color: #ff0000;">P8x8模式，8257.1286207853791</span>

即使是P帧，也还是要做帧内预测的，下面是帧内模式的rdcost

I16x16模式，16021.447683899336

I4x4模式，13026.446972799482

I_PCM模式，105585.41572855003

<span style="color: #f18200;">当然是选rdcost最小的咯，也就是P8x8，而四个8x8子宏块的分块情况也在之前选择好了。</span>

---------------------------------------------------------------------------------------------------------------------

之后还是把用<span style="color: #ff0000;">Elecard StreamEye</span>工具得到的该宏块的信息贴出来研究下：

position       : 7x3 (112x48)
mb_addr        : 40
size (in bits) : 146
mb_type        : 4 <span style="color: #4d90fe;">宏块类型，见标准文档表7-13，4表示的宏块类型名称为P_8x8ref0</span>
pmode          : 3 <span style="color: #4d90fe;">预测模式，8x8</span>
mb_type        : Inter(P_8x8ref0)
slice_number   : 0
transform_8x8  : 0
field\frame    : frame
cbp bits       : 0 1100 0 00 0 00
:   0000   00   00
:   0011
:   0001
quant_param    : 28 <span style="color: #4d90fe;">QP</span>
pmode          : Part_8x8 <span style="color: #4d90fe;">预测模式名称</span>
sub_pmode      : SubPart_8x8 SubPart_4x8 <span style="color: #4d90fe;">子宏块的分块方式</span>
: SubPart_8x8 SubPart_4x4
sub_pdir       : Pred_L0 Pred_L0 <span style="color: #4d90fe;">？</span>
: Pred_L0 Pred_L0
mvL0           :  <span style="color: #4d90fe;">MV，与之前分析的一致</span>
-30,   2, 0| -30,   2, 0| -21,   5, 0| -15,   3, 0
-30,   2, 0| -30,   2, 0| -21,   5, 0| -15,   3, 0
-33,   3, 0| -33,   3, 0| -26,   2, 0| -10,   3, 0
-33,   3, 0| -33,   3, 0| -26,   2, 0| -20,  43, 0

---------------------------------------------------------------------------------------------------------------------

下面分析码流，相关函数<span style="color: #f18200;">writeMBLayerPSlice</span>

先写入的是一个<span style="color: #4d90fe;">mb_skip_run</span>语法元素，表示之前有多少个skip宏块，这里为0，因为之前的宏块不是skip空块

再写入<span style="color: #4d90fe;">MBType</span>，表示宏块类型

然后是4个8x8<span style="color: #4d90fe;">子宏块的模式</span>

之后写入的是<span style="color: #4d90fe;">运动矢量信息</span>

再然后写入<span style="color: #4d90fe;">cbp和DQuant</span>

最后写如的是<span style="color: #4d90fe;">亮度和色度的残差</span>

该实例中，mb_skip_run占<span style="color: #ff0000;">1</span>bit，MBType占<span style="color: #ff0000;">5</span>bits，子宏块的模式占<span style="color: #ff0000;">10</span>bits，运动矢量信息占<span style="color: #ff0000;">82</span>bits，cbp占<span style="color: #ff0000;">9</span>bits，DQuant占<span style="color: #ff0000;">1</span>bit，亮度残差占<span style="color: #ff0000;">38</span>bits，色度残差为空，一共1+5+10+82+9+1+38=<span style="color: #ff0000;">146</span>bits，与上面一致，忽忽

其中运动矢量信息的编码函数为<span style="color: #f18200;">write_pslice_motion_info_to_NAL</span>，编码每个块的最佳MV和MVp的差值MVD。