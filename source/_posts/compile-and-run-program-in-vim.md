title: 在VIM中编译运行程序
tags:
  - vim
id: 1613
categories:
  - wordpress
  - index.php
  - csit
date: 2012-07-12 16:12:20
---

参考[http://www.vimer.cn/2009/10/11.html](http://www.vimer.cn/2009/10/11.html)这个，自己写了个在vim里编译并运行程序的，暂时跑着还不错。直接F5就可以编译运行啦。<!--more-->

[bash]
&quot; compile source file
map &lt;F5&gt; :call Compile()&lt;CR&gt;
function Compile()
&quot;  %表示当前文件名，%:p表示包含文件名的全部路径，%:p:h表示文件所在路径(head of the full path)
if expand(&quot;%:p:h&quot;)!=getcwd()
echohl WarningMsg | echo &quot;Fail to make! This file is not in the current dir! | echohl None
return
endif
let sourcefileename=expand(&quot;%:t&quot;)
if (sourcefileename==&quot;&quot; || (&amp;filetype!=&quot;cpp&quot; &amp;&amp; &amp;filetype!=&quot;c&quot;))
echohl WarningMsg | echo &quot;Fail to make! Please select the right file!&quot; | echohl None
return
endif
let deletedspacefilename=substitute(sourcefileename,' ','','g')
if strlen(deletedspacefilename)!=strlen(sourcefileename)
echohl WarningMsg | echo &quot;Fail to make! Please delete the spaces in the filename!&quot; | echohl None
return
endif
if &amp;filetype==&quot;c&quot;
set makeprg=gcc\ -o\ %&lt;\ %
endif
    elseif &amp;filetype==&quot;cpp&quot;
        set makeprg=g++\ -o\ %&lt;\ %
    endif
    let outfilename=substitute(sourcefileename,'\(\.[^.]*\)' ,'','g')
    let toexename=outfilename
    if filereadable(outfilename)
        let outdeletedsuccess=delete(&quot;./&quot;.outfilename)
        if(outdeletedsuccess!=0)
            set makeprg=make
            echohl WarningMsg | echo &quot;Fail to make! I cannot delete the &quot;.outfilename | echohl None
            return
        endif
    endif
    execute &quot;silent make&quot;
    set makeprg=make
    execute &quot;normal :&quot;
    if filereadable(outfilename)
        execute &quot;!./&quot;.toexename
    endif
    execute &quot;copen&quot;
endfunction
[/bash]