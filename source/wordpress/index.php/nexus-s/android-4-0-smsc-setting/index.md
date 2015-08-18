title: Android 4.0 smsc设置
id: 1239
date: 2012-03-06 11:41:06
---

今天突然发不了短信了，想到了可能是短信中心号码的问题

就上网搜里一下，发现android设置短信中心号码还没那么好设置

需要在*#*#4636#*#*里的手机信息里设置，而且需要格式转换

参考：[http://www.twit88.com/home/utility/sms-pdu-encode-decode](http://www.twit88.com/home/utility/sms-pdu-encode-decode)

反正移动的号码的话，把SMSC设置成0891683108100005F0，点更新就对了<span style="color: #ff0000;">（补充，这只适用于北京移动，如果是其他省份的，需要适当修改，格式为+861380xxxx500，其中XXXX修改成相应区号，不足4位的补0，比如北京010，就是0100，杭州是0571，其他类似）</span>

08表示长度为8个字节，91表示+号，后面其实就是8613800100500F，只是每个字节的顺序反了，f是填充。

----------------------------------

发了一条之后又发不了里，然后发现设置SMSC上面有个“需要打开IMS注册”和“启用通过IMS发送短信的功能”两个选项，勾上还真可以了。

然后就想还有个“打开LTE RAM 转储”是干嘛的呢？有待查证。

发现了这个帖子[http://forum.xda-developers.com/showthread.php?t=1318810](http://forum.xda-developers.com/showthread.php?t=1318810)，

不过公司电脑打不开，回去研究，先上班。