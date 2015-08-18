title: Linux查看文件夹大小
tags:
  - Linux
id: 1019
categories:
  - wordpress
  - index.php
  - csit
date: 2012-01-10 14:30:12
---

刚从海哥那学来的一招：

du -sh dirname

du应该是disk usage的意思

-s是指summarize，计算目录的总大小，而不是所有子目录

-h是指human readable，以人类习惯的方式打印，如1K 234M 2G等