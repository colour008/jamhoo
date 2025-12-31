---
title: C语言刷题笔记🦆
date: 2023-02-11 18:00:00
updated: 2024-11-30 18:00:00
tag: [C语言,刷题笔记]
categories: [编程,C语言]
cover: 
description: C语言刷题笔记🦆
sticky: 4
swiper_index: 4
---

---

> [
> ![img](https://img.shields.io/badge/%E9%A2%98%E7%9B%AE%E6%9D%A5%E6%BA%90-RUNOOB-greenyellow.svg?logo=Notist&label=%E9%A2%98%E7%9B%AE%E6%9D%A5%E6%BA%90&logoColor=greenyellow&)](https://www.runoob.com/)[![img](https://img.shields.io/badge/%E9%A2%98%E7%9B%AE%E6%9D%A5%E6%BA%90-%20C%20%E8%AF%AD%E8%A8%80%E7%BD%91-purple.svg?logo=Notist&label=%E9%A2%98%E7%9B%AE%E6%9D%A5%E6%BA%90&logoColor=purple&)](https://www.dotcpp.com/)
>
> 本文题目转载自[菜鸟教程网](https://www.runoob.com/)和 [C 语言网](https://www.dotcpp.com/)，欢迎编程初学者访问学习！🌼

# 例题 1. [约瑟夫生者死者小游戏](https://www.runoob.com/cprogramming/c-examples-joseph-life-dead-game.html)

`题目描述：`30 个人在一条船上，超载，需要 15 人下船。于是人们排成一队，排队的位置即为他们的编号。报数，从 1 开始，数到 9 的人下船。如此循环，直到船上仅剩 15 人为止，问都有哪些编号的人下船了呢？

`解题关键：`

- 转变思路，对在船上和下船的赋予状态，例如：在船上的人的状态为 0，下船的人的状态为 1；

- 报数到第 10 个人时，报数重新变为 1，这是循环初始化的一个过程；

- 每个人所在的位置（或编号）是固定不变的，即使某个人下船了，他所在的位置在计数时也要算上，这样计数到 31 时，重新初始化为 1，所有人的编号不会发生变化，实现船头和船尾的人连接。

`示例代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int state[30] = {0}; // 将每个人状态初始化为0，表示在船上
    int count = 1;       // 报数
    int down = 0;        // 计下船的人数
    int people = 1;      // 人的编号
    while (down < 15)
    {
        if (people == 31) // 计数到31时,重新初始化为1，所有人的编号在进行到下一轮时将不会发生变化
        {
            people = 1;
        }
        if (state[people - 1] == 0) // 减1是因为索引值是从0开始，且这个人仍在船上时参与报数
        {
            count++;
            if (count == 10) // 报数到第10个人时，重新初始化为1进行下一轮报数
            {
                count = 1;
                state[people - 1] = 1; // 报9的倍数的那个人的状态变为1，不参与下次报数，下船人数+1
                printf("第%d号下船了\n", people);
                down++;
            }
        }
        people++; // 不管这个人是否还在船上，每个人的位置固定不变，编号是从1~30递增的
    }
    printf("留在船上的人的编号为：");
    for (int i = 0; i < 30; i++) // 遍历30个人的状态，当为0的时候，表示还在船上，依次输出
    {
        if (state[i] == 0)
        {
            printf("%-3d", i + 1);
        }
    }
    printf("\n");
    system("pause");
    return 0;
}
```
`运行结果：`

```css
第9号下船了
第18号下船了
第27号下船了
第6号下船了
第16号下船了
第26号下船了
第7号下船了
第19号下船了
第30号下船了
第12号下船了
第24号下船了
第8号下船了
第22号下船了
第5号下船了
第23号下船了
留在船上的人的编号为：1  2  3  4  10 11 13 14 15 17 20 21 25 28 29 
```

# 例题 2. [计算字符串中子串出现的次数](https://www.runoob.com/cprogramming/c-exercise-example96.html)

`题目描述：`输入一个长字符串（即**父串**）和一个短字符串（即**子串**，可与父串等长），计算子串在父串中出现的次数？

`示例代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int count(char *str1, int len1, char *str2, int len2)
{
    int i, j;
    int count = 0;
    if (len1 >= len2)
    {
        printf("父串为%s\n子串为%s\n", str1, str2);
        for (i = 0; i <= len1 - len2; i++) // i最大取len1-len2是防止当i取到len1-1且j取到len2-1时，str[i+j]已经超出str1的范围
        {
            for (j = 0; j < len2; j++)
            {
                if (str1[i + j] != str2[j]) // 遍历子串与父串一一比较，只要有不同就进入下一次循环
                {
                    break;
                }
            }
            if (j == len2) // 如果子串遍历的循环完整完成一轮，代表符合题目条件，则计数一次
            {
                count++;
            }
        }
    }
    else
    {
        printf("父串为%s\n子串为%s\n", str2, str1);
        for (i = 0; i <= len2 - len1; i++)
        {
            for (j = 0; j < len1; j++)
            {
                if (str2[i + j] != str1[j])
                {
                    break;
                }
            }
            if (j == len1)
            {
                count++;
            }
        }
    }
    return count;
}

int main()
{
    int count(char *str1, int len1, char *str2, int len2);
    char str1[100];
    char str2[100];
    printf("请输入两个字符串：\n");
    scanf("%s%s", str1, str2);
    int len1 = strlen(str1), len2 = strlen(str2);
    printf("子串出现了%d次\n", count(str1, len1, str2, len2));
    system("pause");
    return 0;
}
```
`运行结果：`

```css
请输入两个字符串：
12123112312231233
123
父串为12123112312231233
子串为123
子串出现了3次
```

# 例题 3. [无重复数字](https://www.runoob.com/cprogramming/c-exercise-example1.html)

`题目描述：`有 **1、2、3、4** 四个数字，能组成多少个互不相同且无重复数字的三位数？都是多少？

`示例代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int i, j, k, num, count = 0;
    for (i = 1; i <= 4; i++)
    {
        for (j = 1; j <= 4; j++)
        {
            for (k = 1; k <= 4; k++)
            {
                if (i != j && j != k && k != i)
                {
                    num = i * 100 + j * 10 + k;
                    printf("%-5d", num);
                    count++;
                    if (count % 10 == 0)
                    {
                        printf("\n");
                    }
                }
            }
        }
    }
    printf("\n共有%d个数\n", count);
    system("pause");
    return 0;
}
```
`运行结果：`

```css
123  124  132  134  142  143  213  214  231  234
241  243  312  314  321  324  341  342  412  413
421  423  431  432
共有24个数
```

# 例题 4. [计算提成](https://www.runoob.com/cprogramming/c-exercise-example2.html)

`题目描述：`企业发放的奖金根据利润提成。

- 利润 (I) 低于或等于 10 万元时，奖金可提 10%；
- 利润高于 10 万元，低于 20 万元时，低于 10 万元的部分按 10% 提成，高于 10 万元的部分，可提成 7.5%；
- 20 万到 40 万之间时，高于 20 万元的部分，可提成 5%；
- 40 万到 60 万之间时高于 40 万元的部分，可提成 3%；
- 60 万到 100 万之间时，高于 60 万元的部分，可提成 1.5%；
- 高于 100 万元时，超过 100 万元的部分按 1% 提成。

从键盘输入当月利润 I，求应发放奖金总数？

`示例代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
    float profit;
    printf("input your profits:");
    scanf("%f", &profit);
    float bonus, bonus1, bonus2, bonus3, bonus4, bonus5;
    bonus1 = 100000 * 0.1;
    bonus2 = bonus1 + 100000 * 0.075;
    bonus3 = bonus2 + 200000 * 0.05;
    bonus4 = bonus3 + 200000 * 0.03;
    bonus5 = bonus4 + 400000 * 0.015;
    if (profit <= 100000)
    {
        bonus = profit * 0.1;
    }
    else if (profit <= 200000)
    {
        bonus = bonus1 + (profit - 100000) * 0.075;
    }
    else if (profit <= 400000)
    {
        bonus = bonus2 + (profit - 200000) * 0.05;
    }
    else if (profit <= 600000)
    {
        bonus = bonus3 + (profit - 400000) * 0.03;
    }
    else if (profit <= 1000000)
    {
        bonus = bonus4 + (profit - 600000) * 0.015;
    }
    else
    {
        bonus = bonus5 + (profit - 1000000) * 0.01;
    }
    printf("bonus is %f\n", bonus);
    system("pause");
    return 0;
}
```
`运行结果：`

```css
input your profits:120000
bonus is 11500.000000
```

# 例题 5. 百鸡百钱问题

`题目描述：`中国古代数学家张丘建在他的《算经》中提出了一个著名的 “百钱百鸡问题”：一只公鸡（cock）值五钱，一只母鸡（hen）值三钱，三只小鸡（chick）值一钱，现在要用百钱买百鸡，请问公鸡、母鸡、小鸡各多少只？

`示例代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int cock, hen, chick;
    for (cock = 0; cock <= 20; cock++)
    {
        for (hen = 0; hen <= 33; hen++)
        {
            for (chick = 3; chick <= 99; chick++)
            {
                if (cock + hen + chick == 100 && 5 * cock + 3 * hen + chick / 3 == 100 && chick % 3 == 0)
                {
                    printf("cock=%2d只, hen=%2d只, chick=%2d只\n", cock, hen, chick);
                }
            }
        }
    }
    system("pause");
    return 0;
}
```
`运行结果：`

```css
cock= 0只, hen=25只, chick=75只
cock= 4只, hen=18只, chick=78只
cock= 8只, hen=11只, chick=81只
cock=12只, hen= 4只, chick=84只
```

# 例题 6. 水仙花数

`题目描述：`水仙花数（Narcissistic number）是指一个 n 位数 (n≥3)，它的每个位上的数字的 n 次幂之和等于它本身。例如 153 就是一个水仙花数，因为 153=13 + 53 + 33。求出 3 位数中的水仙花数是哪几个？

`示例代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int num;
    int i, j, k;
    for (num = 100; num < 1000; num++)
    {
        i = num / 1 % 10;   // 取个位
        j = num / 10 % 10;  // 取十位
        k = num / 100 % 10; // 取百位
        if (i * i * i + j * j * j + k * k * k == num)
        {
            printf("num=%d\n", num);
        }
    }
    system("pause");
    return 0;
}
```
`运行结果：`

```css
num=153
num=370
num=371
num=407
```

# 例题 7. [自定义函数之字符串连接](https://www.dotcpp.com/oj/problem1032.html)

`题目描述：`写一函数，将两个字符串连接。

`解题思路：`

> 首先，我想说既然题目中给出自己写一个函数将两个字符串连接，那么出题人的意图应该就是要考生自己编写一个函数实现与 `strcat` 函数同样的效果，而不是直接引用 `strcat` 函数，如果直接引用 `strcat` 函数，那么本题目将毫无测试价值。

**我的思路大致如下：**

- 输入两个字符串 `str_1` 和 `str_2`，且要求声明字符数组长度时 `str_1` 的长度大于和等于输入字符串后的 `str_1+str_2` 的长度。
- 构建函数传入两个字符串 `void montage_string(char *string_1, char *string_2)`，在函数内部声明 i = strlen (string_1), k = 0, j = strlen (string_1) + strlen (string_2)，其中 i 等于字符串 1 的长度，j 等于字符串 1 和字符串 2 长度的和。
- 从字符串 1 的最后一个字符 string_1 [i]（即’\0’）开始，将字符串 2 的第一个字符 string_2 [k] 赋值给字符串 1 的最后一个字符 string_1 [i]（即’\0’），实现拼接效果，即 string_1 [i++] = string_2 [k++]。while (i <= j) 时，在赋值运算结束以后，i 和 k 自增，当 i 增加到大于 j (即符串 1 和字符串 2 长度的和) 时，跳出循环。

`注意事项:`

在输入字符串时，`str_1+str_2` 的长度必须小于声明的 `str_1` 的长度，例如：最开始声明了 `str_1[7]`，那么输入的两端字符串可以是 `abc` 和 `123`，但不能是 `abcd` 和 `123` 或者 `abc` 和 `1234`，不然字符数组会溢出出现错误。

`示例代码:`

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void montage_string(char *string_1, char *string_2)
{
    int i = strlen(string_1), k = 0, j = strlen(string_1) + strlen(string_2);
    while (i <= j)
    {
        string_1[i++] = string_2[k++];
    }
}

int main()
{
    char str_1[1024], str_2[512];
    gets(str_1);
    gets(str_2);
    montage_string(str_1, str_2);
    puts(str_1);
    system("pause");
    return 0;
}
```
`运行结果：`

```css
abcd
1356wd
abcd1356wd
```

# 例题 8. [三个字符串的排序](https://www.dotcpp.com/oj/problem1044.html)

`题目描述：`输入三个字符串，按由小到大的顺序输出

`解题思路：`

> 重点知识点：**二维数组**、**指针数组**、**二级指针**、**strcmp 函数**

1. 声明一个二维字符数组 `char string[i][j]`；
2. 声明一个字符型指针数组 `char *p_str[i]`，数组内的每个指针先后指向二维字符数组每行的首地址，即 `p_str[i]=string[i]`；
3. 声明一个字符型二级指针 `**p`，指向指针数组的首地址，即 `p=p_str`；
4. 声明一个排序函数 `void sort`，分别传入字符型二级指针变量 `char **p` 和整型变量 `int n`，利用 `strcmp(str1,str2)` 函数两两比较字符串的大小从而进行排序。这里顺带提一下 `strcmp` 函数的知识点，该函数主要是根据字符串中字符对应的 ASCII 码值对字符串进行判断，然后通过返回函数值体现字符串的大小，不同的函数值对应字符串大小如下：

- 如果返回值为**正数**，则 **str1 > str2**；
- 如果返回值为 **0**，则 **str1 = str2**；
- 如果返回值为**负数**，则 **str1 < str2**。

1. 在利用 `strcmp` 函数进行比较排序时，需要声明一个字符型指针 `char *temp` 作为中间变量，然后通过双重循环和条件 `if (strcmp(p[i], p[j]) > 0){char *temp = p[i];p[i] = p[j];p[j] = temp;}` 交换指针数组的值，最终实现字符串从小到大的排序。

`注意事项：`

需要明确的一个知识点是，利用二级指针比较字符串大小然后进行交换，其实本质上交换的是指针数组的值，也就是指向字符串的地址，而字符串源数据本身不会发生交换。因此，在输出的时候应该输出一级指针 `p_str[i]` 而不是 `string[i]`。


`示例代码：`
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define SIZE 3 // 通过宏定义二维数组的行数

void sort(char **p, int n)
{
    char *temp;                 // 声明一个作为中间变量的指针
    for (int i = 0; i < n; i++) // 遍历每个指针数组内的指针
    {
        for (int j = i + 1; j < n; j++) // 从第二个开始与第一个进行比较
        {
            if (strcmp(p[i], p[j]) > 0) // 比较两个字符串的值，如果前面的大于后面的就进行交换（实际交换的是指向字符串的地址）
            {
                temp = p[i];
                p[i] = p[j];
                p[j] = temp;
            }
        }
    }
}

int main()
{
    char string[SIZE][1024]; // 声明二维数组
    char *p_str[SIZE];       // 声明指针数组
    char **p;                // 声明二级指针
    for (int i = 0; i < SIZE; i++)
    {
        /*  利用gets函数进行输入，相比较于scanf函数的优点就是gets函数会将空格等字符读取而不截断字符串，缺点就是输入一个字符串必须要按一次回车。使用scanf函数，可以使用scanf("%[^\n]s",string);语句来解决遇到空格时截断字符串的问题（%[^\n]是说明只有在读入回车符时才终止读取） */
        gets(string[i]);
        p_str[i] = string[i]; // 将每个字符串的首地址赋值给指针数组内的元素
    }
    p = p_str;     // 将指针数组的首地址赋值给二级指针
    sort(p, SIZE); // 进行排序
    for (int i = 0; i < SIZE; i++)
    {
        puts(p_str[i]); // 循环输出字符指针（因为对指针进行了交换，所以输出的值即为排序后的结果）
    }
    system("pause");
    return 0;
}
```
运行结果：
```css
// 排序前
cde
afg
abc
// 排序后
abc
afg
cde
```

`补充：`对于 `if (strcmp(p[i], p[j]) > 0){char *temp = p[i];p[i] = p[j];p[j] = temp;}` 存在疑虑（是交换的字符串还是字符串对应的地址），可以试试如下的代码，加深理解：

```cpp
#include <stdio.h>
#include <stdlib.h>

void swap(int **x, int **y)
{
    int *temp;
    if (**x < **y)
    {
        temp = *x;
        *x = *y;
        *y = temp;
    }
}

int main()
{
    int a, b;
    scanf("%d%d", &a, &b);
    int *pa = &a, *pb = &b;
    int **p_a = &pa, **p_b = &pb;
    printf("交换前a和b的值为：a=%d, b=%d\n", a, b);
    printf("交换前pa和pb的值为：pa=%x, pb=%x\n", pa, pb);
    printf("交换前*pa和*pb的值为：*pa=%d, *pb=%d\n", *pa, *pb);
    printf("交换前p_a和p_b的值为：p_a=%x, p_b=%x\n", p_a, p_b);
    printf("交换前*p_a和*p_b的值为：*p_a=%x, *p_b=%x\n", *p_a, *p_b);
    printf("交换前**p_a和**p_b的值为：**p_a=%d, **p_b=%d\n", **p_a, **p_b);
    swap(p_a, p_b);
    printf("-------------------------------------------------------\n");
    printf("交换后a和b的值为：a=%d, b=%d\n", a, b);
    printf("交换后pa和pb的值为：pa=%x, pb=%x\n", pa, pb);
    printf("交换后*pa和*pb的值为：*pa=%d, *pb=%d\n", *pa, *pb);
    printf("交换后p_a和p_b的值为：p_a=%x, p_b=%x\n", p_a, p_b);
    printf("交换后*p_a和*p_b的值为：*p_a=%x, *p_b=%x\n", *p_a, *p_b);
    printf("交换后**p_a和**p_b的值为：**p_a=%d, **p_b=%d\n", **p_a, **p_b);
    system("pause");
    return 0;
}
```
`运行结果：`

```css
2 3
交换前a和b的值为：a=2, b=3
交换前pa和pb的值为：pa=855ff8fc, pb=855ff8f8
交换前*pa和*pb的值为：*pa=2, *pb=3
交换前p_a和p_b的值为：p_a=855ff8f0, p_b=855ff8e8
交换前*p_a和*p_b的值为：*p_a=855ff8fc, *p_b=855ff8f8
交换前**p_a和**p_b的值为：**p_a=2, **p_b=3
-------------------------------------------------------
交换后a和b的值为：a=2, b=3
交换后pa和pb的值为：pa=855ff8f8, pb=855ff8fc
交换后*pa和*pb的值为：*pa=3, *pb=2
交换后p_a和p_b的值为：p_a=855ff8f0, p_b=855ff8e8
交换后*p_a和*p_b的值为：*p_a=855ff8f8, *p_b=855ff8fc
交换后**p_a和**p_b的值为：**p_a=3, **p_b=2
```

# 例题 9. [自定义函数之数字后移](https://www.dotcpp.com/oj/problem1046.html)

`题目描述：`有 n 个整数，使前面各数顺序向后移 m 个位置，最后 m 个数变成前面 m 个数。写一函数：实现以上功能，在主函数中输入 n 个数和输出调整后的 n 个数。

`解题思路:`

> 重点知识点：**malloc 函数和 free 函数**

1. 声明两个指针 `a` 和 `b`，应用 `int *a = (int *)malloc(sizeof(int) * n)` 和 `int *b = (int *)malloc(sizeof(int) * n)` 在堆上动态分配两块能够容纳 `n` 个整数的内存，并将该内存块的起始地址赋给指针 `a` 和 `b`。其中 `a` 存放输入的源数据地址，`b` 存放调整后的数据地址。
2. 构建一个函数 `moveNum`，传入 4 个参数，分别是 `int *a, int *b, int n, int m`，`n` 为数组长度，`m` 为移动位置的数值。
3. 在函数内部，遍历数组 `a`，根据题目要求，假定 `n=10,m=2` 时，通过观察发现，数组 `a` 和数组 `b` 的之间的关系如下图：

![例题9示例图](https://bu.dusays.com/2024/12/08/675520c56ef6b.png)

遍历完数组 `a` 后，调整后的数都保存在数组 `b` 中，在主函数输出 `b` 即可。

`注意事项:`

输出后，使用 `free(a)` 和 `free(b)` 来释放这两块内存，以防止内存泄漏。

`示例代码:`

```cpp
#include <stdio.h>
#include <stdlib.h>

// 函数用于将数组元素向右移动 m 位
void moveNum(int *a, int *b, int n, int m)
{
    for (int i = 0; i < n; i++)
    {
        if (i < m)
        {
            // 遍历数组a，当i小于移动的长度的数值时，将a[n - m + i]赋值给b[i],元素向右移动 m 位
            b[i] = a[n - m + i];
        }
        else
        {
            // 当i大于等于移动的长度的数值时，将a[i - m]赋值给b[i],元素向右移动 m 位
            b[i] = a[i - m];
        }
    }
}

int main()
{
    int n, m;
    // 用户输入数组大小 n
    scanf("%d", &n);
    // 动态分配内存并创建数组 a
    int *a = (int *)malloc(sizeof(int) * n);
    // 动态分配内存并创建数组 b
    int *b = (int *)malloc(sizeof(int) * n);
    // 用户输入 n 个整数，存储在数组 a 中
    for (int i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
    }
    // 用户输入右移的位数 m
    scanf("%d", &m);
    // 调用函数进行右移操作
    moveNum(a, b, n, m);
    // 打印右移后的数组 b 中的元素
    for (int i = 0; i < n; i++)
    {
        printf("%d ", b[i]);
    }
    printf("\n");
    // 释放动态分配的内存
    free(a);
    free(b);
    // 暂停程序以查看输出（特定于某些编译器/操作系统）
    system("pause");
    return 0;
}
```
`运行结果：`

```css
10
1 2 3 4 5 6 7 8 9 10
2
9 10 1 2 3 4 5 6 7 8
```

# 例题 10. [亲密数](https://www.dotcpp.com/oj/problem1122.html)

`题目描述：`两个不同的自然数 A 和 B，如果整数 A 的全部因子 (包括 1，不包括 A 本身) 之和等于 B；且整数 B 的全部因子 (包括 1，不包括 B 本身) 之和等于 A，则将整数 A 和 B 称为亲密数。求 3000 以内的全部亲密数。

`解题思路：`

- 创建 `SumOfFactors` 函数：计算一个数的因子和。通过遍历找到所有因子并求和。
- 在 `main` 函数中，创建数组 `array`，并通过循环计算 3000 以内每个数的因子和，存储在数组 `array` 中。
- 接着，再次循环遍历 `array` 数组，利用 `array[array[i]] == i` 条件寻找满足亲和数条件的组合。

`注意事项：`

- 计算因子和时，循环的范围可以优化为 `i <= num / 2`，减少不必要的遍历。
- 在数组访问时，要确保不越界，应确保数组下标在有效范围内。

`参考代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>

// 计算一个数的因子和
int SumOfFactors(int num)
{
    int sum = 1;  // 初始化因子和，1是任何数的因子
    for (int i = 2; i <= num / 2; i++)  // 从2开始遍历到num的一半，因为大于num的因子不可能了
    {
        if (num % i == 0)  // 如果i是num的因子
        {
            sum += i;  // 累加因子
        }
    }
    return sum;  // 返回因子和
}

int main()
{
    int array[3001];
    for (int i = 2; i <= 3000; i++)
    {
        array[i] = SumOfFactors(i);  // 计算每个数的因子和并存储在数组中
    }
    for (int i = 2; i <= 3000; i++)
    {
        if (array[i] <= 3000 && array[array[i]] == i && array[i] != i && array[i] > i)
        {
            printf("(%d,%d)", i, array[i]);  // 输出亲和数的组合
        }
    }
    printf("\n");
    system("pause");  // 暂停，等待用户输入后结束程序
    return 0;
}
```
`运行结果：`

```css
(220,284)(1184,1210)(2620,2924)
```

# 例题 11. [列出最简真分数序列](https://www.dotcpp.com/oj/problem1123.html)

`题目描述：`按递增顺序依次列出所有分母为 40，分子小于 40 的最简分数。

`解题思路：`

理解题目含义，可知所有偶数和 5 的倍数都需要进行化简，因此只需要通过循环输出除 1 之外的奇数但不是 5 的倍数的数与 40 的比值即可。以下是解题的思路：

1. 使用 `for` 循环，从 1 开始，每次递增 2，直到 `i` 大于 40，这样可以确保 `i` 的取值都是奇数。
2. 在循环中使用条件语句进行判断：
   - 如果 `i` 等于 1，输出 `i` 和 40 的比值，并以逗号结尾。
   - 如果 `i` 不等于 1 且 `i` 除以 5 的余数不为 0，输出 `i` 和 40 的比值，并以逗号结尾。

`注意事项：`

在阅读和理解代码的过程中，需要注意以下几点：

1. 循环条件：理解 `for` 循环的控制条件，确保循环在正确的范围内运行。
2. 条件语句：理解条件语句中的逻辑，特别是对奇数和不是 5 的倍数的判断。

`参考代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>

int main()
{
    // 循环，i从1开始，每次递增2，直到i大于40
    for (int i = 1; i <= 40; i = i + 2)
    {
        // 判断i是否等于1
        if (i == 1)
        {
            // 如果i等于1，打印输出i和40的比值，并以逗号结尾
            printf("%d/%d,", i, 40);
        }
        // 如果i不等于1且i除以5的余数不为0
        else if (i % 5 != 0)
        {
            // 打印输出i和40的比值，并以逗号结尾
            printf("%d/%d,", i, 40);
        }
    }
    // 打印输出换行符
    printf("\n");

    // 暂停控制台以查看输出结果
    system("pause");

    // 返回0，表示程序正常结束
    return 0;
}
```
`运行结果：`

```css
1/40,3/40,7/40,9/40,11/40,13/40,17/40,19/40,21/40,23/40,27/40,29/40,31/40,33/40,37/40,39/40,
```

# 例题 12. [自定义函数之字符串连接](https://www.dotcpp.com/oj/problem1032.html)

`题目描述：`写一函数，将两个字符串连接。

`解题思路：`

> 首先，我想说既然题目中给出自己写一个函数将两个字符串连接，那么出题人的意图应该就是要考生自己编写一个函数实现与 `strcat` 函数同样的效果，而不是直接引用 `strcat` 函数，如果直接引用 `strcat` 函数，那么本题目将毫无测试价值。

我的思路大致如下：

- 输入两个字符串 `str_1` 和 `str_2`，且要求声明字符数组长度时 `str_1` 的长度大于和等于输入字符串后的 `str_1+str_2` 的长度。

- 构建函数传入两个字符串 `void montage_string(char *string_1, char *string_2)`，在函数内部实现如下效果：

```cpp
      int i = strlen(string_1), k = 0, j = strlen(string_1) + strlen(string_2);
  //i等于字符串1的长度，j等于字符串1和字符串2长度的和，从字符串1的最后一个字符string_1[i]（即'\0'）开始，将字符串2的第一个字符string_2[k]赋值给字符串1的最后一个字符string_1[i]（即'\0'），实现拼接效果
      while (i <= j)
      {
          string_1[i++] = string_2[k++];
  //在赋值运算结束以后，i和k自增，当i增加到大于j(即符串1和字符串2长度的和)时，跳出循环
      }
```

`运行结果：`

```css
123
abc
123abc
```

# 例题 13. [Minesweeper–扫雷](https://www.dotcpp.com/oj/problem1096.html)

`题目描述：`扫雷员你玩过扫雷吗？这个可爱的小游戏附带了一个我们记不起名字的操作系统。游戏的目标是找到所有地雷在一个 `MxN` 场地内的位置。游戏显示了一个正方形中的数字，它告诉你这个正方形附近有多少地雷。每个正方形最多有八个相邻的正方形。左侧的 `4x4` 字段包含两个地雷，每个地雷由一个 `*` 字符表示。如果我们用上面描述的提示数字来表示同一个字段，我们最终得到下边的字段：

```css
*...
....
.*..
....
*100
2210
1*10
1110
```
`解题思路：`

- 使用二维字符数组存储地雷信息，`*` 表示有地雷，`.` 表示安全。
- 确定了行列数后，声明两个二位数组（分别用于存储地雷信息和地雷数量），且在原来的行列数上增加两行两列，确保计算地雷周围数量时不越界。
- 读取二位字符数组，将地雷信息转换为二维数组，其中地雷表示为 `1`，安全表示为 `0`，然后计算每个格子周围的地雷数量。
- 输出每个格子的地雷数量，如果格子本身是地雷就输出 `*` 表示有地雷。

`注意事项：`

- 在计算地雷周围数量时，注意处理边缘情况，确保不越界。
- 使用 `memset` 初始化数组，避免脏数据影响计算。
- 在输入中添加判断条件，以确保满足输入条件。

`参考代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    int M, N, count = 0; // M和N分别表示行数和列数，count用于记录场地数量

    // 循环读取输入，直到输入不符合条件
    while (scanf("%d%d", &M, &N) == 2 && M && N)
    {
        count++; // 场地数量加一
    
        char input[M][N + 1];            // 二维字符数组用于存储输入的地雷信息
        memset(input, 0, sizeof(input)); // 将字符数组初始化为0
    
        // 循环读取每一行地雷信息
        for (int i = 0; i < M; i++)
        {
            scanf("%s", input[i]);
        }
    
        // 增加两行两列，确保了在计算地雷周围的地雷数量时，不会出现数组越界的情况，且能够正确处理地雷场地的边缘情况
        int row = M + 2, col = N + 2;
        int array[row][col], result[row][col]; // 二维数组存储地雷信息和地雷数量
        memset(array, 0, sizeof(array));       // 将数组初始化为0
        memset(result, 0, sizeof(result));     // 将数组初始化为0
    
        // 将地雷信息转换为二维数组，1表示有地雷
        for (int i = 0; i < M; i++)
        {
            for (int j = 0; j < N; j++)
            {
                if (input[i][j] == '*')
                {
                    array[i + 1][j + 1] = 1;
                }
            }
        }
    
        // 遍历数组，如果数组元素值为0（取反为真），则计算每个格子周围的地雷数量，即格子周围（左上方=array[i - 1][j - 1]，正上方=array[i - 1][j]，右上方=array[i - 1][j + 1]，左方=array[i][j - 1]，右方=array[i][j + 1]，左下方=array[i + 1][j - 1]，正下方=array[i + 1][j]，右下方=array[i + 1][j + 1]）和格子本身=array[i][j]（值为0）地雷的数量
        for (int i = 1; i <= M; i++)
        {
            for (int j = 1; j <= N; j++)
            {
                if (!array[i][j])
                {
                    for (int p = i - 1; p <= i + 1; p++)
                    {
                        for (int q = j - 1; q <= j + 1; q++)
                        {
                            result[i][j] += array[p][q];
                        }
                    }
                }
            }
        }
        printf("Field #%d:\n", count);
        // 输出每个格子的地雷数量或者 '*' 表示有地雷
        for (int i = 1; i <= M; i++)
        {
            for (int j = 1; j <= N; j++)
            {
                if (array[i][j])
                {
                    printf("*");
                }
                else
                {
                    printf("%d", result[i][j]);
                }
            }
            printf("\n");
        }
        printf("\n");
    }
    system("pause");
    return 0;
}
```
`运行结果：`

```css
4 4
*...
....
.*..
....
Field #1:
*100
2210
1*10
1110

3 5
**...
.....
.*...
Field #2:
**100
33200
1*100

0 0
请按任意键继续. . .
```

# 例题 14. [DNA](https://www.dotcpp.com/oj/problem1115.html)

`题目描述：`小强从小就喜欢生命科学，他总是好奇花草鸟兽从哪里来的。终于，小强上中学了，接触到了神圣的名词–`DNA`. 它有一个双螺旋的结构。这让一根筋的小强抓破头皮，“要是能画出来就好了” 小强喊道。现在就请你帮助他吧。

解题思路：

- 使用嵌套循环遍历每个测试用例。
- 在每个测试用例中，使用两层循环分别控制图案的行和列。
- 判断当前位置是否应该打印 `X` 或空格，根据条件确定输出。

`示例图解：`

通过阅读题目可知，`DNA` 中 `X` 刚好出现在对角线上，如果用 `a` 表示一个单位的 `DNA` 串的行数，用 `i` 表示行，用 `j` 表示列，则是在 `j==i` 或者 `j == a - i - 1` 时需要绘制 `X`。假如生成 3 行重复 1 次，具体如下图所示：

![](https://bu.dusays.com/2024/12/08/675520d16908d.png)

`注意事项：`

- 确保输入的测试用例数量在有效范围内。
- 注意循环的边界条件，防止数组越界。
- 在进行 1 次以上重复绘制的时候，从第二次开始数组索引是从 `array[1][1]` 开始遍历的。
- 在每个测试用例之间用换行符分隔。

`参考代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>

int main()
{
    // N: 测试用例数量
    int N;
    while (scanf("%d", &N) == 1 && N > 0 && N <= 15)
    {
        // 遍历每个测试用例
        while (N-- > 0)
        {
            int a, b;
            // a: 图案宽度, b: 图案高度
            scanf("%d%d", &a, &b);
            int flag = 0;

            // 循环打印图案的每一行
            while (b-- > 0)
            {
                for (int i = flag; i < a; i++)
                {
                    // 循环打印图案的每一列
                    for (int j = 0; j < a; j++)
                    {
                        // 如果 j 等于 i 或者等于 a - i - 1，则打印 "X"，否则打印空格
                        if (j == i || j == a - i - 1)
                        {
                            printf("X");
                        }
                        else
                        {
                            printf(" ");
                        }
                    }
                    printf("\n");
                }
                flag = 1;
            }
            // 在每个测试用例之间用换行符分隔
            printf("\n");
        }
    }
    system("pause");
    return 0;
}
```
`运行结果：`

```css
2
3 1
X X
 X
X X

5 4
X   X
 X X
  X
 X X
X   X
 X X
  X
 X X
X   X
 X X
  X
 X X
X   X
 X X
  X
 X X
X   X
```

# 例题 15. [自守数问题](https://www.dotcpp.com/oj/problem1144.html)

`题目描述：`自守数是指一个数的平方的尾数等于该数自身的自然数。例如：25^2=625， 76^2=5776， 9376^2=87909376。请求出 200000 以内的自守数？

`解题思路：`

从 0 开始遍历到 200000，然后设计一个判断自守数函数 `IsAutomorphicNumber` ，将遍历的每个数字 `num` 作为参数输入，在函数内部进行判断，将判断结果返回一个整型值，1 表示是自守数，0 表示不是自守数。函数内部具体实现：

- **初始化标志 `flag` 为 1：** 表示一开始默认是自守数。
- **计算传入数字的平方：** 将输入数字 `num` 的平方存储在 `square` 中。
- 循环判断每一位数字：
  - 通过 `while (num)` 循环，用 `t_num = num % 10` 取数字的末位，用 `t_square = square % 10` 取平方的末位
  - 如果 `t_num` 不等于 `t_square`，将标志 `flag` 设为 `0`，表示不是自守数，然后跳出循环。
  - 如果未跳出循环，继续将 `num` 和 `square` 向右移动一位（通过 `num /= 10` 和 `square /= 10` 实现），以处理下一位数字。
  - 如果循环一直进行，直到传入的数字 `num` 为 `0` 时，循环停止，此时 `flag` 仍然为 `1`，则表示 `num` 为自守数。
- **返回标志 `flag`：** 标志最终表示是否为自守数。

注意事项：

- **数据类型：** 使用 `long long int` 数据类型，因为计算数字平方可能会导致溢出，尤其是对于较大的数字。
- **循环范围：** 注意循环的范围，确保在合适的范围内检查自守数。

`参考代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>

// 函数：判断是否为自守数
int IsAutomorphicNumber(long long int num)
{
    int flag = 1;                     // 初始化标志为1，表示是自守数
    long long int square = num * num; // 计算数字的平方

    // 循环判断每一位数字
    while (num)
    {
        long long int t_num = num % 10;       // 取数字的个位
        long long int t_square = square % 10; // 取平方的个位
    
        // 判断个位数字是否相同
        if (t_num != t_square)
        {
            flag = 0; // 如果个位数字不同，将标志设置为0，表示不是自守数
            break;    // 跳出循环
        }
        
        // 缩小范围，处理下一位数字
        num /= 10;
        square /= 10;
    }
    return flag; // 返回判断结果
}

int main()
{
    // 循环检查0到200000之间的数字
    for (long long int i = 0; i <= 200000; i++)
    {
        // 调用自守数判断函数，如果是自守数则输出
        if (IsAutomorphicNumber(i))
        {
            printf("%lld  ", i); // 输出自守数
        }
    }
    system("pause");
    return 0;
}
```
`运行结果：`

```css
0  1  5  6  25  76  376  625  9376  90625  109376  请按任意键继续. . .
```

# 例题 16. [求矩阵的两对角线上的元素之和](https://www.dotcpp.com/oj/problem1138.html)

`题目描述：`求矩阵的两对角线上的元素之和

`解题思路：`

1. 首先需要理解对角线的定义：

- 主对角线（mainDiagonal）是从矩阵的左上角到右下角的一条线。

- 副对角线（antiDiagonal）是从矩阵的右上角到左下角的一条线。

2. 其次就是求对角线之和

- 主对角线之和（mainDiagonalSum），就是二维数组索引值相等的对应元素之和。

- 副对角线之和（antiDiagonalSum）相对复杂，二维数组列索引`j` 和`i`、`N`之间存在如下关系：

> j = N - 1 - i

3. 最后求矩阵的对角线和。

注意事项：

- 如果矩阵的维度是 **奇数**，那么 **主对角线** 和 **副对角线** 会在中心元素上重叠一次。在这种情况下，代码中计算对角线和时，中心元素会被重复加一次。
- 在计算完副对角线的和之后，如果是**奇数**矩阵，我们从 **主对角线** 或 **副对角线** 两者之一的和中减去中心元素，防止重复计算。

`参考代码：`

```cpp
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int N;
    printf("请输入矩阵的大小 N: ");
    scanf("%d", &N);

    int matrix[N][N];

    printf("请输入矩阵的元素:\n");
    // 输入矩阵元素
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            scanf("%d", &matrix[i][j]);
        }
    }

    int mainDiagonalSum = 0; // 主对角线和
    int antiDiagonalSum = 0; // 副对角线和

    // 计算主对角线和和副对角线和
    for (int i = 0; i < N; i++)
    {
        mainDiagonalSum += matrix[i][i];         // 主对角线元素
        antiDiagonalSum += matrix[i][N - 1 - i]; // 副对角线元素
    }

    // 如果矩阵的维度是奇数，中心元素重复计算了，减去它
    if (N % 2 == 1)
    {
        int centerIndex = N / 2;
        antiDiagonalSum -= matrix[centerIndex][centerIndex]; // 减去中心元素
    }

    printf("主对角线的和是: %d\n", mainDiagonalSum);
    printf("副对角线的和是: %d\n", antiDiagonalSum);
    printf("对角线的总和是: %d\n", mainDiagonalSum + antiDiagonalSum);
    system("pause");
    return 0;
}
```

`运行结果：`

```perl
请输入矩阵的大小 N: 3
请输入矩阵的元素:
1 2 3
4 5 6
7 8 9
主对角线的和是: 15
副对角线的和是: 10
对角线的总和是: 25
```

