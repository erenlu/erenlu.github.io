---
title: CS50 | Week 1 C
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/CS50.png
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/Code.jpg
tags:
  - 学习笔记
  - CS50
  - C语言
abbrlink: 1cf2
date: 2020-07-09 22:53:24
excerpt: CS50 第一周学习笔记。
---

> 提示：在此只是将零散的、个人化的知识点记录下来，完整笔记查看官方 [Notes](https://cs50.harvard.edu/x/2020/weeks/1/) 即可。 

# 零散知识点

- 语法糖

  e.g. `count ++`

- 单个等号是从右向左赋值
- 布尔值：true or false

- 三种循环

  while loop

```
int i = 0;
while (i < 50) //布尔表达式
{
    printf("hello, world\n");
    i++;
}
```

​		for loop: `for(初始值; 布尔表达式; 执行)`

```
int main(void)
{
    for (int i = 1; ; i *= 2)  //无限循环 or true，因为没有布尔表达式
    {
        printf("%i\n", i);
        sleep(1);
    }
}
```

​        do-while loop (至少执行一次)

```
#include <cs50.h>
#include <stdio.h>

int get_positive_int(void);

int main(void)
{
    int i = get_positive_int();
    printf("%i\n", i);
}

// Prompt user for positive integer
int get_positive_int(void)
{
    int n;
    do
    {
        n = get_int("%s", "Positive Integer: ");
    }
    while (n < 1);  // 布尔表达式
    return n;
}
```

-  `#include <CS50.h>` 表示进入库，库只是一个代码代码文件。别人写的，我们来用，

  `#include <stdio.h>` 访问标准 I/O 库，包含 printf 等函数。

- 在可以 copy code 时应该抵制这种诱惑，这会让代码复杂冗长。

- 主要的 `  main` 程序部分应该在最前面，读代码更方便。所以定义的其他函数可以放在底部。但是 C 执行时是从上往下依次执行，会遇到不认识的新函数。此时则把 ` void xxx(void);` 复制到前面去， 去欺骗 C，让它以为曾经见过，只是不完全知道这个函数。（这个解释太棒了！）

- `void xxx(void)`   第一个位置表示输出类型（type of output），第二个位置表示输入类型（type of input）

  e.g. 第一个 void 表示不返回任何输出，第二个 void 表示不接受任何输入。    

  ```
  void cough(int n)  //cough函数接受输入n
  	for(i=0;i<n;i++)
  	{
  		printf("cough\n");
  		i++
  	}
  	
  在主函数中就可以为：
  int main(void)
  {
  	cough(3);
  }
  ```

# 类型、格式、运算符

There are other types we can use for our variables

- `bool`, a Boolean expression of either `true` or `false`
- `char`, a single character like `a` or `2`
- `double`, a floating-point value with even more digits
- `float`, a floating-point value, or real number with a decimal value
- `int`, integers up to a certain size, or number of bits
- `long`, integers with more bits, so they can count higher
- `string`, a string of characters   

And the CS50 library has corresponding functions to get input of various types:

- `get_char`
- `get_double`
- `get_float`
- `get_int`
- `get_long`
- `get_string`

for printf, too, there are different placeholders for each type:

- `%c` for chars
- `%f` for floats, doubles
- `%i` for ints
- `%li` for longs
- `%s` for strings

And there are some mathematical operators we can use:

- `+` for addition
- `-` for subtraction
- `*` for multiplication
- `/` for division
- `%` for remainder

# 浮点的不精确 & 整数的溢出

我们的电脑有内存，在硬件芯片称为随机存取存储器。我们的程序在运行时使用 RAM 来存储数据，但是内存是有限的。因此，对于有限的比特数，我们不能代表所有可能的数（其中有无限个数）。所以我们的计算机对每个浮点和整型都有一定数量的位，并且必须在某一点四舍五入到最接近的十进制值。

RAM 硬件自身限制，使用不当时会造成浮点的不精确 （floating-point imprecision）或整数溢出（inerger overflow）。

- e.g. floating-point imprecision

```
{
    // Prompt user for x
    float x = get_float("x: ");

    // Prompt user for y
    float y = get_float("y: ");

    // Perform division
    printf("x / y = %.50f\n", x / y);
}
```

- With `%50f`, we can specify the number of decimal places displayed.

- Hmm, now we get …

  ```
  x: 1
  y: 10
  x / y = 0.10000000149011611938476562500000000000000000000000
  ```

- It turns out that this is called **floating-point imprecision**, where we don’t have enough bits to store all possible values, so the computer has to store the closest value it can to 1 divided by 10.

