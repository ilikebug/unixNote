# 第三章 文件I/O

## 3.1文件描述符

文件描述符是一个非负整数。

`STDIN_FILENO	STDOUT_FILENO	STDERR_FILENO`

## 3.2 函数open和openat

```c
#include <fcntl.h>
int open(const char *path, int oflag, .../*mode_t mode */);
int openat(int fd, const char *path, int oflag, .../*mode_t mode */);

```

功能：打开文件

返回值：成功，返回文件描述符，出错，返回-1

### oflag参数

| Code        | Desc                                                         |
| ----------- | ------------------------------------------------------------ |
| O_RDONLY    | 只读打开                                                     |
| O_WRONLY    | 只写打开                                                     |
| O_RDWR      | 读写打开                                                     |
| O_EXEC      | 只执行打开                                                   |
| O_APPEND    | 追加到文件尾端                                               |
| O_CREAT     | 文件不存在创建                                               |
| O_DIRECTORY | 如果path引用不是目录，则出错                                 |
| O_EXCL      | 如果同时指定O_CREAT,文件存在，则出错                         |
| O_NOCTTY    | 如果path引用的是终端设备，则不将该设备作为此进程的控制终端   |
| O_NOFOLLOW  | 如果path引用的是一个符号链接，则出错                         |
| O_NONBLOCK  | 如果path引用是一个FIFO，一块特殊文件，一个字符特殊文件后续的IO操作设置非阻塞方式 |
| O_SYNC      | 每次writer等待物理IO操作完成                                 |
| O_TRUNC     | 如果文件存在，而且为只写或读成功打开，则将长度截断为0        |
| O_DSYNC     | 每次writer等待物理IO操作完成，如果操作不影响读取刚写入的数据，则不需要等待文件属性被更新 |
| O_RSYNC     | read操作等待，直至写操作完成                                 |

## 3.3 函数creat

```c
#include <fcntl.h>
int creat(const char *path, mode_t mode);
```

等同于

```c
open(path, O_WRONLY|O_CREAT|O_TRUNC, mode);
```

功能：创建文件

返回值：成功，返回只写打开的文件描述符，出错，返回-1

## 3.4函数close

```c
#include <unistd.h>
int close(int fd);
```

功能：关闭打开文件

返回值： 成功，返回0；失败，返回-1

## 3.5函数lseek

```c
#include <unistd.h>
off_t lseek(int fd, off_t offset, int whence);
```

功能：修改文件偏移量

返回：成功，返回新的文件偏移量；失败，返回-1

### whence参数

| Code | Desc |
| :---- | :---- |
| SEEK_SET | 设置距离文件开始处offset字节 |
| SEEK_CUR | 文件的偏移量设置为当前值加offset |
| SEEK_END | 设置距离文件结束处offset字节 |

### 示例

```c++
#include <unistd.h>
#include <cstdio>
#include <iostream>
#include <string>

int SEEK_INDEX = -1;

int main(void){
        if (lseek(STDIN_FILENO, 0, SEEK_CUR) == SEEK_INDEX)
                std::cout << "cannot seek\n";
        else
                std::cout << "seek ok\n";
}

```

## 3.6函数read

```c
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t nbytes);
```

功能：读数据

返回值：成功，读到的字节数；失败，返回-1；文件结尾，返回0

## 3.7函数writer

```c
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t bytes);
```

功能：写数据

返回值：成功，返回已写字节数；失败，返回-1

## 3.8函数dup和dup2

```c
#include <unistd.h>
int  dup(int fd);
int dup2(int fd, int fd2);
```

功能：复制文件描述符

返回值：成功，返回新的文件描述符；失败，返回-1

## 3.9函数sync，fsync和fdatasync

```c
#include <unistd.h>
int fsync(int fd);
int fdatasync(int fd);
void sync(void);
```

功能：缓冲区管理

返回值：成功，返回0；失败，返回-1

sync： 刷新缓冲区

fsync：只对一个文件起作用，等带磁盘操作结束才会返回，并同步更新文件属性

fdatasync：只影响文件数据部分

## 3.10函数fcntl

```c
#include <fcntl.h>
int fcntl(int fd, int cmd, .../* int arg */);
```

功能：修改打开文件的属性

返回值：成功，跟设置属性有关；失败，返回-1

### cmd参数

| Code   | Desc   |
| :--- | :---- |
| F_DUPFD | 复制文件描述符 |
| F_DUPFD_CLOEXEC | 复制文件描述符，关联新的FD_CLOEXEC |
| F_GETFD | 获取文件描述符 |
| F_SETFD | 设置文件描述符 |
| F_GETFL | 获取文件状态 |
| F_SETFL | 设置文件状态 |
| F_GETOWN | 获取信号的进程ID |
| F_SETOWN | 设置信号的进程ID |
| F_GETLK | 获取文件锁 |
| F_SETLK | 设置文件 |
