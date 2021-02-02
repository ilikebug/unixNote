# 第五章 标准I/O

## 5.1 流和FILE对象

```c
#include <stdio.h>
#include <wchar.h>
int fwide(FILE *fp, int mode);
```

Func：设置流的定向

返回值：若流是宽定向的，返回正直；如流是字节定向的，返回值负值；若流未定向的，返回0

## 5.2 标准输入、标准输出和标准错误

**stdin,  stdout, stderr**

## 5.3 缓冲

- 全缓冲	_IOFBF

- 行缓冲    _IOLBF

- 不带缓冲    _IONBF

  ```c
  #include <stdio.h>
  void setbuf(FILE *restrict fp, char *restrict buf);
  int setvbuf(FILE *restrict fp, char *restrict buf, int mode, size_t size);
  ```

  Func：修改缓冲类型

  返回值：成功，返回0；失败，返回非0

```c
#include <stdio.h>
int fflush(FILE *fp);
```

Func：强制冲洗一个流

返回值：成功，返回0；失败，返回EOF

## 5.4 打开流

```c
#include <stdio.h>
FILE *fopen(const char *restrict pathname, const char *restrict type);
FILE *freopen(const char *restrict pathname, const char *restrict type, FILE *restrict fp);
FILE *fdopen(int fd, const char *type);
```

Func：打开流

返回值：成功，返回文件指针；失败，返回NULL

```c
#include <stdio.h>
int fclose(FILE *fp);
```

Func：关闭流

返回值：成功，返回0；失败，返回EOF

## 5.5 读和写流，字符操作

```c
#include <stdio.h>
int getc(FILE *fp);
int fgetc(FILE *fp);
int getchar(void);
```

Func：按字符读取流

返回值：成功，返回下一个字符；失败或者到达文件尾端，返回EOF

**<u>getc实现为宏，fgetc实现为函数，所以前者执行速度快</u>**

```c
#include <stdio.h>
int ferror(FILE *fp);
int feof(FILE *fp);

void clearerr(FILE *fp); //清除标志
```

Func：判断是读取错误还是到位

返回值：成功，返回非0，失败，返回0

```c
#include <stdio.h>
int ungetc(int c, FILE *fp);
```

Func：流中读取的数据，押送回流中

返回值：成功，返回c；失败，返回EOF

```c
#include <stdio.h>
int putc(int c, FILE *fp);
int fputc(int c, FILE *fp);
int putchar(int c);
```

Func：写流

返回值：成功，返回c；失败，返回EOF

## 5.6 读和写流，行操作

```c
#include <stdio.h>
char *fgets(char *restirct buf, int n, FILE *restrict fp);
char *gets(char *buf);
```

Func：行读取流

返回值：成功，返回buf；失败或者到达文件尾端，返回EOF

**gets不推荐使用，容易造成缓冲区溢出**

```c
#include <stdio.h>
int fputs(const char *restrict str, FILE *restrict fp);
int puts(const char *str);
```

Func ：写流

返回值：成功，返回非负值；失败，返回EOF

## 5.7 二进制I/O

```c
#include <stdio.h>
size_t fread(void *restrict ptr, size_t size, size_t nobj, FILE *restrict fp);
size_t fwrite(const void *restrict ptr, size_t size, size_t nobj, FILE * restrict fp);
```

Func：读和写

返回值：返回读和写对象数

## 5.8 定位流

```c
#include <stdio.h>
long ftell(FILE *fp);	//返回值：成功，返回当前文件位置；失败，返回-1L
int fseek(FILE *fp, long offset, int whence);	//返回值：成功，返回0；失败，返回-1
void rewind(FILE *fp);

off_t ftello(FILE *fp);	//返回值：成功，返回当前文件位置；失败，返回（off_t）-1
int fseeko(FILE *fp, off_t offset, int whence); //返回值：成功，0；失败，-1

int fgetpos(FILE *restrict fp, fpos_t *restrict pos);
int fsetpos(FILE *fp, const fpos_t *pos);
							//返回值：成功，返回0；失败，返回非0
```

## 5.9 格式化I/O

### 格式化输出

```c
#include <stdio.h>
int printf(const char*restrict format, ...);
int fprintf(FILE *restrict fp, const char *restrict format, ...);
int dprintf(int fd, const char *restrict format, ...);
						//返回值：成功，返回输出字符数；失败，返回负值
int sprintf(char *rstrict buf, const char *restrict format, ...);
					 	//返回值：成功，返回存入buf字符数；失败，返回负值
int snprintf(char *restrict buf, size_t n, const char *restrict format, ...);
						//返回值：若缓冲区足够大，返回存入buf字符数；失败，返回负值
```

### 格式化输入

```c
#include <stdio.h>
int scanf(const char *restrict format, ...);
int fscanf(FILE *restrict fp, const char *restrict format, ...);
int sscanf(const char *restrict buf, const char *restrict format, ...);
						//返回值：失败，返回EOF
```

## 5.10  临时文件

```c
#include <stdio.h>
char *tmpnam(char *ptr);
					//Func:创建临时文件夹
					//返回值：指向唯一路径的指针
FILE *tmpfile(void);
					//Func：创建临时文件
					//返回值：成功，返回文件指针；失败，返回NULL

char *mkdtemp(char *template);
					//Func:创建临时文件夹
					//返回值：成功，返回指向目录名的指针；失败，返回NULL
int mkstemp(char *template);
					////Func：创建临时文件
					//返回值：成功，返回文件描述符；失败，返回-1
```

## 5.11内存流

```c
#include <stdio.h>
FILE *fmemopen(void *restrict buf, size_t size, const char *restrict type);
					//返回值：成功，返回流指针；失败，返回NUll
```

