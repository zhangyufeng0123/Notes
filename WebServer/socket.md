# socket
## socket介绍
> 所谓socket(套接字)，就是对网络中不同主机上的应用进程之间进行双向通信的端点的抽象。一个套接字就是网络上进程通信的一端，提供了应用层进程利用网络协议交换数据的机制。从所处的地位来将，套接字上联应用进程，下联网络协议栈，是应用程序通过网络协议进行通信的接口，是应用程序与网络协议栈进行交互的接口。  
> 
> socket 可以看成是两个网络应用程序进行通信时，各自通信连接中的端点，这是一个逻辑上的概念。它是网络环境中进程间通信的 API，也是可以被命名和寻址的通信端点，使用中的每一个套接字都有其类型和一个与之相连进程。通信时其中一个网络应用程序将要传输的一段信息写入它所在主机的 socket 中，该 socket 通过与网络接口卡（NIC）相连的传输介质将这段信息送到另外一台主机的 socket 中，使对方能够接收到这段信息。socket 是由 IP 地址和端口结合的，提供向应用层进程传送数据包的机制。
> 
> socket 本身有“插座”的意思，在 Linux 环境下，用于表示进程间网络通信的特殊文件类型。本质为内核借助缓冲区形成的伪文件。既然是文件，那么理所当然的，我们可以使用文件描述符引用套接字。与管道类似的，Linux 系统将其封装成文件的目的是为了统一接口，使得读写套接字和读写文件的操作一致。区别是管道主要应用于本地进程间通信，而套接字多应用于网络进程间数据的传递。


**套接字通信分两部分**
- 服务器端：被动接受连接，一般不会主动发起连接
- 客户端：主动向服务器发起连接

socket是一套通信的接口，**Linux**和**Windows**都有，但是有一些细微的差别

## 字节序

### 简介

> 现代 CPU 的累加器一次都能装载（至少）4 字节（这里考虑 32 位机），即一个整数。那么这 4字节在内存中排列的顺序将影响它被累加器装载成的整数的值，这就是字节序问题。在各种计算机体系结构中，对于字节、字等的存储机制有所不同，因而引发了计算机通信领域中一个很重要的问题，即通信双方交流的信息单元（比特、字节、字、双字等等）应该以什么样的顺序进行传送。如果不达成一致的规则，通信双方将无法进行正确的编码/译码从而导致通信失败。
>
> <font color = red>字节序，顾名思义字节的顺序，就是大于一个字节类型的数据在内存中的存放顺序(一个字节的数据当然就无需谈顺序的问题了)。</font>
>
> 字节序分为大端字节序（Big-Endian） 和小端字节序（Little-Endian）。大端字节序是指一个整数的最高位字节（23 ~ 31 bit）存储在内存的低地址处，低位字节（0 ~ 7 bit）存储在内存的高地址处；小端字节序则是指整数的高位字节存储在内存的高地址处，而低位字节则存储在内存的低地址处。

**判断本机字节序的代码**
```c
#include <stdio.h>

int main(){

    union {
        short value;    // 2字节
        char bytes[sizeof(short)];  // char[2]
    }test;

    test.value = 0x0102;
    if((test.bytes[0] == 1) && (test.bytes[1] == 2)){
        printf("大端字节序\n");
    }else if((test.bytes[0] == 2) && (test.bytes[1] == 1)){
        printf("小端字节序\n");
    }else{
        printf("未知\n");
    }
    return 0;
}
```


### 字节序转换函数

当格式化的数据在两台使用不同字节序的主机之间直接传递时，接收端必然错误的解释之。解决问题的方法是：发送端总是把要发送的数据转换成大端字节序数据后再发送，而接收端知道对方传送过来的数据总是采用大端字节序，所以接收端可以根据自身采用的字节序决定是否对接收到的数据进行转换（小端机转换，大端机不转换）。

**网络字节顺序**是 TCP/IP 中规定好的一种数据表示格式，它与具体的 CPU 类型、操作系统等无关，从而可以保证数据在不同主机之间传输时能够被正确解释，网络字节顺序采用大端排序方式。

BSD Socket提供了封装好的转换接口，方便程序员使用。包括从主机字节序到网络字节序的转换函数：htons、htonl；从网络字节序到主机字节序的转换函数：ntohs、ntohl。

```
h   - host 主机，主机字节序
to  - 转换成什么
n   - network 网络字节序
s   - short unsigned short
l   - long unsigned int
```

