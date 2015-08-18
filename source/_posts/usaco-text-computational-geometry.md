title: USACO - TEXT Computational Geometry
tags:
  - USACO
id: 1900
categories:
  - wordpress
  - index.php
  - csit
date: 2012-09-10 12:04:10
---

终于知道计算几何是什么了

首先是计算机几何的三个“工具”：叉积、点积、反正切

1.叉积_u x v_

_u x v的结果等于以下行列式的结果：_

|  i    j    k  |
| ux uy uz |
| vx vy vz |

也就是等于(_u_<sub>y</sub>_v_<sub>z</sub>-_v_<sub>y</sub>_u_<sub> z</sub>)**i** + (_u_<sub>z</sub>_v_<sub>x</sub>-_u_<sub> x</sub>_v_<sub>z</sub>)**j** + (_u_<sub>x</sub>_v_<sub> y</sub>-_u_<sub>y</sub>_v_<sub>x</sub>)**k**

![](http://ace.delos.com/usaco/TEXT/geom6.gif)<!--more-->

叉积的三个性质：

（1）两个向量的叉积与这两个向量都垂直

（2）两个向量的叉积的长度=|u|*|v|*sin（夹角）

（3）两个向量的叉积的方向符合右手螺旋法则（手指从u到v，大姆子指的方向）

![](http://ace.delos.com/usaco/TEXT/geom7.gif)

2.点积u · v

_u和v的点积等于u_<sub>_x_</sub>_v_<sub>_x_</sub> + _u_ <sub>y</sub>_v_<sub>y</sub> + _u_<sub>z</sub>_v_ <sub>z</sub>

也等于|u|*|v|*cos（夹角）

如果点积为正，夹角是锐角；点积为负，夹角是钝角；点积为0，夹角为90度。

3.反正切Arctangent

C中的两个函数，一个是_atan，它返回-pi/2到pi/2之间的一个数_

还有一个是_atan2，返回-pi到pi之间的一个数，可以确定象限，所以这个函数一般用的比较多_

-----------------------------------------------------------------------

注意：

1.几何问题往往有很多特例！要注意

2.浮点数的判等！要注意

-----------------------------------------------------------

几何算法：

1.计算三角形面积

知道三个点的坐标：算出两个向量，三角行的面积等于向量叉积长度的一半

直到三条边的长度：令_s = (a+b+c)/2，则面积等于<em>sqrt(s* (s-a)*(s-b)*(s-c))_</em>

![](http://ace.delos.com/usaco/TEXT/geom1.gif)

2.判断两条线段是否平行

在两条线上分别找出一个向量，看这两个向量是否平行，只要看它们的叉积是不是（几乎）等于0

3.多边形的面积

多边形的顶点为别为：(_x_ <sub>1</sub>, _y_ <sub>1</sub>), ..., (_x_ <sub>n</sub>, _y_ <sub>n</sub>)，那么它的面积为：

1   | x1 x2 ... xn |
---  |                    |
2   | y1 y2 ... yn |

行列式的值等于_x_<sub>1</sub> _y_<sub>2 </sub>+ _x_<sub>2</sub>_y_<sub>3</sub>+ ... + _x_<sub>_n_</sub> _y_<sub>1</sub> - _y_<sub>1</sub> _x_<sub>2</sub> - _y_<sub>2</sub>_x_<sub>3</sub> - ... - _y_<sub>n</sub> _x_<sub>1</sub>

4.点到直线的距离

_d_(P,AB) = |(P - A) x (B - A)| / | B - A|，可由上面求三角形的面积转换而来

点P到由A、B、C构成的平面的距离：令_n_ = (B - A) x (C - A)，则_d_(P,ABC) = (P-A) · n / |n|，n是垂直平面的一个向量，所以求出P到n的投影长度即可。

5.点是否在直线上

看该点到该直线的距离是否是0即可，见4

6.点是否在一条直线的同一边（二维）

查看C和D是否在AB直线的同一边，分别计算_(B - A) x (C - A)_ and _(B - A) x (D - A)_的z分量的符号，如果两者的符号相同，则C和D在AB的同一边

7.点是否在一条线段上

要看C是否在线段AB上，只要看AB的长度是不是AC和CB的长度之和

8.点是否在三角形内

先找到三角形内的一个点B（比如三角形3个点取平均值），然后查看A和B是否都在三角行三条边的同一边。

![](http://ace.delos.com/usaco/TEXT/geom3.gif)

9.点是否在凸多边形内

类似于8

![](http://ace.delos.com/usaco/TEXT/geom4.gif)

10.4个点是否共面

选择其中3个点A、B、C，对于剩下的点D，如果满足(B - A) x (C - A)) · (D - A) = 0，则4点共面

11.2条直线相交

2维，不平行，则相交

3维，不平行且共面，则相交

12.2条线段相交

2维，当A、B分别在C、D的两边，同时C、D分别在AB的两边，则两直线相交

![](http://ace.delos.com/usaco/TEXT/geom5.gif)

3维，如果以下方程组

A<sub>_x_</sub> + (B<sub>_x_</sub> - A<sub>_x_</sub>) i = C<sub>_x_</sub> + (D<sub>_x_</sub> - C<sub>_x_</sub>) j
A<sub>_y_</sub> + (B<sub>_y_</sub> - A<sub>_y_</sub>) i = C<sub>_y_</sub> + (D<sub>_y_</sub> - C<sub>_y_</sub>) j
A<sub>_z_</sub> + (B<sub>_z_</sub> - A<sub>_z_</sub>) i = C<sub>_z_</sub> + (D<sub>_z_</sub> - C<sub>_z_</sub>) j

存在一组解(_i_, _j_)，其中0 &lt;= _i_ &lt;= 1且0 &lt;= _j_ &lt;= 1。则AB和CD相交于 (A<sub>_x_</sub> + (B<sub>_x_</sub> - A<sub>_x_</sub>)i, A<sub>_y_</sub> + (B<sub>_y_</sub> - A<sub>_y_</sub>)i, A<sub>_z_</sub> + (B<sub>_z_</sub> - A<sub>_z_</sub>) i。

13.两条线段的交点

2维，解以下方程组

A<sub>_x_</sub> + (B<sub>_x_</sub> - A<sub>_x_</sub>)i = C<sub>_x_</sub> + (D<sub>_x_</sub> - C<sub>_x_</sub>) j
A<sub>_y_</sub> + (B<sub>_y_</sub> - A<sub>_y_</sub>)i = C<sub>_y_</sub> + (D<sub>_y_</sub> - C<sub>_y_</sub>) j

则交点为：(A<sub>_x_</sub> + (B<sub>_x_</sub> - A<sub>_x_</sub>) i, A<sub>_y_</sub> + (B<sub>_y_</sub> - A<sub>_y_</sub>) i)

3维，见12

14.多边形的凸性

按顺时针的方向沿着多边形走，对任意连续的三个点(A, B, C)，计算(B - A) x (C - A)的z分量，如果符号为都为正，这是凸多边形

15.点是否在一个非凸多边形中

从该点出发，向任意方向做一条射线，如果该射线经过了多变行的顶点或与多边形的某边重合，则重新作一条射线。如果该射线与多边形相交的边数为奇数，则在多边形内。

![](http://ace.delos.com/usaco/TEXT/geom8.gif)

------------------------------------------------------

几何方法：（没太理解透）

1.蒙特卡罗方法([http://zh.wikipedia.org/zh/%E8%92%99%E5%9C%B0%E5%8D%A1%E7%BE%85%E6%96%B9%E6%B3%95](http://zh.wikipedia.org/zh/%E8%92%99%E5%9C%B0%E5%8D%A1%E7%BE%85%E6%96%B9%E6%B3%95))

2.分区法

![](http://ace.delos.com/usaco/TEXT/geom9.gif)

------------------------------------------------------