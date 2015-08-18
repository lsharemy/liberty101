title: Linux下批量替换多个文件中的字符串
tags:
  - Linux
id: 1894
categories:
  - wordpress
  - index.php
  - csit
date: 2012-08-26 12:00:25
---

今天想把一个目录下的所有.mpd文件中的某一个路径修改成另外一个路径

但是.mpd文件很多，手动要修改好久，于是想办法用命令来完成

用的是find+grep+sed

很早就听说sed的强大了，但一直没用过，现在终于用到了

find . -type f -regex ".*\.mpd" -exec grep "/local/users/stledere/TestContent" {} \; -exec sed -i[bak] "s/\/local\/users\/stledere\/TestContent/\/var\/www\/OfForestAndMen/g" {} \;

这个就是我使用的命令

先用find找到后缀为.mpd的文件，然后用grep在这些文件中找到包含"/local/users/stledere/TestContent"的行，再然后用sed将"/local/users/stledere/TestContent"替换成"/var/www/OfForestAndMen"，其中-i[bak]中的[bak]表示需要备份，源文件将会保存一份，并增加[bak]后缀。