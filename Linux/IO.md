# IO

## I/O多路复用(I/O多路转接)

I/O多路复用使得程序能同时监听多个文件描述符，能够提高程序的性能，Linux下实现I/O多路复用的系统调用主要有select、poll和epoll

## select

主旨思想
> 1.首先要构造一个关于文件描述符的列表，将要监听的文件描述符添加到该列表中
> 2.抵用一个系统函数，监听该列表中的文件描述符，直到这些描述符中的一个或者多个进行I/O操作时，该函数才返回
> &emsp;&emsp;a.这个函数是阻塞的
> &emsp;&emsp;b.函数对文件描述符的检测的操作是由内核完成的
> 3.在返回时，它会告诉进程有多少（哪些）描述符要进行I/O操作

```cpp
#include<sys/time.h>
#incldue<sys/types.h>
#include<unistd.h>
int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
    - 参数：
        - nfds：委托内核检测的最大文件描述符的值 + 1
        - readfds：要检测的文件描述符的读的集合，委托内核检测哪些文件描述符的读的属性
                    - 一般检测读操作
                    - 对应的是对方发送过来的数据，因为读时被动的接收数据，检测的是读缓冲区
                    - 是一个传入传出参数
        - writefds: 要检测的文件描述符的写的集合，委托内核检测哪些文件描述符的写的属性
                    - 委托内核检测写缓冲区是不是还可以写数据（不满的就可以写）
        - exceptfds：检测异常的文件描述符的集合
        - timeout: 设置的超时时间
                - NULL：永久阻塞，直到检测到了文件描述符有变化
    - 返回值：
        - -1：失败
        - >0(n): 检测的集合中有n个文件描述符发生了变化

// 将参数文件描述符fd对应的标志位设置为0
void FD_CLR(int fd, fd_set *set);

// 判断fd对应的标志位是0还是1，返回值：fd对应的标志位的值
int FD_ISSET(int fd, fd_set *set);

// 将参数文件描述符fd对应的标志位，设置为1
void FD_SET(int fd, fd_set *set);

// fdset一共有1024bit，全部初始化为0
void FD_ZERO(fd_set *set);
```