```cpp
#include <arpa/inet.h>
// 转换端口
uint16_t htons(uint16_t hostshort); // 主机字节序 - 网络字节序
uint16_t ntohs(uint16_t netshort); // 主机字节序 - 网络字节序

// 转IP
uint32_t htonl(uint32_t hostlong); // 主机字节序 - 网络字节序
uint32_t ntohl(uint32_t netlong); // 主机字节序 - 网络字节序
```

网络通信时，需要将主机字节序转换成网络字节序（大端）
另外一段获取到数据以后根据情况将网络字节序转换成主机字节序

**转换示例**
```cpp

#include<stdio.h>
#include<arpa/inet.h>

int main(){
    // htons
    unsigned short a = 0x0102;
    printf("a : %x\n", a);

    unsigned short b = htons(a);
    printf("b : %x\n", b);

    // htonl 转换IP
    char buf[4] = {192, 168, 1, 100};
    int num = *(int *)buf;
    unsigned int sum = htonl(num);
    unsigned char *p = (char *)&sum;
    printf("%d %d %d %d\n", *p, *(p + 1), *(p + 2), *(p + 3));


    // ntohs
    unsigned char buf1[4] = {1, 1, 168, 192};
    int num1 = *(int *)buf1;
    int sum1 = ntohl(num1);
    unsigned char *p1 = (unsigned char *)&sum1;
    printf("%d %d  %d %d\n", *p1, *(p1 + 1), *(p1 + 2), *(p1 + 3));

    // ntohs
    return 0;
}

```

## socket地址

socket地址其实是一个结构体，封装端口号和IP等信息。后面的socket相关的api中需要使用到这个socket地址。

### 通用socket地址

socket 网络编程接口中表示 socket 地址的是结构体 sockaddr，其定义如下：

```cpp
#include <bits/socket.h>
struct sockaddr {
    sa_family_t sa_family;
    char sa_data[14];
};

typedef unsigned short int sa_family_t;
```

sa_family 成员是地址族类型（sa_family_t）的变量。地址族类型通常与协议族类型对应。常见的协议族（protocol family，也称 domain）和对应的地址族入下所示：

| 协议族 | 地址族 | 描述 |
| -- | -- | -- |
| PF_UNIX | AF_UNIX | UNIX本地域协议族 |
| PF_INET | AF_INET | TCP/IPv4协议族 |
| PF_INET6 | AF_INET6 | TCP/IPv6协议族 |

宏 PF_* 和 AF_* 都定义在 bits/socket.h 头文件中，且后者与前者有完全相同的值，所以二者通常混用。

sa_data 成员用于存放 socket 地址值。但是，不同的协议族的地址值具有不同的含义和长度，如下所示：

| 协议族 | 地址值含义和长度 |
| -- | -- | 
| PF_UNIX | 文件的路径名，长度可达到108字节 |
| PF_INET | 16bit端口号和32bit IPv4地址，共6字节 |
| PF_INET6 | 16bit端口号和32bit流标识，128bit IPv6 地址，32bit范围ID，共26字节 |

由上表可知，14 字节的 sa_data 根本无法容纳多数协议族的地址值。因此，Linux 定义了下面这个新的通用的 socket 地址结构体，这个结构体不仅提供了足够大的空间用于存放地址值，而且是内存对齐的。

```cpp
#include <bits/socket.h>
struct sockaddr_storage
{
    sa_family_t sa_family;
    unsigned long int __ss_align;
    char __ss_padding[ 128 - sizeof(__ss_align) ];
};

typedef unsigned short int sa_family_t
```

## IP地址转换（字符串ip-整数，主机、网络字节序的转换）

通常，人们习惯用可读性好的字符串来表示 IP 地址，比如用点分十进制字符串表示 IPv4 地址，以及用十六进制字符串表示 IPv6 地址。但编程中我们需要先把它们转化为整数（二进制数）方能使用。而记录日志时则相反，我们要把整数表示的 IP 地址转化为可读的字符串。下面 3 个函数可用于用点分十进制字符串表示的 IPv4 地址和用网络字节序整数表示的 IPv4 地址之间的转换：

```c
#include <arpa/inet.h>
in_addr_t inet_addr(const char *cp);
int inet_aton(const char *cp, struct in_addr *inp);
char *inet_ntoa(struct in_addr in);
```

下面这对更新的函数也能完成前面 3 个函数同样的功能，并且它们同时适用 IPv4 地址和 IPv6 地址：

