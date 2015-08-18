title: 进入fastboot模式
id: 1072
date: 2012-01-20 12:30:14
---

官方有教程：

[http://source.android.com/source/building-devices.html](http://source.android.com/source/building-devices.html)

上面这个页面的Booting into fastboot mode小节。

有两种方式，一种是通过机器启动的时候按特定的按键组合（Neuxs S是音量向上键+电源键）。

另一种就是用adb reboot bootloader命令（需先开启USB调试），adb是官方提供的调试工具，包含在SDK中，可以从源代码中自己编译。

如果没有出错，这时手机已经进入了fastboot模式。