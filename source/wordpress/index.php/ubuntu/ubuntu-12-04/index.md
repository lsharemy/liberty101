title: Ubuntu 12.04
id: 1684
date: 2012-07-15 01:15:54
---

浏览器：chrome

最小化最大化关闭放到右上方：sudo apt-get install gconf-editor，gconf-editor，app-&gt;smetacity-&gt;general，将button_layout的值由“**close,maximize,minimize:**”改成“**:minimize,maximize,close**”

中文输入法：ibus

右键终端：sudo apt-get install nautilus-open-terminal

虚拟机：virtualbox

UltraSurf：sudo add-apt-repository ppa:ubuntu-wine/ppa、sudo apt-get install wine1.4、winetricks->mfc42、^_^