```c
#include <arpa/inet.h>
// p:点分十进制的IP字符串，n:表示network，网络字节序的整数
int inet_pton(int af, const char *src, void *dst);
    af:地址族： AF_INET AF_INET6
    src:需要转换的点分十进制的IP字符串
    dst:转换后的结果保存在这个里面
// 将网络字节序的整数，转换成点分十进制的IP地址字符串
const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);
    af:地址族： AF_INET AF_INET6
    src: 要转换的ip的整数的地址
    dst: 转换成IP地址字符串保存的地方
    size：第三个参数的大小（数组的大小）
    返回值：返回转换后的数据的地址（字符串），和 dst 是一样的
```

**函数示例**
```c
#include<arpa/inet.h>
#include<stdio.h>
#include<string.h>

int main(){
    // 创建一个ip字符串，点分十进制的IP地址字符串
    char buf[] = "192.168.1.4";
    unsigned int num = 0;

    // 将点分十进制的IP字符串转换成网络字节序的整数
    inet_pton(AF_INET, buf, &num);
    unsigned char *p = (unsigned char *)&num;
    printf("%d %d %d %d\n", *p, *(p + 1), *(p + 2), *(p + 3));


    // 将网络字节序的整数转换成点分十进制的IP字符串

    char ip[16] = "";
    const char* str = inet_ntop(AF_INET, &num, ip, 16);
    printf("%s\n", str);

    return 0;
}
```

## TCP通信流程
```
// TCP 和 UDP -> 传输层的协议
UDP:用户数据报协议，面向无连接，可以单播，多播，广播， 面向数据报，不可靠
TCP:传输控制协议，面向连接的，可靠的，基于字节流，仅支持单播传输
                UDP                                     TCP
是否创建连接    无连接                                    面向连接
是否可靠        不可靠                                   可靠的
连接的对象个数  一对一、一对多、多对一、多对多              支持一对一
传输的方式      面向数据报                              面向字节流
首部开销        8个字节                                 最少20个字节
适用场景        实时应用（视频会议，直播）              可靠性高的应用（文件传输)
```
![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514132846.png)

```
// TCP 通信的流程
// 服务器端 （被动接受连接的角色）
1. 创建一个用于监听的套接字
    - 监听：监听有客户端的连接
    - 套接字：这个套接字其实就是一个文件描述符
2. 将这个监听文件描述符和本地的IP和端口绑定（IP和端口就是服务器的地址信息）
    - 客户端连接服务器的时候使用的就是这个IP和端口
3. 设置监听，监听的fd开始工作
4. 阻塞等待，当有客户端发起连接，解除阻塞，接受客户端的连接，会得到一个和客户端通信的套接字（fd）
5. 通信
    - 接收数据
    - 发送数据
6. 通信结束，断开连接 

// 客户端
1. 创建一个用于通信的套接字（fd）
2. 连接服务器，需要指定连接的服务器的 IP 和 端口
3. 连接成功了，客户端可以直接和服务器通信
    - 接收数据
    - 发送数据
4. 通信结束，断开连接
```

**函数示例**
```cpp
// TCP 通信的服务器端

#include<stdio.h>
#include<arpa/inet.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>

int main(){

    // 1.创建套接字，用于监听的套接字

    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if(lfd == -1){
        perror("socket");
        exit(1);
    }

    // 2.绑定端口号+IP地址
    struct sockaddr_in saddr;
    inet_pton(AF_INET, "172.28.117.186", &saddr.sin_addr.s_addr);
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(9999);
    int ret = bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));

    if(ret == -1){
        perror("bind");
        exit(1);
    }

    // 3. 监听
    ret = listen(lfd, 8);
    if(ret == -1){
        perror("listen");
    }

    // 4. 接受客户端的连接
    struct sockaddr_in clientaddr;
    socklen_t len = sizeof(clientaddr);
    int cfd = accept(lfd, (struct sockaddr *)&clientaddr, &len);
    if(cfd == -1){
        perror("accept");
        exit(1);
    }

    // 输出客户端的信息
    char clientIP[16];
    inet_ntop(AF_INET, &clientaddr.sin_addr.s_addr, clientIP, sizeof(clientIP));
    unsigned short clientPort = ntohs(clientaddr.sin_port);
    printf("client ip is %s, port is %d\n", clientIP, clientPort);

    // 5. 通信
    // 获取客户端的数据
    
    char recvBuf[1024] = {0};
    int recvlen = read(cfd, recvBuf, sizeof(recvBuf));
    if(recvlen == -1){
        perror("read");
        exit(1);
    }else if(recvlen > 0){
        printf("recv client data : %s\n", recvBuf);
    }else if(recvlen == 0){
        // 表示客户端断开连接
        printf("client closed ...");
    }

    char * data = "hello, I am server";

    // 给客户端发送数据
    write(cfd, data, strlen(data));

    // 关闭文件描述符
    close(cfd);
    close(lfd);
    return 0;
}
```

