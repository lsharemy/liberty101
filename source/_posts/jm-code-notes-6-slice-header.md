title: 'JM代码阅读之六条带头语法 | JM Code Notes 6 – slice header'
tags:
  - codec
  - H.264/AVC
  - JM
id: 317
categories:
  - wordpress
  - index.php
  - csit
date: 2011-09-09 13:15:31
---

在“[JM代码阅读之三帧内预测 | JM Code Notes 3 – Intra Prediction](http://lsharemy.com/wordpress/index.php/csit/jm-code-notes-3-intra-prediction/ "Permalink to JM代码阅读之三帧内预测 | JM Code Notes 3 – Intra Prediction")”和“[JM代码阅读之五帧间预测 | JM Code Notes 5 – Inter Prediction](http://lsharemy.com/wordpress/index.php/csit/jm-code-notes-5-inter-prediction/ "Permalink to JM代码阅读之五帧间预测 | JM Code Notes 5 – Inter Prediction")”中，已经分析了<span style="color: #f18200;">macroblock_layer</span>的语法结构了，其实也就是分析完<span style="color: #f18200;">slice_data</span>了，下面继续往上层分析，也就是<span style="color: #f18200;">slice_header</span>语法。

<!--more-->

请打开文档<span style="color: #ff0000;">7.3.3</span>小节:-D

这一次直接看实例，嘿嘿，不要太开心

<span style="color: #f18200;">JM</span>中，产生<span style="color: #f18200;">slice_header</span>的函数为<span style="color: #f18200;">start_slice</span>。

先看第一帧的<span style="color: #f18200;">slice_header</span>：

first_mb_in_slice <span style="color: #4d90fe;">0</span>
slice_type <span style="color: #4d90fe;">I slice</span>
pic_parameter_set_id <span style="color: #4d90fe;">0</span> 指定使用的PPS
frame_num <span style="color: #4d90fe;">0</span>
idr_pic_id 0 标识一个IDR图像。一个IDR图像的所有条带中的idr_pic_id 值应保持不变。当按解码顺序的两个连续访问单元都是IDR 访问单元时，第一个IDR 访问单元的条带的idr_pic_id 值应与第二个IDR 访问单元的idr_pic_id 值不同。idr_pic_id 的值应在0到65535的范围内（包括0和65535）。
pic_order_cnt_lsb <span style="color: #4d90fe;">0</span> ？
dec_ref_pic_marking() 见<span style="color: #ff0000;">7.3.3.3</span>
no_output_of_prior_pics_flag <span style="color: #4d90fe;">0</span> 表示在解码图像缓冲器中先前解码的图像在一个IDR图像解码之后是如何处理的
long_term_reference_flag <span style="color: #4d90fe;">0</span> 等于0表示变量 MaxLongTermFrameIdx 被设为等于“无长期帧序号”而且该IDR图像被标记为“用于短期参考”。long_term_reference_flag 等于1表示变量MaxLongTermFrameIdx 被设为等于0，而且当前的IDR 图像被标记为“用于长期参考”并且LongTermFrameIdx 被分配为等于0。
slice_qp_delta <span style="color: #4d90fe;">2</span>(28-26) 表示用于条带中的所有宏块的QP的初始值，该值在宏块层将被mb_qp_delta 的值修改。

<span style="color: #f18200;">下面是第二帧的：</span>

first_mb_in_slice <span style="color: #4d90fe;">0</span>
slice_type <span style="color: #4d90fe;">P Slice</span>
pic_parameter_set_id <span style="color: #4d90fe;">0</span>
frame_num <span style="color: #4d90fe;">1</span>
pic_order_cnt_lsb <span style="color: #4d90fe;">2</span> ？
num_ref_idx_active_override_flag <span style="color: #4d90fe;">1</span> ？
num_ref_idx_l0_active_minus1 <span style="color: #4d90fe;">0</span>
ref_pic_list_reordering( ) <span style="color: #ff0000;">发现这个在最新的文档（2010/03）中已经没有了，囧</span>
ref_pic_list_reordering_flag_l0 <span style="color: #4d90fe;">0</span>
dec_ref_pic_marking()
adaptive_ref_pic_buffering_flag <span style="color: #4d90fe;">0</span>
slice_qp_delta <span style="color: #4d90fe;">2</span>(28-26)

<span style="color: #f18200;">第三帧，最后一帧：</span>

first_mb_in_slice <span style="color: #4d90fe;">0</span>
slice_type <span style="color: #4d90fe;">P Slice</span>
pic_parameter_set_id <span style="color: #4d90fe;">0</span>
frame_num <span style="color: #4d90fe;">2</span>
pic_order_cnt_lsb <span style="color: #4d90fe;">4</span>
num_ref_idx_active_override_flag <span style="color: #4d90fe;">1</span>
num_ref_idx_l0_active_minus1 <span style="color: #4d90fe;">1</span>
ref_pic_list_reordering()
ref_pic_list_reordering_flag_l0 <span style="color: #4d90fe;">0</span>
dec_ref_pic_marking()
adaptive_ref_pic_buffering_flag <span style="color: #4d90fe;">0</span>
slice_qp_delta <span style="color: #4d90fe;">2</span>(28-26)

<span style="color: #f18200;">好吧，还很多个语法元素不晓得什么意思，暂时就先这样了，等知道的时候再更新。</span>