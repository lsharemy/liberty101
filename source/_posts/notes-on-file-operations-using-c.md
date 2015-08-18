title: 'c语言文件读写小记 | notes on file operations using C'
tags:
  - C
id: 247
categories:
  - wordpress
  - index.php
  - csit
date: 2011-08-24 09:27:15
---

![](http://localhostr.com/file/FpK2Tif/write.png "write")

昨天mentor叫我写个小工具，用来修改配置文件中的内容。结果在写文件的时候卡住了。思路就是在配置文件中找到需要设置的参数名，然后修改在它之后的参数值。但程序正确运行后，配置文件却没被修改。函数调用也没返回错误值。久久没找到原因。不过最后还是解决了，来作个记录。

<!--more-->

什么<span style="color: #4d90fe;">fwrite fputs fprintf</span>都用过了，<span style="color: #4d90fe;">fopen</span>的选项也一个一个试，<span style="color: #4d90fe;">r+ w+ a+</span>，虽然从<span style="color: #4d90fe;">MSDN</span>上看到应该用<span style="color: #4d90fe;">r+</span>，但再找不到原因的时候也只能相信奇迹（文档写错了）的存在了。

<span style="color: #4d90fe;">google</span>上搜搜也没找到解决办法，突然想起来看<span style="color: #4d90fe;">fopen</span>的文档的时候看到过一条注意，就是关于读完之后马上写的问题的。

翻出来一看：

<span style="color: #4d90fe;">When the "r+", "w+", or "a+" access type is specified, both reading and writing are allowed (the file is said to be open for "update"). However, when you switch between reading and writing, there must be an intervening fflush, fsetpos, fseek, or rewind operation. The current position can be specified for the fsetpos or fseek operation, if desired.</span>

恍然大悟，马上在读完之后加了一个<span style="color: #4d90fe;">fflush</span>，重新build，run，结果配置文件还是雷打不动。又被打击了一次。又想<span style="color: #4d90fe;">fflush</span>是<span style="color: #4d90fe;">flush</span>缓冲区用的，后面三个都是设置指针用的，是不是还应该重新设置一下指针呢，于是<span style="color: #4d90fe;">fseek</span>一下，再build，run，内牛满面啊，终于找到原因了。

不过这样的话文档那句话是不是不太正确呢？请大虾指教

<span style="color: #f18200;">总结：</span>

<span style="color: #f18200;">要用C语言对文件读写操作，既有读又有写的情况下，在读完之后要重新设置一下文件指针，再进行写操作，否则，写操作不生效。</span>