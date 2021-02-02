

# 第六章 系统数据文件和信息

## 6.1 口令文件

**struct passwd结构**

```c
struct passwd {
    char *pw_name, //用户名
    char *pw_passwd, //加密口令
    uid_t pw_uid, //数值用户ID
    gid_t pw_gid, //数值组ID
    char *pw_gecos, //注释字段
    char *pw_dir, //初始工作目录
    char *pw_shell //初始shell 
}
```

**Func：根据登录名称和数值用户ID获取struct passwd**

```c
#include <pwd.h>
struct passwd *getpwuid(uid_t uid);
struct passwd *getpwnam(const char *name);
					//返回值：成功，返回指针；失败，返回NULL
```

**Func：查看真个口令文件**

 ```c
#include <pwd.h>
struct passwd *getpwent(void);
					// 返回值：成功，返回指针；失败，返回NULL
void setpwent(void);
void endpwent(void);
 ```

## 6.2 阴影口令文件

**struct spwd结构**

```c
struct spwd {
    char *sp_namp,
    char *sp_pwdp,
    int sp_lstchg, // 上次更改口令时间
    int sp_min, //经多少天口允许更改
    int sp_max, //要求更改尚余天数
    int sp_warn, //超期警告天数
    int sp_inact, //账户不活动之前尚余天数
    int sp_exprie, //账户超期天数
    unsigned int sp_flag //保留
}
```



```c
#include <shadow.h>
struct spwd *getspnam(const char *name);
struct spwd *getspent(void);
					//返回值：成功，返回指针；失败，返回NULL
void setspent(void);
void endspent(void);
```

## 6.3 时间和日期例程

**Func：time函数返回当前时间和日期**

```c
#include <time.h>
time_t time(time_t *calptr);
					//返回值：成功，返回时间值；失败，返回-1
```

**Func：获取指定时钟的时间**

```c
#include <sys/time.h>
int clock_gettime(clockid_t clock_id, struct timespec *tsp);
int clock_getres(clockid_t clock_id, struct timespec *tsp);
int clock_settime(clockid_t clock_id, const struct timespec *tsp);
int gettimeofday(struct timeval *restrict tp, void *restrict tzp);
					//返回值：成功，返回时间值；失败，返回-1
```



```c
#include <time.h>
struct tm *gmtime(const time_t *calptr);
struct tm *localtime(const time_t *calptr);
					//返回值：成功，指向tm结构指针；失败，NULL
time_t mktime(struct tm *tmptr);
					//返回值：成功，返回日历时间；失败，-1
size_t strftime(char *restrict buf, size_t maxsize,
               const char *restrict format,
               const struct tm *restrict tmptr);
					//返回值：成功，返回存入数组的字符数；失败，返回0
char *strptime(const char *restrict buf, const char *restrict format,
              struct tm * restrict tmptr);
					
```