```cpp
#include<unistd.h>
#include<stdio.h>
#include<arpa/inet.h>
#include<stdlib.h>
#include<string.h>

int main(){
    // 1. 创建套接字
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    if(fd == -1){
        perror("socket");
        exit(-1);
    }

    // 2. 连接服务器端
    struct sockaddr_in serveraddr;
    inet_pton(AF_INET, "172.28.117.186", &serveraddr.sin_addr.s_addr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_port = htons(9999);
    int ret = connect(fd, (struct sockaddr *)&serveraddr, sizeof(serveraddr));

    if(ret == -1){
        perror("connect");
        exit(-1);
    }

    // 3. 通信
    char *data = "hello, I am client";
    // 给服务端发送数据

    write(fd, data, strlen(data));

    // 从服务端读数据
    char recvBuf[1024] = {0};
    int recvLen = read(fd, recvBuf, sizeof(recvBuf));
    if(recvLen == -1){
        perror("read");
        exit(-1);
    }else if(recvLen == 0){
        printf("server close ...");
    }else if(recvLen > 0){
        printf("recv server data : %s\n", recvBuf);
    }

    // 4. 关闭文件描述符
    close(fd);
    return 0;
}
```


## 套接字函数

```c
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h> // 包含了这个头文件，上面两个就可以省略
int socket(int domain, int type, int protocol);
    - 功能：创建一个套接字
    - 参数：
        - domain: 协议族
            - AF_INET : ipv4
            - AF_INET6 : ipv6
            - AF_UNIX, AF_LOCAL : 本地套接字通信（进程间通信）
        - type: 通信过程中使用的协议类型
            - SOCK_STREAM : 流式协议
            - SOCK_DGRAM : 报式协议
        - protocol : 具体的一个协议。一般写0
            - SOCK_STREAM : 流式协议默认使用 TCP
            - SOCK_DGRAM : 报式协议默认使用 UDP
    - 返回值：
        - 成功：返回文件描述符，操作的就是内核缓冲区。
        - 失败：-1
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen); // socket命名
    - 功能：绑定，将fd 和本地的IP + 端口进行绑定
    - 参数：
        - sockfd : 通过socket函数得到的文件描述符
        - addr : 需要绑定的socket地址，这个地址封装了ip和端口号的信息
        - addrlen : 第二个参数结构体占的内存大小
int listen(int sockfd, int backlog); // /proc/sys/net/core/somaxconn
    - 功能：监听这个socket上的连接
    - 参数：
        - sockfd : 通过socket()函数得到的文件描述符
        - backlog : 未连接的和已经连接的和的最大值， 5
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
    - 功能：接收客户端连接，默认是一个阻塞的函数，阻塞等待客户端连接
    - 参数：
        - sockfd : 用于监听的文件描述符
        - addr : 传出参数，记录了连接成功后客户端的地址信息（ip，port）
        - addrlen : 指定第二个参数的对应的内存大小
    - 返回值：
        - 成功 ：用于通信的文件描述符
        - -1 ： 失败
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
    - 功能： 客户端连接服务器
    - 参数：
        - sockfd : 用于通信的文件描述符
        - addr : 客户端要连接的服务器的地址信息
        - addrlen : 第二个参数的内存大小
    - 返回值：成功 0， 失败 -1
ssize_t write(int fd, const void *buf, size_t count); // 写数据
ssize_t read(int fd, void *buf, size_t count); // 读数据
```

## TCP三次握手

TCP 是一种面向连接的单播协议，在发送数据前，通信双方必须在彼此间建立一条连接。所谓的“连接”，其实是客户端和服务器的内存里保存的一份关于对方的信息，如 IP 地址、端口号等。

TCP 可以看成是一种字节流，它会处理 IP 层或以下的层的丢包、重复以及错误问题。在连接的建立过程中，双方需要交换一些连接的参数。这些参数可以放在 TCP 头部。

