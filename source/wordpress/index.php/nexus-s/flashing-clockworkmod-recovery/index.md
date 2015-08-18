title: 刷入ClockworkMod Recovery
id: 1091
date: 2012-01-20 15:50:46
---

所需工具：fastboot.exe、recovery.img

解锁完bootloader后，在fastboot模式，在PC上执行以下命令：

<span style="color: #008000;">fastboot flash recovery recovery.img</span>

输出：

sending 'recovery' (4036 KB)... OKAY
writing 'recovery'... OKAY

Recovery就刷好了

---------------------------------------

这个时候不要重启手机，直接进入Recovery（重启后，好像又会恢复成原来的recovery）

会看到ClockworkMod Recovery v3.1.0.1启动起来了

通过它，就可以进行刷系统、root等等过程了。