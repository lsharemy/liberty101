title: Linxu下查找文件内容 find + grep
tags:
  - Linux
id: 915
categories:
  - wordpress
  - index.php
  - csit
date: 2011-12-26 11:09:52
---

<span style="color: #ff0000;">find . -type f -regex ".*\.cpp" -exec grep "RateBasedAdaptationLogic" {} \; -print</span>

<span style="color: #ff0000;">find</span>是查找文件

<span style="color: #ff0000;">.</span>表示在当前目录下查找（包括子目录）

<span style="color: #ff0000;">-regex ".*\.cpp"</span>是正则表达式，匹配所有.cpp文件

<span style="color: #ff0000;">-exec</span>是find的一个选项对每个文件执行相应的命令，这里的命令是<span style="color: #ff0000;">grep</span>，后面的<span style="color: #ff0000;">"RateBasedAdaptationLogic"</span>是grep的参数，即需要查找的文件内容，后面的{}是用来代替当前文件名的，<span style="color: #ff0000;">\;</span>就是结尾的分号，是-exec要求的（-exec command ;）。

最后的<span style="color: #ff0000;">-print</span>用来在查找到相应的内容后，打印出包含该内容的文件名。