TCP 提供了一种可靠、面向连接、字节流、传输层的服务，采用三次握手建立一个连接。采用 四次挥手来关闭一个连接。

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514141413.png)

- 16 位端口号（port number）：告知主机报文段是来自哪里（源端口）以及传给哪个上层协议或应用程序（目的端口）的。进行 TCP 通信时，客户端通常使用系统自动选择的临时端口号。
- 32 位序号（sequence number）：一次 TCP 通信（从 TCP 连接建立到断开）过程中某一个传输方向上的字节流的每个字节的编号。假设主机 A 和主机 B 进行 TCP 通信，A 发送给 B 的第一个TCP 报文段中，序号值被系统初始化为某个随机值 ISN（Initial Sequence Number，初始序号值）。那么在该传输方向上（从 A 到 B），后续的 TCP 报文段中序号值将被系统设置成 ISN 加上该报文段所携带数据的第一个字节在整个字节流中的偏移。例如，某个 TCP 报文段传送的数据是字节流中的第 1025 ~ 2048 字节，那么该报文段的序号值就是 ISN + 1025。另外一个传输方向（从B 到 A）的 TCP 报文段的序号值也具有相同的含义。
- 32 位确认号（acknowledgement number）：用作对另一方发送来的 TCP 报文段的响应。其值是收到的 TCP 报文段的序号值 + 标志位长度（SYN，FIN） + 数据长度 。假设主机 A 和主机 B 进行TCP 通信，那么 A 发送出的 TCP 报文段不仅携带自己的序号，而且包含对 B 发送来的 TCP 报文段的确认号。反之，B 发送出的 TCP 报文段也同样携带自己的序号和对 A 发送来的报文段的确认序号。
- 4 位头部长度（head length）：标识该 TCP 头部有多少个 32 bit(4 字节)。因为 4 位最大能表示15，所以 TCP 头部最长是60 字节。
- 6 位标志位包含如下几项：
    - URG 标志，表示紧急指针（urgent pointer）是否有效。
    - ACK 标志，表示确认号是否有效。我们称携带 ACK 标志的 TCP 报文段为确认报文段。
    - PSH 标志，提示接收端应用程序应该立即从 TCP 接收缓冲区中读走数据，为接收后续数据腾出空间（如果应用程序不将接收到的数据读走，它们就会一直停留在 TCP 接收缓冲区中）。
    - RST 标志，表示要求对方重新建立连接。我们称携带 RST 标志的 TCP 报文段为复位报文段。
    - SYN 标志，表示请求建立一个连接。我们称携带 SYN 标志的 TCP 报文段为同步报文段。
    - FIN 标志，表示通知对方本端要关闭连接了。我们称携带 FIN 标志的 TCP 报文段为结束报文段。
    - 16 位窗口大小（window size）：是 TCP 流量控制的一个手段。这里说的窗口，指的是接收通告窗口（Receiver Window，RWND）。它告诉对方本端的 TCP 接收缓冲区还能容纳多少字节的数据，这样对方就可以控制发送数据的速度
    - 16 位校验和（TCP checksum）：由发送端填充，接收端对 TCP 报文段执行 CRC 算法以校验TCP 报文段在传输过程中是否损坏。注意，这个校验不仅包括 TCP 头部，也包括数据部分。这也是 TCP 可靠传输的一个重要保障。
    - 16 位紧急指针（urgent pointer）：是一个正的偏移量。它和序号字段的值相加表示最后一个紧急数据的下一个字节的序号。因此，确切地说，这个字段是紧急指针相对当前序号的偏移，不妨称之为紧急偏移。TCP 的紧急指针是发送端向接收端发送紧急数据的方法

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514160023.png)

```
第一次握手：
    1. 客户端将SYN标志位置为1
    2. 生成一个随机的32位的序号，这个序号后边是可以携带数据（数据的大小）
第二次握手：
    1. 服务器端接收客户端的连接：ACK = 1；
    2. 服务器会回发一个确认序号：ack = 客户端的序号 + 数据长度 + SYN/FIN(按一个字节算)
    3. 服务端会向客户端发起一个连接请求：SYN = 1
    4. 服务器会生成一个随即序号：seq == K
第三次握手：
    1. 客户端应答服务器的连接请求：ACK = 1;
    2. 客户端回复收到了服务器端的数据：ack = 服务端的序号 + 数据长度 + SYN/FIN(按一个子结算)
```
## TCP滑动窗口

