title: 编译最新版本DASHEncoder
id: 829
date: 2011-12-23 11:23:42
---

目前最新版本为November 05, 2011发布的版本

官网在这里：[http://www-itec.uni-klu.ac.at/dash/](http://www-itec.uni-klu.ac.at/dash/)

DASHEncoder的页面：[http://www-itec.uni-klu.ac.at/dash/?page_id=282](http://www-itec.uni-klu.ac.at/dash/?page_id=282)

DASHEncoder的依赖有[x264](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-x264/)，[ffmpeg](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-ffmpeg/)，[mp4box](http://lsharemy.com/wordpress/index.php/ubuntu/ubuntu-10-04/compile-the-latest-gpac/)和mysql client libraries，

前面三个都已经装好了，见相应的文章。

所以sudo apt-get install libmysqlclient-dev一下把mysql client libraries装好

直接make，就ok了