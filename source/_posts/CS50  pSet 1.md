---
title: CS50 | pset 1
index_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/cs50pset.png
banner_img: https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/CS50pSet_homework.jpg
tags:
  - CS50
  - 学习笔记
  - C语言 
abbrlink: bfe6
date: 2020-07-10 00:23:29
excerpt: CS50 Problem Set 1 (Fall 2020). Mario More, Cash, Credit Solutions
---

{% note success %}

本文仅记录笔者在 CS50 Problem Set 1 (Fall 2020) 中的解题思路和过程。如有错误或疏漏欢迎留言指正！

{% endnote %}

# CS50 IDE

CS50 的课后作业将使用 CS50 IDE 进行完成和提交。

CS50 IDE 常用命令：

- 创建新文件夹。

  ```
  mkdir ~/pset1/
  ```

  eg: 在您的主目录中创建一个目录（即文件夹 pset1）。 `~` 表示您的home目录，`~/pset1` 表示一个名为`pset1` 的目录。

- 将自己移入（即打开）该目录。

  ```
  cd ~/pset1/
  ```

- clang 是一种编译程序，将源代码转换为机器代码。（clang 的操作可由 make xxx 代替）

  ```
  clang xxx.c
  ```

- 编译代码。

  ```
  make xxx
  ```

- 运行代码。

  ```
  ./xxx
  ```

# Mario More

Toward the beginning of World 1-1 in Nintendo’s Super Mario Brothers, Mario must hop over adjacent pyramids of blocks, per the below.

![pyramids](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004352.png)

Let’s recreate those pyramids in C, albeit in text, using hashes (`#`) for bricks, a la the below. Each hash is a bit taller than it is wide, so the pyramids themselves are also be taller than they are wide.

## Puprose

The program we’ll write will be called `mario`. And let’s allow the user to decide just how tall the pyramids should be by first prompting them for a positive integer between, say, 1 and 8, inclusive.

```
$ ./mario
Height: 8
       #  #
      ##  ##
     ###  ###
    ####  ####
   #####  #####
  ######  ######
 #######  #######
########  ########
```

## Method

![Snipaste_2020-07-09_23-09-55](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004406.png)

首先是对左边金字塔进行完成，因为右边只需要行数个 #，而左边要对空格和 # 进行处理。

最后使用判断的方式进行 **i+j<h-1** ，因为刚好在第四行的时候 “空格” 和 “#” 的数量是一个分水岭。其中i是行数，j 是列数，他们都是从 0 开始计数。

## The Code

```
/********************************************************
*   This is a simple C program that allows you to       *
*   determine just how tall the pyramids should be      *
*   by first prompting them for a positive integer      *
*   between, say, 1 and 8, inclusive.                   *
*                                                       *
*   Eren Lu                                             *
*   Fall 2020 CS50  pSet1 Mario （more comfortable）     *
*********************************************************/

#include <stdio.h>
#include <cs50.h>

int main(void)
{
    int h;

    do
    {
        //告诉用户要输入1-8的整数，否则再次提醒输入
        h = get_int("Please input the height:\n"); 
    }
    while(h<=0||h>8);
    
    for(int i=0;i<h;i++)
    {
        for(int j=0;j<h;j++)
        {
            //直接进行空格和#的判断
            if(i+j<h-1)
                printf(" ");
            else
            {
                printf("#");
            }
          
        } 
        printf("  ");
        
        for(int k=0; k<=i ; k++)
        {
            printf("#");
        }
        
        printf("\n");
    }
}
```

# Crash

假设你是一位收营员，你手中只有 25 cent, 10 cent, 5 cent, 1 cent面值的纸币供你进行找零。 在你找零时要依据优先使用能用的最面值进行找零，这样才能使总共找零纸币数量最少，即全局最优解。

## Puprose

请设计一个程序，已知应找顾客多少钱，在仅使用以上 4 种面值的纸币的情况下，求出找零多少张纸币的数量。输入为负数或字符时提示再次输入。

e.g.

```
$ ./cash
Change owed: -0.41
Change owed: foo
Change owed: 0.41
4
```

## Method

![Snipaste_2020-07-10_11-34-13](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004424.png)

需将 9 dollar 先转换为 900 cent。

## The Code

```
#include <stdio.h>
#include <cs50.h>
#include <math.h>

int main(void)
{
    //m 是全局变量，不能在do-while里面去定义：float m = get_float();
    float m;
    
    do
    {
        m = get_float("How much do I owe you?\n");
    }
    while(m<0);
    
    int cent = round(m*100);
    
    int a,b,c,d,n;
    a = cent/25;
    b = cent%25/10;
    c = cent%25%10/5;
    d = cent%25%10%5;
    
    n = a+b+c+d;
    printf("%i\n",n);
}
```

# Credit

