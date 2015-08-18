title: Spinner - SimpleCursorAdapter
tags:
  - Android
id: 1511
categories:
  - wordpress
  - index.php
  - csit
date: 2012-06-18 15:42:50
---

想将数据库中的数据源绑定到Spinner中，需要用到SimpleCursorAdapter。

官方教程：[http://developer.android.com/guide/topics/ui/binding.html](http://developer.android.com/guide/topics/ui/binding.html)

不过遇到了如下错误：

<span style="color: #ff0000;">06-18 07:31:55.445: E/AndroidRuntime(4211): Uncaught handler: thread main exiting due to uncaught exception</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): java.lang.RuntimeException: Unable to start activity ComponentInfo{lmyyyx.ydnt/lmyyyx.ydnt.StudyHome}: java.lang.IllegalArgumentException: column '_id' does not exist<!--more--></span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2496)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2512)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.ActivityThread.access$2200(ActivityThread.java:119)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1863)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.os.Handler.dispatchMessage(Handler.java:99)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.os.Looper.loop(Looper.java:123)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.ActivityThread.main(ActivityThread.java:4363)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at java.lang.reflect.Method.invokeNative(Native Method)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at java.lang.reflect.Method.invoke(Method.java:521)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:860)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:618)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at dalvik.system.NativeStart.main(Native Method)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): Caused by: java.lang.IllegalArgumentException: column '_id' does not exist</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.database.AbstractCursor.getColumnIndexOrThrow(AbstractCursor.java:314)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.widget.CursorAdapter.init(CursorAdapter.java:111)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.widget.CursorAdapter.&lt;init&gt;(CursorAdapter.java:90)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.widget.ResourceCursorAdapter.&lt;init&gt;(ResourceCursorAdapter.java:47)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.widget.SimpleCursorAdapter.&lt;init&gt;(SimpleCursorAdapter.java:88)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at lmyyyx.ydnt.StudyHome.onCreate(StudyHome.java:35)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1047)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2459)</span>
<span style="color: #ff0000;"> 06-18 07:31:55.455: E/AndroidRuntime(4211): ... 11 more</span>

最后发现，数据源必须要有一个_id字段，所以最好在数据库中将主键命名为_id。