滑动窗口（Sliding window）是一种流量控制技术。早期的网络通信中，通信双方不会考虑网络的拥挤情况直接发送数据。由于大家不知道网络拥塞状况，同时发送数据，导致中间节点阻塞掉包，谁也发不了数据，所以就有了滑动窗口机制来解决此问题。滑动窗口协议是用来改善吞吐量的一种技术，即容许发送方在接收任何应答之前传送附加的包。接收方告诉发送方在某一时刻能送多少包（称窗口尺寸）。

TCP 中采用滑动窗口来进行传输控制，滑动窗口的大小意味着接收方还有多大的缓冲区可以用于接收数据。发送方可以通过滑动窗口的大小来确定应该发送多少字节的数据。当滑动窗口为 0时，发送方一般不能再发送数据报。

滑动窗口是 TCP 中实现诸如 ACK 确认、流量控制、拥塞控制的承载结构。
![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514190115.png)

```
# mss: Maximum Segment Size(一条数据的最大的数据量)
# win: 滑动窗口
1. 客户端向服务器发起连接，客户端的滑动窗口是4096，一次发送的最大数据量是1460
2. 服务器接收连接情况，告诉客户端服务器的窗口大小是6144，一次发送的最大数据量是1024
3. 第三次握手
4. 4-9 客户端连续给服务器发送6k的数据，每次发送1k
5. 第10次，服务器告诉客户端：发送的6k数据已经接收到，存储在缓存区中，缓存区数据已经处理了2k，窗口大小是2k
6. 第11次，服务器告诉客户端：发送的6k数据已经接收到，存储在缓冲区中，缓冲区数据已经处理了4k，窗口大小是4k
7. 第12次，客户端给服务器发送了1k的数据
8. 第13次，客户端主动请求和服务器端断开连接，并且给服务器发送1k的数据
9. 第14次，服务器回复ACK 8194，a：同意断开连接的请求，b：告诉客户端已经接收到方才的2k的数据，c：滑动窗空2k
10. 第15、16次，通知客户端滑动窗口的大小
11. 第17次，第三次挥手，服务器端给客户端发送FIN，请求断开连接
12. 第18次，第四次挥手，客户端同意了服务器端的断开
```

## TCP四次挥手

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230514194417.png)

```
四次挥手发生在断开连接的时候，在程序中调用了close()会使用TCP协议进行四次挥手
客户端和服务器端都可以主动发起断开连接，谁先调用close()谁就是发起者
因为在TCP连接的时候，采用三次握手建立的连接是双向的，在断开的时候也需要双向断开
```

## TCP通信并发
```
要实现TCP通信服务器处理并发的任务，使用多线程或者多进程来解决

思路：
    1.一个父进程，多个子进程
    2.父进程负责等待并接受客户端的连接
    3.子进程：完成通信，接受一个客户端连接，就创建一个子进程用于通信
```

### 多进程实现并发服务器

```cpp
//客户端
#include<unistd.h>
#include<stdio.h>
#include<arpa/inet.h>
#include<stdlib.h>
#include<string.h>

int main(){
    // 1. 创建套接字
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    if(fd == -1){
        perror("socket");
        exit(-1);
    }

    // 2. 连接服务器端
    struct sockaddr_in serveraddr;
    inet_pton(AF_INET, "172.28.117.186", &serveraddr.sin_addr.s_addr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_port = htons(9999);
    int ret = connect(fd, (struct sockaddr *)&serveraddr, sizeof(serveraddr));

    if(ret == -1){
        perror("connect");
        exit(-1);
    }

    // 3. 通信
    char recvBuf[1024] = {0};
    int i = 0;
    while(1){
        sprintf(recvBuf, "data : %d\n", i);

        // 给服务端发送数据
        write(fd, recvBuf, strlen(recvBuf) + 1);
        i++;
        
        // 从服务端读数据
        int recvLen = read(fd, recvBuf, sizeof(recvBuf));
        if(recvLen == -1){
            perror("read");
            exit(-1);
        }else if(recvLen == 0){
            printf("server closed...");
        }else if(recvLen > 0){
            printf("recv server data : %s\n", recvBuf);
        }
        sleep(1);
    }
    // 4. 关闭文件描述符
    close(fd);

    return 0;
}
```

