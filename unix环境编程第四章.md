# 文件和目录

## 4.1函数stat, fstat, fstatat和 lstat

```c
#include <sys/stat.h>
int stat(const char * restrict pathname, struct stat *restrict buf);
int fstat(int fd, struct stat *buf);
int lstat(const char * restrict pathname, struct stat *restrict buf);
int fstatat(int fd, const char *restrict pathname, struct stat *restrict buf, int flag);
```

返回值：成功，返回0；失败，返回-1

stat：返回此文件有关的信息结构

fstat：获得已在描述符fd上打开文件的有关信息

lstat：返回此文件有关的信息结构，包含符号链接 

fstatat：相对获得已在描述符fd上打开文件的有关信息

### stat结构参数

```c
struct stat {
    mode_t		st_mode;
    ino_t		st_ino;
    dev_t		st_dev;
    dev_t		st_rdev;
    nlink_t		st_nlink;
    uid_t		st_uid;
    gid_t		st_gid;
    off_t		st_size;
   	struct timespec		st_atime;
    struct timespec		st_mtime;
    struct timespec		st_ctime;
    blksize_t	st_blksize;
    blkcnt_t	st_blocks;
}
```

## 4.2文件类型(struct stat.st_mode)

- 普通文件(S_ISREG())
- 目录文件(S_ISDIR())
- 块特殊文件(S_ISBLK())
- 字符特殊文件(S_ISCHR())
- FIFO(S_ISFIFO())
- 套接字(S_ISSOCK())
- 符号链接(S_ISLNK())

## 4.3文件访问权限

| stat.st_mode | Desc |
| :---- | :---- |
| S_IRUSR | 用户读 |
| S_IWUSR | 用户写 |
| S_IXUSR | 用户执行 |
| S_IRGRP | 组读 |
| S_IWGRP | 组写 |
| S_IXGRP | 组执行 |
| S_IROTH | 其他读 |
| S_IWOTH | 其他写 |
| S_IXOTH | 其他执行 |

## 4.4函数access和faccessat

```c
#include <unistd.h>
int access(const char *pathname, int mode);
int faccessat(int fd, const char *pathname, int mode, int flag);
```

返回值：成功，返回0；失败，返回-1

Func：判断实际用户ID和实际组ID是否有相应权限

faccessat特殊说明：如果flag设置为AT_EACCESS检查调用进程的有效用户ID和有效组ID

### mode参数

| mode | Desc             |
| ---- | ---------------- |
| R_OK | 测试读权限       |
| W_OK | 测试写权限       |
| X_OK | 测试执行功能权限 |
| F_OK | 文件存在         |

## 4.5函数umask

```c
#include <sys/stat.h>
mode_t umask(mode_t cmask);
```

返回值：返回文本模式创建屏蔽字

### 屏蔽位

| 屏蔽位 | 含义     |
| ------ | -------- |
| 0400   | 用户读   |
| 0200   | 用户写   |
| 0100   | 用户执行 |
| 0040   | 组读     |
| 0020   | 组写     |
| 0010   | 组执行   |
| 0004   | 其他读   |
| 0002   | 其他写   |
| 0001   | 其他执行 |

 ## 4.6 函数chomd、fchmod和fchmodat

```c
#include <sys/stat.h>
int chmod(const char *pathname, mode_t mode);
int fchmod(int fd, mode_t mode);
int fchmodat(int fd, const char *pathname, mode_t mode, int flag);
```

返回值：成功，返回0；失败，返回-1

Func：设置文件权限

### mode参数

| mode     | Desc               |
| -------- | ------------------ |
| S_ISUID  | 执行设置用户id     |
| S_ISGID  | 执行设置组id       |
| S_ISVTX  | 保存正文（粘着位） |
| S_ISRWXU | 用户读、写和执行   |
| S_IRUSR  | 读                 |
| S_IWUSR  | 写                 |
| S_IXUSR  | 执行               |
| S_IRWXG  | 组读、写和执行     |
| S_IRGRP  | 读                 |
| S_IWGRP  | 写                 |
| S_IXGRP  | 执行               |
| S_IRWXO  | 其他读、写和执行   |
| S_IROTH  | 读                 |
| S_IWOTH  | 写                 |
| S_IXOTH  | 执行               |

## 4.7 粘着位特性

如果对一个目录设置了粘着位，拥有以下点才能删除或重命名文件：

- 拥有此文件
- 拥有此目录
- 是超级用户

## 4.8 函数 chown、fchown、fchownat和lchown

```c
#include <unistd.h>
int chown(const char *pathname, uid_t owner, gid_t group);
int fchown(int fd, uid_t owner, gid_t group);
int fchownat(int fd, const char *pathname, uid_t owner, gid_t group, int flag);
int lchown(const char *pathname, uid_t owner, gid_t group);
```

返回值：成功，返回0；失败，返回-1

Func：修改文件所有者

## 4.9文件截断

```c
#include <unistd.h>
int truncate(const char *pathname, off_t length);
int ftruncate(int fd, off_t length);
```

