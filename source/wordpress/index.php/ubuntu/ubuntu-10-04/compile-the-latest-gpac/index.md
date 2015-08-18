title: 编译最新版本GPAC
id: 826
date: 2011-12-23 11:14:19
---

因为要使用mp4box，所以要编一个GPAC。当前最新版本为0.4.5

官网在这里：[http://gpac.wp.institut-telecom.fr/](http://gpac.wp.institut-telecom.fr/)

编译教程在这里：[http://gpac.wp.institut-telecom.fr/2011/04/20/compiling-gpac-on-ubuntu/](http://gpac.wp.institut-telecom.fr/2011/04/20/compiling-gpac-on-ubuntu/)

安装好依赖包以后，再在/etc/ld.so.conf文件里加一行/usr/lib/xulrunner-1.9.2.24/

需要看一下目前电脑上有没有/usr/lib/xulrunner-1.9.2.24/这个目录，可能后面的版本号不一样。

sudo ldconfig更新一下。

在编译安装就没有问题啦。