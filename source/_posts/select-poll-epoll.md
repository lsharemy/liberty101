title: select/poll/epoll
tags:
  - Linux
id: 1947
categories:
  - wordpress
  - index.php
  - csit
date: 2012-10-18 13:32:21
---

好吧，面试老问这个，总结下

select函数原型：

[c]
int select(int nfds, fd_set *readfds, fd_set *writefds,
                  fd_set *exceptfds, struct timeval *timeout);
[/c]

fd_set结构：

[c]
#undef __FD_SETSIZE
#define __FD_SETSIZE	1024

typedef struct {
	unsigned long fds_bits[__FD_SETSIZE / (8 * sizeof(long))];
} __kernel_fd_set;
[/c]

原来就是8个无符号整型的数组，当做位向量来用。

然后相关的FD_CLR、FD_SET、FD_ISSET和FD_ZERO等函数就是相应的位操作了。<!--more-->

select中的第一个参数nfds要比你想检查的最大的描述符的值大，因为它会检查0到nfds-1之间的描述符，看其是否可用。

函数返回时，会返回可用描述符的个数，参数中的三个描述符集合向量也会被修改，所以循环调用select的时候，记得重新设置集合向量

由于__FD_SETSIZE被设置为1024，所以select也就最多能检查1024个描述符

poll函数原型：

[c]
int poll(struct pollfd *fds, nfds_t nfds, int timeout);
[/c]

采用了数组的形式，所以对描述符的个数没有了1024的限制，不过效率还是很慢，因为它还是会遍历所有关心的描述符，看其是否可用（不过比select好点）

结构体：

[c]
struct pollfd {
    int fd;
    short events;
    short revents;
};
[/c]

events表示对该描述符关心的事件，函数返回的时候，可用的描述符的revents会被设置。

epoll相关函数：

[c]
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
int epoll_wait(int epfd, struct epoll_event *events,
                      int maxevents, int timeout);
[/c]

结构体：

[c]
typedef union epoll_data {
    void    *ptr;
    int      fd;
    uint32_t u32;
    uint64_t u64;
} epoll_data_t;

struct epoll_event {
    uint32_t     events;    /* Epoll events */
    epoll_data_t data;      /* User data variable */
};
[/c]

epoll_create用来创建一个epoll实例

epoll_ctl则用来往增加、删除、修改集合。

epoll_wait等待epoll实例epfd上的事件，函数返回时，返回可用描述符的个数，并将它们放在events指向的地方

epoll与select/poll不一样，不是采用的轮询的方式，而是采用回调函数的方式。因此效率高很多。

参考：[http://blog.dccmx.com/2011/04/select-poll-epoll-in-kernel/](http://blog.dccmx.com/2011/04/select-poll-epoll-in-kernel/)，

[http://www.kegel.com/c10k.html](http://www.kegel.com/c10k.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man2/select_tut.2.html](http://www.kernel.org/doc/man-pages/online/pages/man2/select_tut.2.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man2/select.2.html](http://www.kernel.org/doc/man-pages/online/pages/man2/select.2.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man2/poll.2.html](http://www.kernel.org/doc/man-pages/online/pages/man2/poll.2.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man7/epoll.7.html](http://www.kernel.org/doc/man-pages/online/pages/man7/epoll.7.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man2/epoll_create.2.html](http://www.kernel.org/doc/man-pages/online/pages/man2/epoll_create.2.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man2/epoll_ctl.2.html](http://www.kernel.org/doc/man-pages/online/pages/man2/epoll_ctl.2.html)，

[http://www.kernel.org/doc/man-pages/online/pages/man2/epoll_wait.2.html](http://www.kernel.org/doc/man-pages/online/pages/man2/epoll_wait.2.html)