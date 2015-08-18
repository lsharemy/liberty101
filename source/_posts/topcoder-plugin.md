title: 'Topcoder插件安装 | Topcoder plug-ins - KawigiEdit & FileEdit + CodeProcessor + TZTester'
tags:
  - Topcoder
id: 420
categories:
  - wordpress
  - index.php
  - csit
date: 2011-11-07 15:09:41
---

![](http://i.minus.com/ibb0FjVOQ5TCwx.jpg "topcoder1")

重装了系统，又要装插件，趁这个机会就总结下吧。 官方的文档在这里：[http://apps.topcoder.com/wiki/display/tc/How+to+install+The+Arena+plug-ins](http://apps.topcoder.com/wiki/display/tc/How+to+install+The+Arena+plug-ins)

<!--more-->

用的比较多就应该是<span style="color: #f18200;">KawigiEdit和FileEdit + CodeProcessor + TZTester</span>组合吧 首先是[KawigiEdit](http://community.topcoder.com/contest/classes/KawigiEdit/KawigiEdit.html)，相当于一个增强的编辑器，其中也包括了代码生成等功能，安装过程和功能特性插件页面都有，就不累赘了，下面是安装好之后的效果（还有即时记分功能噢！）：

![](http://i.minus.com/iZU6J9MbSxY5B.jpg "topcoder2")

有一点需要<span style="color: #ff0000;">注意</span>，就是要用这个插件的测试功能，必须要有<span style="color: #f18200;">g++</span>，在<span style="color: #f18200;">windows</span>下的话就要安装一个<span style="color: #f18200;">mingw</span>了，我装的是<span style="color: #f18200;">CodeBlocks</span>，里面带有<span style="color: #f18200;">mingw</span>，把<span style="color: #f18200;">bin</span>目录加一下环境变量就ok了。 然后就是<span style="color: #f18200;">FileEdit + CodeProcessor + TZTester</span>组合，其中[FileEdit](http://community.topcoder.com/contest/classes/FileEdit/FileEdit.htm)让你可以在一个外部文件中编辑代码，然后在<span style="color: #f18200;">TC</span>里直接提交（是不是很酷，不过提交之前别忘了先保存下外面的文件）。[CodeProcessor](http://community.topcoder.com/contest/classes/CodeProcessor/How%20to%20use%20CodeProcessor%20v2.htm)则是一个代码处理插件，它允许你自己写一个类来对代码进行处理，然后再交给编辑器，等编辑完代码，需要提交的时候，代码也会经过它处理一下，才交给平台。所以她本身不能独立工作，需要一个处理类以及一个编辑器，这个类也有人给我们写了一个，[TZTester](http://community.topcoder.com/contest/classes/TZTester/TZTester.html)，用来生成测试代码，而编辑器则可以用<span style="color: #f18200;">FileEdit</span>、<span style="color: #f18200;">PopsEdit</span> 或标准的编辑器，我选择的FileEdit，因为目前还离不开<span style="color: #f18200;">IDE</span>。 安装过程在上面第一个链接中有，也不累赘啦。 需要注意的几点，<span style="color: #f18200;">FileEdit</span>安装好之后，需要改一下代码输出目录（桌面比较方便），然后还需要改下代码模板。默认的代码模板为：
<pre>$BEGINCUT$
$PROBLEMDESC$
$ENDCUT$
#line $NEXTLINENUMBER$ "$FILENAME$"
#include &lt;vector&gt;
#include &lt;string&gt;

class $CLASSNAME$ {
	public:
	$RC$ $METHODNAME$($METHODPARMS$) {

	}
};</pre>
我一般改成这样：
[c]
#include &lt;vector&gt;
#include &lt;string&gt;
#include &lt;list&gt;
#include &lt;map&gt;
#include &lt;set&gt;
#include &lt;deque&gt;
#include &lt;stack&gt;
#include &lt;bitset&gt;
#include &lt;algorithm&gt;
#include &lt;functional&gt;
#include &lt;numeric&gt;
#include &lt;utility&gt;
#include &lt;sstream&gt;
#include &lt;iostream&gt;
#include &lt;iomanip&gt;
#include &lt;cstdio&gt;
#include &lt;cmath&gt;
#include &lt;cstdlib&gt;
#include &lt;ctime&gt;
#include &lt;cstring&gt;
#include &lt;climits&gt;
#include &lt;queue&gt;

using namespace std;

class $CLASSNAME$
{
public:
    $RC$ $METHODNAME$($METHODPARMS$)
    {
        $RC$ res = 0;
        return res;
    }
    $TESTCODE$
};
// BEGIN CUT HERE
int main()
{
    $CLASSNAME$ ___test;
    ___test.run_test(-1);
}
// END CUT HERE
[/c]