**注意**：sleep需要放在read和write的同侧，不能放在这俩之间，不然会报错`read: Connection reset by peer`，放在中间的时候如果终端终止客户端，即终止客户端的socket，如果另一端仍发送数据，发送的第一个数据包引发该异常(Connect reset by peer).

```cpp
// 服务端
#include<stdio.h>
#include<arpa/inet.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>
#include<sys/types.h>
#include<signal.h>
#include<wait.h>
#include<errno.h>

void recycleChild(int arg){
    while(1){
        int ret = waitpid(-1, NULL, WNOHANG);
        if(ret == -1){
            break;
            //所有子进程都回收
        }else if(ret == 0){
            break;
            // 还有子进程活着
        }else if(ret > 0){
            printf("子进程 %d 被回收了\n", ret);
        }
    }
}

int main(){

    // 注册信号捕捉
    struct sigaction act;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);
    act.sa_handler = recycleChild;

    sigaction(SIGCHLD, &act, NULL);

    // 创建socket
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if(lfd == -1){
        perror("socket");
        exit(-1);
    }

    // 绑定
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(9999);
    saddr.sin_addr.s_addr = INADDR_ANY;

    int ret = bind(lfd, (struct sockaddr*)&saddr, sizeof(saddr));
    if(ret == -1){
        perror("bind");
        exit(-1);
    }

    // 监听
    ret = listen(lfd, 128);
    if(ret == -1){
        perror("listen");
        exit(-1);
    }

    // 不断循环等待连接
    while(1){
        // 接受连接
        struct sockaddr_in clientaddr;
        int len = sizeof(clientaddr);
        int cfd = accept(lfd, (struct sockaddr*)&clientaddr, &len);
        if(cfd == -1){
            if(errno == EINTR){
                // printf("1\n");
                continue;
            }
            perror("accept");
            exit(-1);
        }
        pid_t pid = fork();
        if(pid == 0){
            // 子进程
            // 获取客户端的信息
            char clientIP[16];
            inet_ntop(AF_INET, &clientaddr.sin_addr.s_addr, clientIP, sizeof(clientIP));
            unsigned short clientPort = ntohs(clientaddr.sin_port);
            printf("client ip is : %s, port is %d \n", clientIP, clientPort);

            // 接受客户端发来的数据
            char recvBuf[1024] = {0};
            while(1){
                int recvLen = read(cfd, &recvBuf, sizeof(recvBuf));

                if(len == -1){
                    perror("read");
                    exit(-1);
                }else if(len == 0){
                    printf("client closed...\n");
                    break;
                }else{
                    printf("recv data : %s\n", recvBuf);
                }

                write(cfd, recvBuf, strlen(recvBuf));
            }
            close(cfd);
            exit(0);
        }
    }
    close(lfd);
    return 0;
}
```

### 多线程实现并发服务器

```cpp
#include<stdio.h>
#include<arpa/inet.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>
#include<sys/types.h>
#include<pthread.h>

struct sockInfo{
    int fd; //通信的文件描述符
    struct sockaddr_in addr;
    pthread_t tid;  // 线程号
};

struct sockInfo sockinfos[128];

void *working(void *arg){
    struct sockInfo *pinfo = (struct sockInfo *)arg;
    char clientIP[16];
    inet_ntop(AF_INET, &pinfo->addr.sin_addr.s_addr, clientIP, sizeof(clientIP));
    unsigned short clientPort = ntohs(pinfo->addr.sin_port);
    printf("client ip is : %s, port is %d \n", clientIP, clientPort);

    // 接受客户端发来的数据
    char recvBuf[1024] = {0};
    while(1){
        int len = read(pinfo->fd, &recvBuf, sizeof(recvBuf));

        if(len == -1){
            perror("read");
            exit(-1);
        }else if(len == 0){
            printf("client closed...\n");
            break;
        }else if(len > 0){
            printf("recv data : %s\n", recvBuf);
        }

        write(pinfo->fd, recvBuf, strlen(recvBuf) + 1);
    }
    close(pinfo->fd);
    return NULL;
}

int main(){
    // 创建socket
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if(lfd == -1){
        perror("socket");
        exit(-1);
    }

    // 绑定
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    saddr.sin_port = htons(9999);
    saddr.sin_addr.s_addr = INADDR_ANY;

    int ret = bind(lfd, (struct sockaddr*)&saddr, sizeof(saddr));
    if(ret == -1){
        perror("bind");
        exit(-1);
    }

    // 监听
    ret = listen(lfd, 128);
    if(ret == -1){
        perror("listen");
        exit(-1);
    }

    // 初始化数据
    int max = sizeof(sockinfos) / sizeof(sockinfos[0]);
    for(int i = 0; i < max; i++){
        bzero(&sockinfos[i], sizeof(sockinfos[i]));
        sockinfos[i].fd = -1;
        sockinfos[i].tid = -1;
    }

    // 循环等待客户端连接
    while(1){
        // 接受连接
        struct sockaddr_in clientaddr;
        int len = sizeof(clientaddr);
        int cfd = accept(lfd, (struct sockaddr*)&clientaddr, &len);

        struct sockInfo *pinfo;
        for(int i = 0; i < max; i++){
            if(sockinfos[i].fd == -1){
                pinfo = &sockinfos[i];
                break;
            }
            if(i == max - 1){
                sleep(1);
                i = 0;
            }
        }
        pinfo->fd = cfd;
        memcpy(&pinfo->addr, &clientaddr, len);
        pthread_create(&pinfo->tid, NULL, working, pinfo);

        // 回收资源，设置线程分离
        pthread_detach(pinfo->tid);
    }
    close(lfd);
    return 0;
}
```

