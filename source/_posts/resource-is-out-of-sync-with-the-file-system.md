title: "Could not open the editor: Resource is out of sync with the file system: '***/***.properties'"
tags:
  - Android
id: 1490
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-06 09:40:54
---

导入Android工程的时候会遇到project.properties或default.properties打不开，提示以下错误：

<span style="color: #ff0000;">Could not open the editor: Resource is out of sync with the file system: '***/***.properties'</span>

这个问题通过工程右键Refresh一下就好了，直接F5也一样。

而project.properties和default.properties有什么区别的问题，好像是ADT版本不一样，命名不一样。

找到了，ADT 14以后都命名为project.properties，见[http://tools.android.com/recent/buildchangesinrevision14](http://tools.android.com/recent/buildchangesinrevision14)里面的最后一部分（Project setup）：

<span style="color: #ff6600;">_default.properties_ which is the main project’s properties file containing information such as the build platform target and the library dependencies has been renamed _project.properties_.</span>