美国信用卡的卡号有相应的数学关系，可以通过其关系判断是否为合法的信用卡卡号。[Luhn’s Algorithm](https://cs50.harvard.edu/x/2020/psets/1/credit/#luhns-algorithm) 算法就是一种判断信用卡卡号是否合法的一种算法。其定义为：

1. 从数字的第二位到最后一位开始，每隔一位数字乘以 2，然后将这些乘积的数字相加。
2. 将总和加到没有乘以 2 的数字的总和上。
3. 如果总和的最后一位数字是 0（或者，更正式地说，如果总和乘以10是与0同位的），那么这个数字就是有效的。

e.g. That’s kind of confusing, so let’s try an example with David’s Visa: 4003600000000014.

1. For the sake of discussion, let’s first underline every other digit, starting with the number’s second-to-last digit:

   4003600000000014

   Okay, let’s multiply each of the underlined digits by 2:

   1•2 + 0•2 + 0•2 + 0•2 + 0•2 + 6•2 + 0•2 + 4•2

   That gives us:

   2 + 0 + 0 + 0 + 0 + 12 + 0 + 8

   Now let’s add those products’ digits (i.e., not the products themselves) together:

   2 + 0 + 0 + 0 + 0 + 1 + 2 + 0 + 8 = 13

2. Now let’s add that sum (13) to the sum of the digits that weren’t multiplied by 2 (starting from the end):

   13 + 4 + 0 + 0 + 0 + 0 + 0 + 3 + 0 = 20

3. Yup, the last digit in that sum (20) is a 0, so David’s card is legit!

   ![Snipaste_2020-07-10_12-27-47](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008120027.png)

## Puprose

write a program that prompts the user for a credit card number and then reports (via `printf`) whether it is a valid American Express, MasterCard, or Visa card number, per the definitions of each’s format herein. So that we can automate some tests of your code, we ask that your program’s last line of output be `AMEX\n` or `MASTERCARD\n` or `VISA\n` or `INVALID\n`, nothing more, nothing less. 

## Method

![Snipaste_2020-07-10_12-26-16](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004740.png)

![Snipaste_2020-07-10_12-27-07](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004804.png)

![Snipaste_2020-07-10_12-49-45](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004836.png)

![Snipaste_2020-07-10_12-50-27](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812005831.png)

![Snipaste_2020-07-10_12-52-25](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200812004933.png)

重点在于对奇数和偶数部分的求和。奇数部分 for 循环使用 `i / 100` 来获得下一次奇数位(间隔两位)，使用 `oddsum % 10 ` 获得当前奇数位的数值。

偶数部分 for 循环，先 `x2 = i / 10` 让卡号从偶数位开始。由于偶数位需要 ×2 再相加，并且如果乘机为十位还需要拆开分别数字相加（e.g. 2×6=12，相加时是 1+2。此时在 for 循环中嵌套if判断，若乘机两位数则进行分解相加，否则直接相加，再同理求得偶数位和。

## The Code

```
/********************************************************
*   This is a simple C program that allows you to       *
*   determine whether a given credit card number        *
*   is a Visa, American Express, or MasterCard.         *
*                                                       *
*   Eren Lu                                             *
*   Fall 2020 CS50  pset 1                              *
*********************************************************/


#include <stdio.h>
#include <cs50.h>

int main(void)
{
    long i,cd, x2even;  
    int sum, oddsum, evensum;
    
    do
    {
        i = get_long("Please input your Credit Card number:\n");
    }while( i < 0);
    
    //奇数位求和
    for(cd = i, oddsum = 0; cd > 0; cd /= 100)
    {
        oddsum = oddsum + cd % 10;
    }
    
    //偶数位求和
    for(x2even = i / 10, evensum = 0; x2even > 0; x2even /= 100 )
    {
        //判断×2以后是否为两位数，若是则需要再次分离相加
        if(2 * (x2even % 10)> 9 )
        {
            evensum = evensum + 2*(x2even % 10) / 10;  //分离十位数并相加
            evensum = evensum + 2*(x2even % 10) % 10;  //分离个位数并相加
        }
        else 
            evensum = evensum + 2*(x2even % 10);
    }
    
    sum = oddsum + evensum;
        
    if(sum % 10 == 0)
    {
        if((i >= 340000000000000 && i < 350000000000000) || (i >= 370000000000000 && i < 380000000000000))
            printf("AMEX\n");
        else if(i >= 5100000000000000 && i < 5600000000000000)
            printf("MASTERCARD\n");
        else if((i >= 4000000000000 && i < 5000000000000) || (i >= 4000000000000000 && i < 5000000000000000))
            printf("VISA\n");
        else 
            printf("INVALID\n");
    }
    else 
        printf("INVALID\n");
        
    return 0;
}
```

# Summary

把 pset1做完整体下来陆陆续续花了小两天的时间。看似题数不多，可每一题都很扎实。最后一题在做的时候硬是想不出来，最后还是去网上搜寻了思路。虽然才第一次作业，但作为一名 C 很垃圾并且很久没用 C 的我，也不得不开始感叹 CS50 的课程设计真的名不虚传。

