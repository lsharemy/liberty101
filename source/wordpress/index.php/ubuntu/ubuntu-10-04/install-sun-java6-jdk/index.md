title: 安装sun-java6-jdk
id: 1032
date: 2012-01-11 10:36:43
---

<del>开始Android</del>

<del><span style="text-align: left;">根据官方教程：</span>[http://developer.android.com/sdk/installing.html#troubleshooting](http://developer.android.com/sdk/installing.html#troubleshooting)</del>

<del><span style="text-align: left;">需要先装个sun-java6-jdk</span></del>

<del>不过sudo apt-get install sun-java6-jdk后得到以下错误：</del>

<span style="color: #ff0000;">Package sun-java6-jdk is not available, but is referred to by another package.</span>
<span style="color: #ff0000;"> This may mean that the package is missing, has been obsoleted, or</span>
<span style="color: #ff0000;"> is only available from another source</span>
<span style="color: #ff0000;"> E: Package sun-java6-jdk has no installation candidate</span>

<del>解决方法：在软件源中增加partner软件源</del>

<del>命令行：sudo add-apt-repository "deb http://archive.canonical.com/ lucid partner"</del>

<del>当然也可以在图形界面完成，打个勾就可以了</del>

----------------------------------------------------------------------------------------

2012.02.16，java又被从partner库中移除了，所以以上办法失效

[https://lists.ubuntu.com/archives/ubuntu-security-announce/2012-January/001554.html](https://lists.ubuntu.com/archives/ubuntu-security-announce/2012-January/001554.html)

-------------------------------------------------------------------------------------

这次是为了在Ubuntu下搭Topcoder的环境，所以要用java

用OpenJDK成功代替

sudo apt-get install openjdk-6-jdk

------------------------------------------------------------------------