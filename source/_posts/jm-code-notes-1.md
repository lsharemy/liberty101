title: 'JM代码阅读之一SODB RBSP EBSP NALU | JM Code Notes 1 - SODB RBSP EBSP NALU'
tags:
  - codec
  - H.264/AVC
  - JM
id: 252
categories:
  - wordpress
  - index.php
  - csit
date: 2011-08-24 12:50:54
---

<span style="color: #f18200;">JM版本16.0，配置文件encoder_baseline.cfg，H.264标准文档（03/2010）版。</span>

通过对码流的第一个NALU（SPS）的形成来分析。

首先给出编码后的最终码流（<span style="color: #ff0000;">SPS</span> + PPS）：
<span style="color: #ff0000;">00 00 00 01 67 42 00 28 F3 05 89 C8</span> 00 00 00 01 68 C9 4A 38 80

将<span style="color: #ff0000;">SPS（红色部分）</span>转换成二进制：00000000 00000000 00000000 00000001 01100111 <span style="color: #ff6600;">01000010</span> <span style="color: #808000;">00000000</span> <span style="color: #008000;">00101000</span> <span style="color: #008080;">1111</span><span style="color: #0000ff;">0011 0</span><span style="color: #00ff00;">0</span><span style="color: #ff00ff;">000101 1</span><span style="color: #00ffff;">0001001</span> <span style="color: #cc99ff;">1100</span>1000

<!--more-->

然后介绍一个码流分析工具：Elecard StreamEye Tools

用这个工具分析用JM编码得到的码流，它会给出各个NALU的信息

其中SPS的内容如下：
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

其中每一个参数对应码流中的位置用颜色对应关系给出，其中后面标有ue_v的是采用Exp-Golomb-coded编码的，暂时还没有研究。其他没有颜色的bit为一些填充或头部，后面详细分析。

---------------------------------------------------------------------------------------------------

<span style="color: #4d90fe;">好吧，下面分析这个NALU是怎么形成的：<span style="color: #00ffff;">00 00 00 01 </span><span style="color: #ff0000;"><span style="color: #00ffff;">67</span> 42 00 28 F3 05 89 C</span><span style="color: #ff00ff;">8</span></span>

<span style="color: #4d90fe;">首先形成的是<span style="color: #ff0000;">String Of Data Bits (SODB)</span>，请参考标准文档7.2.3.1.1部分</span>

<span style="color: #ff0000;">01000010 00000000 00101000 11110011 00000101 10001001 1100</span>

<span style="color: #4d90fe;">这个就是形成的<span style="color: #ff0000;">SODB</span>，转换成16进制，可以发现它就是上面码流的<span style="color: #ff0000;">42 00 28 F3 05 89 C</span>这一段。</span>

<span style="color: #4d90fe;">然后要形成的是<span style="color: #ff00ff;">Raw Byte Sequence Packet (RBSP)</span>，它其实就是在SODB后面加上**<strong>**</strong></span>

<span style="color: #4d90fe;"><span style="color: #ff00ff;">RBSP trailing bits</span>的结果，见标准文档7.2.3.1，目的是为了形成整数字节。</span>

<span style="color: #4d90fe;">填充规则见标准文档的7.4.1部分，大概为先填充一个1（rbsp_stop_one_bit），然后都填充0（rbsp_alignment_zero_bit），所以对于上面的SODB，填充一个1，3个0之后，便得到了</span>
<span style="color: #4d90fe;">01000010 00000000 00101000 11110011 00000101 10001001 1100<span style="color: #ff00ff;">1000</span></span>
<span style="color: #4d90fe;">即42 00 28 F3 05 89 C<span style="color: #ff00ff;">8</span></span>

<span style="color: #4d90fe;">现在，码流的后面7个字节都得到了，现在要得到的是<span style="color: #00ff00;">Extended Byte Sequence Packet (EBSP)</span>，它在RBSP基础上填加了仿校验字节，防止与起始码冲突，如果出现连续的三个字节00000000 00000000 000000xx，着插入一个0x03，变成00000000 00000000 00000003 000000xx。在上面的RBSP中没有出现这样的序列，所以木有改变什么。</span>

<span style="color: #4d90fe;">最后在EBSP前面加上一个4字节的起始码00 00 00 01和一个NAL unit type字节就形成最后的<span style="color: #00ffff;">Network Abstraction Layer Unit (NALU)</span></span>

<span style="color: #4d90fe;">NAL unit type字节</span><span style="color: #4d90fe;">包含三个字段（具体含义见7.4.1）：<span style="color: #00ffff;">67 &lt;==&gt; 0 11 00111</span></span>
<span style="color: #4d90fe;">forbidden_zero_bit，总为<span style="color: #00ffff;">0</span></span>
<span style="color: #4d90fe;">nal_ref_idc，2个bit，表示该NAL的重要性，是00的话，说明它可以被安全的丢弃，这里SPS的这个指为3（<span style="color: #00ffff;">11</span>），即最高值。参考[<span style="color: #4d90fe;">RFC 3984</span>](http://www.apps.ietf.org/rfc/rfc3984.html)。（现在知道这个字节叫作NAL unit type octet了）</span>
<span style="color: #4d90fe;">nal_unit_type，5个bit，在7.4.1中的table 7-1中有说明。这里值为7（<span style="color: #00ffff;">00111</span>），表示NAL中是SPS，验证成功:-D</span>

--------------------------------------------------------------------------------------------------------------------

<span style="color: #f18200;">在JM代码中，输出SPS和PPS的实现在函数int start_sequence(ImageParameters *p_Img, InputParameters *p_Inp)中，有兴趣的小朋友自己研究研究吧。</span>

最后把PPS的信息也贴出来：

pic_parameter_set_id = 0
seq_parameter_set_id = 0
entropy_coding_mode_flag = 0
pic_order_present_flag = 0
num_slice_groups_minus1 = 0
num_ref_idx_L0_active_minus1 = 4
num_ref_idx_L1_active_minus1 = 4
weighted_pred_flag = 0
weighted_bipred_idc = 0
pic_init_qp_minus26 = 0
pic_init_qs_minus26 = 0
chroma_qp_index_offset = 0
deblocking_filter_control_present_flag = 0
constrained_intra_pred_flag = 0
redundant_pic_cnt_present_flag = 0