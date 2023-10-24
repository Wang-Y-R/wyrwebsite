# CPL 与 一些C 的语法
- 前言
- 没有包含所有学习的内容
- 仅仅包含部分不熟悉或教材还没看到的内容，以及上网学习的内容
- 自用，仅供参考


## CMake写法

```c
cmake_minimum_required(VERSION 3.25) :现在要用cmake 最小版本是3.25

project(2023hello C) :文件名字以及语言

set(CMAKE_C_STANDARD 17) :C的标准

add_executable(hello hello.c) : 前面那个是编译后的名字，后面是要编译的文件（要编译的文件）

//用于说明它管理下列文件夹中的camke文件,则文件夹下cmake文件只需要添加可执行文件
add_subdirectory(0-intro)  
add_subdirectory(1-types-io)

//linux系统中可能出现库函数找不到的解决方法
target_link_library(admin.c m)
```

## 输入

### gets函数   from  ``<stdio.h>``

 `char *gets(char *str);`
 
 >效果简析:读入一行字符串(包括空格,不包括换行符),并清除缓冲区的换行符.
 
从输入缓冲区中读取一个字符串存储到字符指针变量 str 所指向的内存空间，并且***可以直接输入带空格的字符串***。
  
``` c
include <stdio.h>
int main(void)
{
    char str[30];
    char *string = str;  //一定要先将指针变量初始化
    printf("请输入字符串：");
    gets(string);  //也可以写成gets(str);
    printf("%s\n", string);  //输出参数是已经定义好的“指针变量名”
    return 0;
}

/***************
*输入:I LOVE C *
*输出:I LOVE C *
****************/
```

此外，关于使用 gets() 函数需要注意：使用 gets() 时，***系统会将最后“敲”的换行符从缓冲区中取出来，然后丢弃，所以缓冲区中不会遗留换行符***。这就意味着，如果前面使用过 gets()，而后面又要从键盘给字符变量赋值的话就不需要吸收回车清空缓冲区了，因为缓冲区的回车已经被 gets() 取出来扔掉了。

### fgets函数

``char *fgets(char *restrict str, int size, FILE *restrict stream)）``

 >效果简析:读入一行字符串(包括空格,若字符数小于`size-1`,则包括换行符),并在`size`加上'\0'
 
这个函数原型不太好看出个所以然来，可以理解为（char *fgets(“容器的地址”， “容器的大小”， “从哪里读取”)

```c
char a[100] = {0};  
fgets(a, 100, stdin); //stdin 指缓冲区
```

从第三个参数指定的流中读取最多第二个参数大小的字符到第一个参数指定的容器地址中。在这个过程中，**在还没读取够第二个参数指定大小的字符前，读取到`换行符'\n'`或者需要读取的流中已经没有数据了。则提前结束**，并把已经读取到的字符存储进第一个参数指定的容器地址中。

>`fgets()`函数的最大读取大小是其“`第二个参数减1`”，这是由于字符串是以`’\0’`为结束符的，`fgets()`为了保证输入内容的`字符串格式`，当输入的数据大小超过了`第二个参数`指定的大小的时候，`fgets()`会仅仅读取前面的“`第二个参数减1`”个字符，而预留`1个字符`的空间来存储字符串结束符`’\0’`。

>重点:与gets不同,当fgets读取的字符数小于n-1,则\n也会被读入进字符串内作为一个普通字符,printf时会打印回车.

## 字符处理

### ctype.h

#### 字符分类函数（返回0/1）(常用)

- isalnum(c) 是否为字母或数字


- isdigit(c) 是否是数字


- isalpha(c) 是否是字母
  > islower(c) 是否是小写字母
  > 
  > isupper(c) 是否是大写字母

- ispunct(c) 是否是标点符号

#### 字符大小写映射函数

- tolower(c) 将大写字母输入返回小写字母,其他字符不变

- toupper(c) 将小写字母输入返回大写字母,其他字符不变

### string.h(会用的)

- strcpy(s1,s2) 将s2字符串复制给s1
>strncpy(s1,s2,n)将s2字符串最多复制n位给s1,建议n等于sizeof(s1)-1

- strcat(s1,s2) 将s2字符串内容追加到s1
>strncat(s1,s2,n)将s2字符串最多n位内容追加到s1,建议n等于sizeof(s1)-sizeof(s2)-1

- strcmp(s1,s2) 比较s1和s2字符串
> 比较顺序:每位ASCLL码顺位比较;位数大小比较,小于等于大于分别返回一个小于等于大于0的值

- strlen(s) 返回字符串长度,数到'\0',空字符不计入

- strchr(s,'a') 从字符串中从左向右搜索第一个字符'a'，并返回从这个'a'的指针,无则返回NULL
> strrchr 表示从右向左搜索.

- strstr字符串中搜索字符串，还没遇见过，用上了再说

- memset(*s,c,n),将字符c/数值c存储到字符串s/数组,从首位开始存n位
> 将数组全部初始化为0 
> `memest(a,0,sizeof(a))`

### stdlib.h

- atoi(): int atoi(const char *str )将字符串转化为数字，直到遇见一个不是数字的字符

## 文件输入输出

<img src="..\photos\file.jpg" alt="file" style="width: 100%; height: 100%;">

<img src="..\photos\fopen.jpg" alt="fopen" style="width: 100%; height: 1010%;">


## 其它

### 静态本地变量与不公开对外变量/函数

- ``static int a``  在函数中静态本地变量,***仅在第一次进行初始化***,离开函数时依然存在,下一次进入函数值为上一次离开时的值.***具有全局生存期，但作用域仅在此函数（与全局变量类似但不同）***
- 若在全局变量面前加上``static`` 则是说明该全局变量不在同项目所有.c文件共享,仅在当下本文件使用,函数同理,只能在本编译单元中被使用.

### 头文件

- XXXX.h文件 仅仅是插入到.c文件中
- 用来放函数声明,这些函数并不在这个.c文件中,需要声明
- 也可以放全局变量声明,用于多个.c文件共享变量 如``extern int a;``
> 注:声明和定义不是同种东西
>
> 声明不会产生代码,但定义会. 如extern int a是声明 int a是定义

- 标准头文件的防卫式声明:采用 #ifndef、#define 和 #endif 的组合来确保头文件只被编译一次。(比如防止``struct``被多次定义)
```c
//某个.h头文件中
#ifndef __LIST_HEAD__ //如果还没插入过就插入,否则就算了
#define __LIST_HEAD__

#include "node.h" //还引入了其他头文件的声明

typedef struct _list {
  Node* head;
  Node* tail;
} List;

#endif //结束位置标志

```

### 宏

#### 预定义宏

- ``__FILE__``  文件名字符串
- ``__LINE__``  当前行号
- ``__DATE__``  编译日期
- ``__TIME__``  编译时间 

#### 带参数的宏

```c
#define cube(x) ((x)*(x)*(x))
printf("%d",cube(5)); //printf("%d",5*5*5);
输出:125
```

> 注意:本质没有变化,还是文本替换,注意替换文本后运算顺序
>
> 一切都要有括号,整个值有括号,每个参数有括号以防止替换后不符合预期
>
> 支持多参数 如 ``#define MIN(a,b) ((a)>(b)?(b):(a))``

