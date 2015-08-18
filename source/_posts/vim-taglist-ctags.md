title: Vim + taglist + ctags
tags:
  - vim
id: 1610
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 15:00:20
---

记得上次配置过的，但是好像没记录，刚好上次弄python又把系统弄坏，重装了一次，所以之前配置好的都没了

所以再配置一遍，做个记录

taglist是vim的一个插件，[http://www.vim.org/scripts/script.php?script_id=273](http://www.vim.org/scripts/script.php?script_id=273)

ctags则是一个tag文件，包含了很多语言的支持，[http://ctags.sourceforge.net/index.html](http://ctags.sourceforge.net/index.html)

暂时把~/.vimrc设置如下：
<pre>" taglist
nnoremap &lt;silent&gt; &lt;F12&gt; :TlistToggle&lt;CR&gt;
let Tlist_Use_Right_Window=1</pre>