需要注意的几个点
1. 为什么要设置全局变量sockInfo
    如果设置的是局部变量，那么在循环结束的时候，栈需要回收该内存空间，那么我们传给working的地址就是无效的，线程不能够获得参数
2. 对全局变量sockInfo初始化
    如果不进行初始化会怎么样？也许我们就不知道这个变量是否有被使用从而导致多个线程共用一个变量

遗留的问题：
&emsp;&emsp;回收线程后怎么将对应的sockInfo重新初始化


## TCP状态转换


什么是半关闭？
> 当TCP链接中A向B发送FIN请求关闭，另一端B回应ACK之后（A端静茹FIN_WAIT_2状态），并没有立即发送FIN给A，A方处于半连接状态（半开关），此时A可以继续接受B发送的数据，但是A已经不能再向B发送数据

从程序的角度，可以使用API来控制实现半连接状态：
```cpp
#include<sys/socket.h>
int shutdown(int sockfd, int how);
sockfd: 需要关闭的socket的描述符
how：   允许为shutdown操作选择以下几种方式：
    SHUT_RD(0)：关闭sockfd上的读功能，此选项将不允许sockfd进行读操作
                该套接字不再接收数据，任何当前在套接字接受缓冲区的数据将被无声的丢掉
    SHUT_WR(1): 关闭sockfd上的写功能，此选项将不允许sockfd进行写操作。进程不能再对此套接字发出写操作
    SHUT_RDWR(2): 关闭sockfd上的读写功能。相当于调用shutdown两次：首先是SHUT_RD，然后是SHUT_WR。
```

使用close中止一个连接，但它只是减少描述符的引用计数，并不直接关闭连接，只有当描述符的引用计数为0时才关闭连接。shutdown不考虑描述符的引用计数，直接关闭描述符。也可以选择中止一个方向的连接，只中止读或者写
**注意**
1. 如果有多个进程共享一个套接字，close每被调用一次，计数减1，直到计数为0时，也就是所用进程都调用了close，套接字将被释放
2. 在多进程中如果一个进程调用了shutdown(sfd, SHUT_RDWR)后，其它的进程将无法进行通信。但如果一个进程close(sfd)将不会影响到其它进程


## 端口复用
> 端口复用最常用的用途是
> - 防止服务器重启时之前绑定的端口还未释放
> - 程序突然退出而系统没有释放端口

```cpp
#include<sys/types.h>
#include<sys/socket.h>
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
    参数：
        - sockfd：要操作的文件描述符
        - level：级别 - SQL_SOCKET（端口复用的级别）
        - optname：选项的名称
            - SO_REUSEADDR
            - SO_REUSEPORT
        - optval：端口复用的值（整型）
            - 1：可以复用
            - 0： 不可以复用
        - optlen：optval参数的大小
端口复用，设置的时机是在服务器绑定端口之前
setsockopt();
bind();
```

查看网络相关信息的命令
netstat
&emsp;&emsp;参数
&emsp;&emsp;&emsp;&emsp;-a 所有的socket
&emsp;&emsp;&emsp;&emsp;-p 显示正在使用socket的程序的名称
&emsp;&emsp;&emsp;&emsp;-n 直接使用IP地址，而不通过域名服务器
