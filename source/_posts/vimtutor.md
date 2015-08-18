title: vimtutor
tags:
  - Linux
  - vim
id: 1453
categories:
  - wordpress
  - index.php
  - csit
date: 2012-05-21 12:47:46
---

学习下vim，顺被做个记录，方便以后参考

vimtutor是vim自带的一个教程，本文就是<!--more-->对vimtutor的一个记录

只要装了vim，在终端里敲vimtutor就可以打开这个教程啦，windows下的vim装好后，有一个专门的快捷方式指向vimtutor，可以从那里打开

<span style="color: #ff0000;">Lesson 1.1:  MOVING THE CURSOR</span>

教你怎么移动光标，h左，j下，k上，l右，移多就习惯了

<span style="color: #ff0000;">Lesson 1.2: EXITING VIM</span>

教用:q!来退出vim（不保存）

<span style="color: #ff0000;">Lesson 1.3: TEXT EDITING - DELETION</span>

教你用x来删除单个字符

<span style="color: #ff0000;">Lesson 1.4: TEXT EDITING - INSERTION</span>

教你用i来插入文本

<span style="color: #ff0000;">Lesson 1.5: TEXT EDITING - APPENDING</span>

教你用A来追加文本

<span style="color: #ff0000;">Lesson 1.6: EDITING A FILE</span>

教你用:wq来保存一个文件

<span style="color: #ff0000;">Lesson 2.1: DELETION COMMANDS</span>

教你用dw(delete word)来删除一个单词

<span style="color: #ff0000;">Lesson 2.2: MORE DELETION COMMANDS</span>

教你用d$来删除到行末

<span style="color: #ff0000;">Lesson 2.3: ON OPERATORS AND MOTIONS</span>

解释了之前的删除命令，由两部分组成(d motion)，d表示一个操作operator，而motion表示运动(有w, e, $三种)，dw删除知道下一个单词的开始，de删除直到当前单词结束，d$则表示删除知道行末

同时，w, e, $本身可以单独使用，用来移动光标

<span style="color: #ff0000;">Lesson 2.4: USING A COUNT FOR A MOTION</span>

motion还可以结合count使用，比如2w向前移动两个单词，3e移动到第三个单词的结尾，0可以移动到一行的开头

<span style="color: #ff0000;">Lesson 2.5: USING A COUNT TO DELETE MORE</span>

motion和count的组合还可以于d一起使用，格式为：

d   number   motion

比如d2w删除2个单词

<span style="color: #ff0000;">Lesson 2.6: OPERATING ON LINES</span>

教你用dd来删除一行，2dd来删除2行

<span style="color: #ff0000;">Lesson 2.7: THE UNDO COMMAND</span>

教你用u来撤销undo一次操作，用U来撤销应用于整行的操作，用CTRL-R来重做redo

<span style="color: #ff0000;">Lesson 3.1: THE PUT COMMAND</span>

教你用p来粘贴之前dd删掉的行，注：p会把行放在光标的前面

<span style="color: #ff0000;">Lesson 3.2: THE REPLACE COMMAND</span>

教你用r来替换单个字符，格式为rx，将光标所在位置的字符替换为x

<span style="color: #ff0000;">Lesson 3.3: THE CHANGE OPERATOR</span>

教你用ce来修正单个单词

<span style="color: #ff0000;">Lesson 3.4: MORE CHANGES USING c</span>

c和d一样，可以用以下的格式：

c [number] motion

来修正单词们

<span style="color: #ff0000;">Lesson 4.1: CURSOR LOCATION AND FILE STATUS</span>

教你用CTRL-G来显示当前文件的位置文件状态，用G来移动到文件中的一行

直接按G，跳到文件末尾，按gg跳到文件开头

先输入一个数字，在按G，着跳到数字所对应的行

<span style="color: #ff0000;">Lesson 4.2: THE SEARCH COMMAND</span>

教你用/来进行正向搜索，直接用/后面跟关键词进行搜索，按n搜索下一个，按N反方向搜索。用?来进行逆向搜索

CTRL-O可以返回你之前的位置，CTRL-I则是前进

<span style="color: #ff0000;">Lesson 4.3: MATCHING PARENTHESES SEARCH</span>

教你用%来查找配对的括号

<span style="color: #ff0000;">Lesson 4.4: THE SUBSTITUTE COMMAND</span>

教你用:s/old/new/g来将old替换为new

比如:s/thee/the将该行第一个thee替换为the

后面加个g表示全局的，:s/thee/the/g，替换该行所有的thee

:#,#s/old/new/g可以替换指定的两行之前的所有

:%s/old/new/g替换整个文件

:%s/old/new/gc加上c，则每次都询问是否替换

<span style="color: #ff0000;">Lesson 5.1: HOW TO EXECUTE AN EXTERNAL COMMAND</span>

教你用:!来执行外部命令，比如:!ls，:!dir

<span style="color: #ff0000;">Lesson 5.2: MORE ON WRITING FILES</span>

教你用:w FILENAME来另存为

教你用v来选中文本，并结合w来将选中文本写到另一个文件

同时，对于选中的文本还可以进行其他操作，如d

<span style="color: #ff0000;">Lesson 5.4: RETRIEVING AND MERGING FILES</span>

教你用:r FILENAME来读取并合并指定文件到光标位置

甚至还可以把外部命令的输出插入文件，比如:r !ls

<span style="color: #ff0000;">Lesson 6.1: THE OPEN COMMAND</span>

教你用o命令在光标之后打开一个新的行，O在光标之前

<span style="color: #ff0000;">Lesson 6.2: THE APPEND COMMAND</span>

教你用a来在光标之后进行插入，并结合e一起使用

<span style="color: #ff0000;">Lesson 6.3: ANOTHER WAY TO REPLACE</span>

教你用R来替换多个字符

<span style="color: #ff0000;">Lesson 6.4: COPY AND PASTE TEXT</span>

教你用v来选中，y来复制，p来粘贴

y可以当做operator，比如用yw来复制一个单词

<span style="color: #ff0000;">Lesson 6.5: SET OPTION</span>

教你用:set ic来忽略大小写(Ignore case)

还有:set hls来高亮匹配(highlight)

还有:set is来增量匹配

如果要取消设置的选项，在前面加no，如:set noic

<span style="color: #ff0000;">Lesson 7.1: GETTING HELP</span>

教你用:help来获取帮助

用CTRL-W CTRL-W来在不同窗口间切换

还可以指定的主题，如:help c_CTRL-D，:help insert-index，:help w，:help user-manual等

<span style="color: #3366ff;">帮助文档的格式有必要好好研究下</span>

<span style="color: #ff0000;">Lesson 7.2: CREATE A STARTUP SCRIPT</span>

教你创建<span style="color: #3366ff;">vimrc</span>文件，来进行设置

<span style="color: #ff0000;">Lesson 7.3: COMPLETION</span>

教你用CTRL-D来显示所有命令，TAB键来自动补全一个命令，用样也适用于文件名的补全