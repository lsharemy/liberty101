title: 用wondershaper限速
id: 1137
date: 2012-02-06 09:57:31
---

安装：apt-get install wondershaper

限速：wondershaper eth0 2000 240，第一个参数为接口名，后面两个数字分别代表下行和上行的速度

取消限速：wondershaper clear eth0

-----------------------------------------

发现这个限速效果很不理想，下次弄个NetEm玩玩

--------------------------------------

好吧，原来Linux系统自带有限速工具，叫tc，是包含在iproute2包中的流量控制工具，而NetEm是tc的一个扩展功能，专门用来模拟网络延时丢包等情况。

[http://www.linuxfoundation.org/collaborate/workgroups/networking/netem](http://www.linuxfoundation.org/collaborate/workgroups/networking/netem)

[http://lartc.org/howto/](http://lartc.org/howto/)

[http://tcn.hypert.net/tcmanual.pdf](http://tcn.hypert.net/tcmanual.pdf)