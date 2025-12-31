---
title: LeetCode刷题笔记🐦
date: 2023-02-12 20:02:56
updated: 2024-12-08 18:00:00
tag: [算法,LeetCode]
categories: [算法,LeetCode]
cover: 
description: LeetCode刷题笔记🐦
sticky: 6
swiper_index: 6

---

---



> <img alt="Static Badge" src="https://img.shields.io/badge/题目来源-LeetCode-red?style=flat-square&logo=leetcode&logoColor=">
>
> 题目选自[力扣（LeetCode）](https://leetcode.cn/)著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
>
> ![](https://img.shields.io/badge/解题方法-Feis Studio-green?logo=YouTube&logoColor=red)
>
> 解题方案参考YouTube 频道🎬[Feis Studio](https://www.youtube.com/@KenYiLee)，欢迎订阅老师的频道进行学习。

`Tips:徽章HTML和Markdown语法` 

```html
<img alt="Static Badge" src="https://img.shields.io/badge/题目来源-LeetCode-red?style=flat-square&logo=leetcode&logoColor=">
![Static Badge](https://img.shields.io/badge/解题方法-Feis Studio-green?logo=YouTube&logoColor=red)
```

在线制作网站：https://shields.io/badges/static-badge

# 例题 1. [只出现一次的数字](https://leetcode.cn/problems/single-number/)（LeetCode 第 136 题–singleNumber）

`题目内容：`给你一个非空整数数组 nums ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

`解题示例：`

> 输入：nums = [4,1,2,1,2]
> 输出：4

`示例代码：`

> 1. 暴力解法：时间复杂度为 O (n2)

```CPP
#include <stdio.h>
#include <stdlib.h>
int singleNumber(int *nums, int numsSize)
{
    int i, j;
    for (i = 0; i < numsSize; i++)
    {
        int count = 0;
        for (j = 0; j < numsSize; j++)
        {
            if (nums[i] == nums[j])
            {
                count++;
            }
        }
        if (count == 1)
        {
            return nums[i];
        }
    }
    return -1;
}
int main()
{
    int singleNumber(int *nums, int numsSize);
    int nums[5] = {4, 1, 2, 1, 2};
    printf("singleNumber is %d\n", singleNumber(nums, 5));
    system("pause");
    return 0;
}
```

> 2. 巧妙解法：应用位运算符 “异或”–时间复杂度为 O (n)

```CPP
int singleNumber(int *nums, int numsSize) // 将第一个数赋予n，应用位运算符"^"(异或),从第2个数开始与n进行异或运算，输出运算结果
{
    int n = nums[0];
    for (int i = 1; i < numsSize; i++)
    {
        n = n ^ nums[i]; // 计算过程类似于：n=4^1^2^1^2 => n=1^1^2^2^4=0^0^4=4
    }
    return n;
}
```

# 例题 2. [快乐数](https://leetcode.cn/problems/happy-number/)（LeetCode 第 202 题–happyNumber）

`题目内容：`对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。然后重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。
如果这个过程结果为 1，那么这个数就是快乐数。如果 n 是快乐数就返回 `true`；不是，则返回 `false`。

```
解题示例：
```

> 输入：n = 19
> 输出：true
> 解释：
> 12 + 92 = 82
> 82 + 22 = 68
> 62 + 82 = 100
> 12 + 02 + 02 = 1

`示例代码`：

> 1. 暴力解法：存在溢出风险

```CPP
#include <stdio.h>
#include <stdlib.h>

// 计算平方和
int next_n(int n)
{
    int sum = 0;
    while (n != 0)
    {
        int d = n % 10;
        n /= 10;
        sum += d * d;
    }
    return sum;
}

// 遍历存储在history中的数与n进行一一比较，判断是否相等，相等则返回true则停止循环，不相等则返回false继续循环求下一个n
bool contains(int *history, int size, int n)
{
    for (int i = 0; i < size; i++)
    {
        if (history[i] == n)
        {
            return true;
        }
    }
    return false;
}

// 判断是否为快乐数（暴力法）
bool isHappy(int n) // 利用储存历史数方法
{
    int history[10000]; // 数组长度的精度很难计算
    int size = 0;       // 为数组索引
    while (!contains(history, size, n))
    {
        history[size] = n; // 将n和之后的next_n储存在history中
        size++;
        n = next_n(n);
    }
    return n == 1;
}

int main()
{
    int next_n(int n);
    bool isHappy(int n);
    int n;
    scanf("%d", &n);
    if (isHappy(n))
    {
        printf("%d是一个快乐数\n", n);
    }
    else
    {
        printf("%d不是一个快乐数\n", n);
    }
    system("pause");
    return 0;
}
```

> 2. 巧妙解法：解题思路源自➡️[弗洛伊德的乌龟 - 兔子循环寻找](https://visualgo.net/en/cyclefinding?slide=1)

```CPP
#include <stdio.h>
#include <stdlib.h>

// 计算平方和
int next_n(int n)
{
    int sum = 0;
    while (n != 0)
    {
        int d = n % 10;
        n /= 10;
        sum += d * d;
    }
    return sum;
}

// 判断是否为快乐数（巧妙法）
bool isHappy(int n) // 利用龟兔赛跑方法(双指针)
{
    int slow = n; // 代表乌龟
    int fast = n; // 代表兔子
    do
    {
        slow = next_n(slow);         // 乌龟一次走一步
        fast = next_n(next_n(fast)); // 兔子一次走两步
    } while (slow != fast);          // 乌龟和兔子一定会在某一圈相遇，届时循环停止
    return fast == 1;
}

int main()
{
    int next_n(int n);
    bool isHappy(int n);
    int n;
    scanf("%d", &n);
    if (isHappy(n))
    {
        printf("%d是一个快乐数\n", n);
    }
    else
    {
        printf("%d不是一个快乐数\n", n);
    }
    system("pause");
    return 0;
}
```

**弗洛伊德的乌龟 - 兔子循环寻找**图例如下：其中，橙色代表兔子，绿色代表乌龟

![弗洛伊德的乌龟-兔子循环寻找示例图](https://bu.dusays.com/2024/12/08/675581dba0292.gif)





# 例题 3. [移动零](https://leetcode.cn/problems/move-zeroes/)（LeetCode 第 283 题–moveZeroes）

`题目内容：`给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

`解题示例：`

> 输入: nums = [0,1,0,3,12]
> 输出: [1,3,12,0,0]

`示例代码`：

> 1. 暴力解法：存在超时风险

```CPP
#include <stdio.h>
#include <stdlib.h>

void moveZeroes(int *nums, int numsSize)
{
    while (true)
    {
        int count = 0;
        for (int i = 0; i + 1 < numsSize; i++)
        {
            if (nums[i] == 0 && nums[i + 1] != 0) // 遍历数组，如果相邻两个数前一个数为0且后一个数不为0，就仅进行位置交换，类似冒泡排序
            {
                count++;
                nums[i] = nums[i + 1];
                nums[i + 1] = 0;
                break;
            }
        }
        if (count == 0) // 当遍历完数组，没有发生1次交换代表排序完成，停止循环
        {
            break;
        }
    }
}

int main()
{
    void moveZeroes(int *nums, int numsSize);
    int nums[6] = {0, 3, 2, 0, 5, 7};
    moveZeroes(nums, 6);
    for (int i = 0; i < 5; i++)
    {
        printf("%-3d", nums[i]);
    }
    printf("\n");
    system("pause");
    return 0;
}
```

> 2. 巧妙解法：时间复杂度为 O (n)

```CPP
#include <stdio.h>
#include <stdlib.h>

void moveZeroes(int *nums, int numsSize)
{
    int j = 0;
    for (int i = 0; i < numsSize; i++)
    {
        if (nums[i] != 0) // 遍历数组，如果nums[i]不为0，就依次将这些数从nums[0]、nums[1]....进行赋值
        {
            nums[j] = nums[i];
            j++;
        }
    }
    while (j != numsSize) // 对数组剩余的空位进行补0
    {
        nums[j] = 0;
        j++;
    }
}

int main()
{
    void moveZeroes(int *nums, int numsSize);
    int nums[6] = {0, 3, 2, 0, 5, 7};
    moveZeroes(nums, 6);
    for (int i = 0; i < 5; i++)
    {
        printf("%-3d", nums[i]);
    }
    printf("\n");
    system("pause");
    return 0;
}
```

# 例题 4. [最大子数组和](https://leetcode.cn/problems/maximum-subarray/)（LeetCode 第 53 题–Maximum Subarray）

`题目内容：`给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。**子数组** 是数组中的一个连续部分。

`解题示例：`

> 输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出：6
> 解释：连续子数组 [4,-1,2,1] 的和最大，为 6。

`示例代码`：

> 1. 暴力解法：超时，时间复杂度为 O (n2)

```CPP
#include <stdio.h>
#include <stdlib.h>

int maxSubArray(int *nums, int numsSize)
{
    int max = nums[0];
    for (int i = 0; i < numsSize; i++)
    {
        int sum = 0;
        for (int j = i; j < numsSize; j++)
        {
            sum += nums[j];
            if (sum > max)
            {
                max = sum;
            }
        }
    }
    return max;
}

int main()
{
    int maxSubArray(int *nums, int numsSize);
    int nums[9] = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    printf("%d\n", maxSubArray(nums, 9));
    system("pause");
    return 0;
}
```

> 2. 巧妙解法：时间复杂度为 O (n)，应用正数增益效应

```CPP
#include <stdio.h>
#include <stdlib.h>

int maxSubArray(int *nums, int numsSize)
{
    int num[numsSize];
    for (int i = 0; i < numsSize; i++)
    {
        num[i] = nums[i];
    }
    for (int i = 0; i < numsSize - 1; i++)
    {
        /* 利用正数增益效应：因为是连续子序列，所以只有相邻两数如相加结果为正将结果赋给后一个
        才有可能与下一个数相加实现累加增益，如结果为负则果断舍弃，重新寻找正数开始相加 */
        if (num[i] > 0)
        {
            num[i + 1] = num[i] + num[i + 1];
        }
    }
    int max = num[0];
    for (int i = 1; i < numsSize; i++)
    {
        if (max < num[i])
        {
            max = num[i];
        }
    }
    return max;
}

int main()
{
    int maxSubArray(int *nums, int numsSize);
    int nums[9] = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    printf("%d\n", maxSubArray(nums, 9));
    system("pause");
    return 0;
}
```

# 例题 5. [数元素](https://leetcode.cn/problems/counting-elements/)（LeetCode 第 1426 题–Counting-Elements） **VIP 题目**

`题目内容：`给你一个整数数组 `nums` ，请你找出数组内的元素加 1 后是否还出现在数组内，统计符合这个条件的元素个数。本题有个条件就是：`0<=nums[i]<=1000, 1<=numsSize<=1000`，此条件对优化算法有很大作用。

`解题示例：`

> 输入：nums = [1,1,2,3]
> 输出：3
> 解释：比 1 大 1 的数为 2 依旧在数组内，1 有 2 个；比 2 大 1 的数为 3 依旧在数组内，2 有 1 个。总计结果为 3。

`示例代码`：

> 1. 暴力解法：时间复杂度为 O (n2)

```CPP
#include <stdio.h>
#include <stdlib.h>

int countElements(int *nums, int numsSize) 
{
    int i, j;
    int count = 0;
    for (i = 0; i < numsSize; i++)
    {
        for (j = 0; j < numsSize; j++)
        {
            if (nums[i] + 1 == nums[j])
            {
                count++; // 只要比nums[i]大1的数出现在数组内，计数加1，停止循环
                break;
            }
        }
    }
    return count;
}

int main()
{
    int nums[4] = {1, 1, 2, 3};
    printf("符合条件的元素有%d个\n", countElements(nums, 4));
    system("pause");
    return 0;
}
```

> 2. 巧妙解法：合理利用题目给定的条件，降低时间复杂度为 O (n)

```CPP
#include <stdio.h>
#include <stdlib.h>

int countElements(int *nums, int numsSize) // 合理利用题目给定的条件，即0<=nums[i]<=1000, 1<=numsSize<=1000
{
    int ArrayX[1002] = {0};
    for (int i = 0; i < numsSize; i++)
    {
        ArrayX[nums[i]]++; // 统计ArrayX[nums[i]]出现的次数,假如nums[i]==1，意味着ArrayX[1]就统计1次;
    }
    int count = 0;
    printf("x   : ArrayX[x]\n");
    for (int x = 0; x <= 1000; x++)
    {
        if (ArrayX[x + 1] > 0) // 遍历0~1000，比如x=1,且ArrayX[2]>0时，意味着比1大的数也就是2存在数组内
                               // 经过上面步骤可以统计出ArrayX[1]出现的次数，那么1出现的次数就是元素的个数
        {
            count += ArrayX[x];
        }
        printf("%-4d: %-5d\n", x, ArrayX[x]);
    }
    return count;
}

int main()
{
    int nums[4] = {1, 1, 2, 3};
    printf("符合条件的元素有%d个\n", countElements(nums, 4));
    system("pause");
    return 0;
}
```

`Tips:` 巧妙解法根据如下输出结果理解会更直观：

```css
x : ArrayX[x]
0 : 0
1 : 2
2 : 1
3 : 1
4 : 0
......后面输出的ArrayX[x]全为0，此处省略
符合条件的元素有3个
```

# 例题 6. [比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)（LeetCode 第 844 题–Backspace String Compare）

`题目内容：`给定 `s` 和 `t `两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true`。`# `代表退格字符。注意：如果对空文本输入退格字符，文本继续为空。

> 输入：s = “ab#c”, t = “ad#c”
> 输出：true
> 解释：s 和 t 都会变成 “ac”。

`提示：`

> - `1 <= s.length, t.length <= 200`
> - `s` 和 `t` 只含有小写字母以及字符`#`

`示例代码`：

> 1. 暴力解法：时间复杂度为 O (n)

```CPP
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void backspaceCompare(char *s, char *t)
{
    int len_s = strlen(s);
    char result_s[len_s + 1];
    { // 加这个大括号是防止下面提示j重复声明
        int j = 0;
        for (int i = 0; i < len_s; i++)
        {
            if (s[i] != '#')
            {
                result_s[j] = s[i]; // 遍历字符串，如果遇到的不是'#'，则将s[i]依次保存在新建的result_s字符串中，同时索引j向后挪一位
                j++;
            }
            else
            {
                if (j > 0)
                {
                    j--; // 如果遇到'#'，索引j向前挪一位，但必须在j>0的前提下，因为当j=0时,如果向前挪一位会造成超出原字符串范围
                }
            }
        }
        result_s[j] = '\0'; // result_s的长度小于等于s的长度，当小于s时，须在result_s末位补'\0'，表示字符串结束
    }
    int len_t = strlen(t);
    char result_t[len_t + 1];
    {
        int j = 0;
        for (int i = 0; i < len_t; i++)
        {
            if (t[i] != '#')
            {
                result_t[j] = t[i];
                j++;
            }
            else
            {
                if (j > 0)
                {
                    j--;
                }
            }
        }
        result_t[j] = '\0';
    }
    if (strcmp(result_s, result_t) == 0) // 应用strcmp函数比较处理后的两个字符串，如果比较结果为0表示两字符串完全相同，则输出为true
    {
        printf("true\n");
    }
    else
    {
        printf("false\n");
    }
}

int main()
{
    void backspaceCompare(char *s, char *t);
    char s[5] = "ab#c", t[5] = "ad#c";
    backspaceCompare(s, t);
    system("pause");
    return 0;
}
```

> 2. 自建函数法（无返回值）：时间复杂度为 O (n)

```CPP
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void process(char *result, const char *str)
{
    int len = strlen(str);
    int j = 0;
    for (int i = 0; i < len; i++)
    {
        if (str[i] != '#')
        {
            result[j] = str[i];
            j++;
        }
        else
        {
            if (j > 0)
            {
                j--;
            }
        }
    }
    result[j] = '\0';
}

int main()
{
    void process(char *result, const char *str);
    char s[5] = "ab#c", t[5] = "ad#c";
    int len_s = strlen(s), len_t = strlen(t);
    char result_s[len_s + 1], result_t[len_t + 1];
    process(result_s, s);
    process(result_t, t);
    if (strcmp(result_s, result_t) == 0)
    {
        printf("true\n");
    }
    else
    {
        printf("false\n");
    }
    system("pause");
    return 0;
}
```

> 3. 自建函数法（有返回值）：时间复杂度为 O (n)

```CPP
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char *process(const char *str)
{
    int len = strlen(str);
    char *result = (char *)malloc(sizeof(char) * (len + 1));
    int j = 0;
    for (int i = 0; i < len; i++)
    {
        if (str[i] != '#')
        {
            result[j] = str[i];
            j++;
        }
        else
        {
            if (j > 0)
            {
                j--;
            }
        }
    }
    result[j] = '\0';
    return result;
}

int main()
{
    char *process(const char *str);
    char s[5] = "ab#c", t[5] = "ad#c";
    char *result_s = process(s);
    char *result_t = process(t);
    if (strcmp(result_s, result_t) == 0)
    {
        printf("true\n");
    }
    else
    {
        printf("false\n");
    }
    free(result_s);
    free(result_t);
    system("pause");
    return 0;
}
```

> 4. 自建函数法（无返回值）：时间复杂度为 O (n)，空间复杂度为 O (1)

```CPP
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void process(char *str)
{
    int len = strlen(str);
    int j = 0;
    for (int i = 0; i < len; i++)
    {
        if (str[i] != '#')
        {
            str[j] = str[i];  // 读取字符串永远先于写入字符串
            j++;
        }
        else
        {
            if (j > 0)
            {
                j--;
            }
        }
    }
    str[j] = '\0';
}

int main()
{
    void process(char *str);
    char s[5] = "ab#c", t[5] = "ad#";
    process(s);
    process(t);
    if (strcmp(s, t) == 0)
    {
        printf("true\n");
    }
    else
    {
        printf("false\n");
    }
    system("pause");
    return 0;
}
```
