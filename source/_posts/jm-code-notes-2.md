title: 'JM代码阅读之二指数哥伦布熵编码 | JM Code Notes 2 - Exp-Golomb Code'
tags:
  - codec
  - H.264/AVC
  - JM
id: 264
categories:
  - wordpress
  - index.php
  - csit
date: 2011-08-26 11:34:45
---

在标准文档的语法部分，每个语法元素都有一个描述符来规定该语法元素的编码类型。

它们的含义如下（见标准文档<span style="color: #ff0000;">7.2</span>小节）：

<!--more-->

<span style="color: #4d90fe;">ae(v)：上下文自适应算术熵编码语法元素。该描述符的解析过程在9.3节中规定。</span>
<span style="color: #4d90fe;">b(8)：任意形式的8比特字节。该描述符的解析过程通过函数read_bits( 8 )的返回值来规定。</span>
<span style="color: #4d90fe;"> ce(v)：左位在先的上下文自适应可变长度熵编码语法元素。该描述符的解析过程在9.2节中规定。</span>
<span style="color: #4d90fe;"> f(n)：n位固定模式比特串（由左至右），左位在先， 该描述符的解析过程通过函数read_bits( n )的返</span>
<span style="color: #4d90fe;"> 回值来规定。</span>
<span style="color: #4d90fe;"> i(n)：使用n比特的有符号整数。在语法表中，如果n是‘v’，其比特数由其它语法元素值确定。解析过程由函数read_bits(n)的返回值规定，该返回值用最高有效位在前的2的补码表示。</span>
<span style="color: #f18200;"> me(v)：映射的指数哥伦布码编码的语法元素，左位在先。解析过程在9.1中定义。</span>
<span style="color: #f18200;"> se(v)：有符号整数指数哥伦布码编码的语法元素位在先。解析过程在9.1中定义。</span>
<span style="color: #f18200;"> te(v)：舍位指数哥伦布码编码语法元素，左位在先。解析过程在9.1中定义。</span>
<span style="color: #4d90fe;"> u(n)：n位无符号整数。在语法表中，如果n是‘v’，其比特数由其它语法元素值确定。解析过程由函</span>
<span style="color: #4d90fe;"> 数read_bits(n)的返回值规定，该返回值用最高有效位在前的二进制表示。</span>
<span style="color: #f18200;"> ue(v)：无符号整数指数哥伦布码编码的语法元素，左位在先。解析过程在9.1中定义。</span>

其中，<span style="color: #f18200;">ue(v) me(v) se(v)</span>使用的是指数哥伦布编码(Exp-Golomb-coded)，<span style="color: #f18200;">te(v)</span>使用的是舍位指数哥伦布编码(trunated Exp-Golomb-coded)。

<span style="color: #f18200;">ue(v)</span>为无符号整数的指数哥伦布码编码，编码规则如下表所示：

![](http://localhostr.com/file/wHJDrZT/Exp-Golomb1.png "Exp-Golomb1")

下表是该编码的一些举例：

![](http://localhostr.com/file/DDAx0lj/Exp-Golomb2.png "Exp-Golomb2")

再拿上篇文章中SPS码流来实例分析：

00000000 00000000 00000000 00000001 01100111 <span style="color: #ff6600;">01000010</span> <span style="color: #808000;">00000000</span> <span style="color: #008000;">00101000</span> <span style="color: #008080;">1111</span><span style="color: #0000ff;">0011 0</span><span style="color: #00ff00;">0</span><span style="color: #ff00ff;">000101 1</span><span style="color: #00ffff;">0001001</span> <span style="color: #cc99ff;">1100</span>1000

<span style="color: #ff6600;">profile_idc = 66 (PROFILE_IDC_Baseline) （01000010）</span>
<span style="color: #808000;">constraint_set0_flag = 0 </span>
<span style="color: #808000;">constraint_set1_flag = 0 </span>
<span style="color: #808000;">constraint_set2_flag = 0 </span>
<span style="color: #808000;">constraint_set3_flag = 0 </span>
<span style="color: #808000;">reserved_zero_4bits = 0</span>
<span style="color: #008000;">level_idc = 40（00101000）</span>
<span style="color: #008080;">seq_parameter_set_id = 0 ue_v</span>
<span style="color: #008080;">log2_max_frame_num_minus4 = 0 ue_v</span>
<span style="color: #008080;">pic_order_cnt_type = 0 ue_v</span>
<span style="color: #008080;">log2_max_pic_order_cnt_lsb_minus4 = 0 ue_v</span>
<span style="color: #0000ff;">num_ref_frames = 5 ue_v</span>
<span style="color: #00ff00;">gaps_in_frame_num_value_allowed_flag = 0</span>
<span style="color: #ff00ff;">pic_width_in_mbs_minus1 = 10 (176) ue_v</span>
<span style="color: #00ffff;">pic_height_in_map_units_minus1 = 8 (144) ue_v</span>
<span style="color: #cc99ff;">frame_mbs_only_flag = 1 </span>
<span style="color: #cc99ff;">direct_8x8_inference_flag = 1 </span>
<span style="color: #cc99ff;">frame_cropping_flag = 0 </span>
<span style="color: #cc99ff;">vui_parameters_present_flag = 0</span>

可以看到，<span style="color: #008080;">值为0的uv_e元素被编码为1</span>，<span style="color: #0000ff;">值为5的uv_e元素被编码为00110</span>，<span style="color: #ff00ff;">值为10的ue_v元素被编码为0001011</span>，<span style="color: #00ffff;">值为8的ue_v元素被编码为0001001</span>。验证完毕:-D

<span style="color: #f18200;">se(v)</span>为有符号整数的指数哥伦布码编码，它通过下表简单的映射到无符号整数：

![](http://localhostr.com/file/uepVv9W/Exp-Golomb3.png "Exp-Golomb3")

<span style="color: #f18200;">te(v)</span>的规则如下：如果语法元素值为0，则编码为1，如果语法元素值为1，则编码为0，如果为其他大于1的值，则按<span style="color: #f18200;">ue(v)</span>进行编码。所以解码的时候需要先确定语法元素的长度，至于如何确定现在还不晓得，求赐教。

最后就是<span style="color: #f18200;">me(v)</span>啦，没理解错的话，它主要用来<span style="color: #ff0000;">coded_block_pattern</span>的编码，但现在<span style="color: #ff0000;">coded_block_pattern</span>是什么还不晓得，还得继续学习。

-------------------------------------------------------------------------------------------------------

JM代码中<span style="color: #f18200;">ue(v)</span>的编码由<span style="color: #f18200;">int ue_v (char *tracestring, int value, Bitstream *bitstream)</span>过程来实现。