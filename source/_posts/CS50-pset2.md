---
title: CS50 | pset 2
index_img: 'https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/cs50pset.png'
banner_img: 'https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/CS50pSet_homework.jpg'
excerpt: 'CS50 Problem Set 2 (Fall 2020). Readability, Caesar Solutions. (missing Substitution)'
abbrlink: bea6
date: 2020-07-22 11:04:25
categories: 学习派
---

{% note success %}

本文仅记录笔者在 CS50 Problem Set 2 (Fall 2020) 中的解题思路和过程。如有错误或疏漏欢迎留言指正！

{% endnote %}

## Readability

设计一个程序，计算理解某些文本所需的大致等级，如下所示。

```
$ ./readability
Text: Congratulations! Today is your day. You're off to Great Places! You're off and away!
Grade 3
```

根据 [Scholastic ](https://www.scholastic.com/teachers/teaching-tools/collections/guided-reading-book-lists-for-every-level.html)的说法，[艾伯特·怀特](https://www.scholastic.com/teachers/teaching-tools/collections/guided-reading-book-lists-for-every-level.html)（EB White）的“夏洛特之网”介于二年级和四年级之间，而洛依·劳瑞（Lois Lowry）的“赐予者”介于八年级和十二年级之间。但是，一本书达到“四年级阅读水平”是什么意思？

好吧，在许多情况下，人类专家可能会读一本书，并决定他们认为该书最适合的等级。但是您也可以想象有一种算法试图弄清楚什么是文本阅读水平。

那么，较高的阅读水平具有哪些特征呢？好吧，更长的单词可能与更高的阅读水平相关。同样，较长的句子也可能与较高的阅读水平相关。多年来，已经开发了许多“可读性测试”，以提供一种公式化的过程来计算文本的阅读水平。

文本的阅读等级由 Coleman-Liau 索引来判断。其公式为：

```
index = 0.0588 * L - 0.296 * S - 15.8
```

此处，`L` 是文本中每100个单词的平均字母数，也是文本中每100个单词 `S` 的平均句子数。

### Puprose

设计并实现一个程序，该程序 `readability` 可计算文本的Coleman-Liau 索引。

- 在一个名为文件实现程序 `readability.c` 的`~/pset2/readability` 目录。
- 您的程序必须提示用户输入 `string` 文字（使用`get_string`）。
- 您的程序应计算文本中字母，单词和句子的数量。你可以假设一个字母是任何小写字符 `a `来 `z` 或从任何大写字符 `A` 到 `Z`，任何字符序列分离用空格应该算作一个字，并经过一段时间的任何事件，感叹号或问号表示结束一句话。
- 您的程序应打印为输出 `"Grade X"`，其中 `X` 是由 Coleman-Liau 公式计算的等级级别，四舍五入到最接近的整数。
- 如果得到的索引号是16或更高（等于或大于高年级本科生的阅读水平），则程序应输出 `"Grade 16+"` 而不是给出确切的索引号。如果索引号小于1，则程序应输出 `"Before Grade 1"`。

### Method

- 让用户输入一串文本。
- 计算字母（letters）数量，计算方法为：判断所输入字符串的范围是否在 'a' ~ 'z' 或者 ‘A’ ~ 'Z' 中。计算在范围内的所有字母数量。
- 计算单词（words）数量，计算方法为：判断字符串的值为 `空格` 时则记为一个单词，单词数+1。等于是计算文本中有多少个空格（默认文本中只会出现一个空格的情况），就求出有多少个单词数。
- 计算句子（sentences）的数量，计算方法为：判断字符串的值为 `!` , `.` 或 `?` 时则记作一个句子。
- 将上述结果带入 index 的公式中求的阅读等级。

![Snipaste_2020-08-21_12-47-37](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822113122.png)



![Snipaste_2020-08-21_12-48-11](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822113124.png)

![Snipaste_2020-08-21_12-48-54](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822150.png)

![](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822113123.png)

![Snipaste_2020-08-21_12-49-08](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/2020082251.png)

### Code

```
/********************************************************
*   This is a simple C program that computes the        *
*   approximate grade level needed to comprehend        *
*   some text.                                          *
*                                                       *
*   Eren Lu                                             *
*   Fall 2020 CS50  pSet2 Readability                   *
*********************************************************/


#include <stdio.h>
#include <cs50.h>
#include <string.h>
#include <math.h>

int main(void)
{
    int letterscount = 0;
    int wordscount = 1;
    int sentscount = 0;
    string text = get_string("Text: ");

    for(int i = 0; i < strlen(text); i++)
    
        //要用单引号，不要用双引号！，是把字符括起来
        if((text[i] >= 'a' && text[i] <= 'z') || (text[i] >= 'A' && text[i] <= 'Z'))
        {
            letterscount++;
        }
        else if(text[i] == ' ')
        {
            wordscount++;
        }
        else if(text[i] == '.' || text[i] == '!' || text[i] == '?')
        {
            sentscount++;
        }
    printf("%d letter(s);\n%d word(s);\n%d sentence(s);\n", letterscount, wordscount, sentscount);
    
    //letterscount 等要用 float 强制转换,不然会计算错误
    float L = ((float)letterscount / (float)wordscount) * 100, S = ((float)sentscount / (float)wordscount) * 100;
    float grade = 0.0588 * L - 0.296 * S -15.8;

    if(grade <= 16 && grade > 0)
        printf("Grade %i\n", (int) round(grade));
    else if(grade > 16)
        printf("Grade 16+\n");
    else if(grade < 1)
        printf("Before Grade 1\n");
}
```

### Summary

- 要对数组的概念有一定的认识。把数组想成一串箱子，每个箱子里面放了对应的一个字符。
- 在使用 ASCII 码进行比较时使用 'a' 单引号。
- 在类型不同的变量进行计算式应该使用强制类型转换。否则导致虽然编译不会报错，但最后的计算结果会出错。

## Caesar

根据以下内容，实现一个使用 Caesar 密码对消息进行加密的程序。

```
$ ./caesar 13
plaintext:  HELLO
ciphertext: URYYB
```

![Snipaste_2020-08-21_19-09-57](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822122019.png)

Caesar的算法（即密码）通过将每个字母 “旋转”  *k* 个位置来加密消息。更正式地，如果 *pi* 是一些明文（即，未加密的消息），那么每个字母 *ci* 在密文中计算为：

***ci = (pi + k) % 26***

其中的 `% 26` 意思是“除以26时的余数”。这个公式可能使密码看起来比实际更复杂，但这实际上只是精确表达算法的一种简洁方法。实际上，为了讨论起见，将A（或a）视为0，将B（或b）视为1，…，将H（或h）视为7，将I（或i）视为8，…，以及Z（或z）的值为25。

### Puprose

设计并实现一个程序，该程序 `caesar` 使用 Caesar 的密码对消息进行加密。

- 在一个名为文件实现程序 `caesar.c`的 `~/pset2/caesar` 目录。
- 您的程序必须接受一个命令行参数，一个非负整数。为了讨论起见，我们将其称为 *k*。
- 如果您的程序在执行时没有任何命令行参数或带有多个命令行参数，则您的程序应打印您选择的错误消息（带有 `printf`），并立即从 `main` 值 `1`（通常表示错误）返回。
- 如果命令行参数的任何字符都不是十进制数字，则程序应打印该消息 `Usage: ./caesar key` 并从 `main` 值返回 `1`。
- 您的程序必须输出 `plaintext:`（没有换行符），然后提示用户输入 `string` 纯文本（使用`get_string`）。
- 您的程序必须输出 `ciphertext:`（没有换行符），再输出明文的相应密文，明文中的每个字母字符都“旋转”了*k个*位置；非字母字符应原样输出。
- 您的程序必须保留大小写：大写字母尽管可以旋转，但必须保持大写字母；小写字母尽管旋转，但必须保持小写字母。
- 输出密文后，应打印换行符。然后，您的程序应通过 `0` 从返回，退出 `main`。

### Method

- 获取密钥
- 判断是否是有效的密钥；
- 用户输入明文
- 进行加密
- 输出密文

伪代码如下：

![Snipaste_2020-08-21_19-16-13](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822132445.png)

**argc & argv**

argc: argument count 

argv: argument vector

两者用于接受参数和记录参数信息。

我们经常用的 main 函数都是不带参数的。因此 main 后的括号都是空括号。实际上，main 函数可以带参数，这个参数可以认为是 main 函数的形式参数。Ｃ语言规定 main 函数的参数只能有两个，习惯上这两个参数写为 argc和 argv。因此，main 函数的函数头可写为： main (argc,argv)。Ｃ语言还规定 argc (第一个形参) 必须是整型变量, argv ( 第二个形参) 必须是指向字符串的指针数组。

由于 main 函数不能被其它函数调用，因此不可能在程序内部取得实际值。那么，在何处把实参值赋予 main 函数的形参呢? 实际上,main 函数的参数值是从操作系统命令行上获得的。当我们要运行一个可执行文件时，在DOS提示符下键入文件名，再输入实际参数即可把这些实参传送到main的形参中去。 DOS提示符下命令行的一般形式为： C:/>可执行文件名 参数 参数……



![Snipaste_2020-08-21_19-10-49](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822132741.png)

例如：

![Snipaste_2020-08-21_19-12-08](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008221349876785646.png)

在本题中 main 里面的两个参数满足以下条件：

![Snipaste_2020-08-21_19-11-40](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822133532.png)

例如：

![Snipaste_2020-08-21_18-12-45](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822133632.png)

**Key**

对输入的密钥进行检查，查看是否满足条件，否则输出相应提示。

![Snipaste_2020-08-21_23-04-58](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822133956.png)



**Encipher**

- 符合密钥条件之后，对明文进行加密；

- 只对大小写字母进行转换，不对其他符合或者数字进行转换；

- 其中用到了 `isupper` 和 `islower` 函数，对数组中的大写和小写字母进行判断并转换。

- 使用 `atoi` 函数对 `argv[1]` 中的数取整。

  `atoi()` 函数会扫描参数 str 字符串，跳过前面的空白字符（例如空格，tab缩进等，可以通过 isspace() 函数来检测），直到遇上数字或正负符号才开始做转换，而再遇到非数字或字符串结束时('\0')才结束转换，并将结果返回。【返回值】返回转换后的整型数；如果 str 不能转换成 int 或者 str 为空字符串，那么将返回 0。

![Snipaste_2020-08-21_23-05-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822134309.png)

![Snipaste_2020-08-21_18-31-30](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822134124.png)

先将 ASCII 码转化为字母表的顺序：

![Snipaste_2020-08-21_19-26-18](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/202008221927524.png)

![Snipaste_2020-08-21_19-19-20](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822134551.png)

再使用密文计算公式求出密文对应的字母表值，最后再加上 ASCII 码对应值转换为 ASCII 码的密文。

![Snipaste_2020-08-21_19-19-37](https://cdn.jsdelivr.net/gh/erenlu/PicGo/img/20200822134610.png)

### Code

```
/***********************************************************
*   This is a simple C program that encrypts messages      *
*   using Caesar’s cipher. Your program must accept a      *
*   single command-line argument: a non-negative integer.  * 
*   Let’s call it k for the sake of discussion. If your    * 
*   program is executed without any command-line arguments * 
*   or with more than one command-line argument, your      *
*   program should yell at the user and return a value of  *
*   1.                                                     *
*                                                          *
*   Eren Lu                                                *
*   Fall 2020 CS50  pSet2 Caesar                           *
***********************************************************/


#include <stdio.h>
#include <cs50.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>

//argc: argument count; argv: argument vector。两者用于接受参数和记录参数信息。
int main(int argc, string argv[])
{
    if(argc != 2)
    {
        printf("Usage: ./caesar key\n");
        return 1；
    }
    
    int k = atoi(argv[1]);
    
    if(k < 0)
    {
        printf("Usage: ./caesar key\n");
        return 1;
    }
    else 
    {
        string text = get_string("plain text: ");
        printf("ciphertext: ");
        int i;
        
        for(i = 0; i < strlen(text); i++)
        {
          
            if(isupper(text[i]))
            {
                //加密公式 c = （p + k）% 26
                printf("%c", (text[i] - 65 + k) % 26 + 65);
            }
            else if(islower(text[i]))
            {
                printf("%c", (text[i] - 97 + k) % 26 + 97);
            }
            else
            {
                printf("%c", text[i]);
            }
            
        }
    printf("\n");  
    return 0;
    }
}
```

### Summary

- 了解到 `main` 函数也是可以带参数的形式，一般都为 `main（argc, argv[]）`的参数形式。
- 学习了`isupper` `islower` `atoi` 三个函数的使用。
- 对 ASCII 码有了进一步的理解。

## Substitution (To be continued)

根据以下内容，实现一个实现替代密码的程序。

```
$ ./substitution JTREKYAVOGDXPSNCUIZLFBMWHQ
plaintext:  HELLO
ciphertext: VKXXN
```

在替换密码中，我们通过将每个字母替换为另一个字母来“加密”（即，以可逆方式隐藏）一条消息。为此，我们使用一个*密钥*：在这种情况下，加密时，字母表中每个字母到它应该对应的字母的映射。要“解密”消息，消息的接收者需要知道密钥，以便他们可以逆转该过程：将加密文本（通常称为*ciphertext*）转换回原始消息（通常称为*明文*）。

例如，键可能是字符串 `NQXPOMAFTRHLZGECYJIUWSKDVB`。此26个字符的键表示 `A`（字母的第一个字母）应转换为 `N`（键的第一个字符），`B`（字母的第二个字母）应转换为 `Q`（键的第二个字符），并且等等。

`HELLO` 然后，像这样的消息将被加密为 `FOLLE`，根据由密钥确定的映射替换每个字母。

### Puprose

设计并实现一个程序，该程序 `substitution` 使用替换密码对消息进行加密。

- 在一个名为文件实现程序 `substitution.c` 的 `~/pset2/substitution` 目录。
- 您的程序必须接受一个命令行参数，即用于替换的键。**键本身应该不区分大小写，因此键中的任何字符都是大写还是小写都不会影响程序的行为**。
- 如果您的程序在执行时没有任何命令行参数或带有多个命令行参数，则您的程序应打印您选择的错误消息（带有 `printf`），并立即从 `main` 值 `1`（通常表示错误）返回。
- 如果键无效（例如，不包含26个字符，包含不是字母字符的任何字符或不完全包含每个字母一次），则程序应打印您选择的错误消息（带有 `printf`）并从 `main` 值返回的`1`马上。
- 您的程序必须输出 `plaintext:`（没有换行符），然后提示用户输入 `string` 纯文本（使用 `get_string`）。
- 您的程序必须输出 `ciphertext:`（没有换行符），后跟明文的相应密文，并用明文中的每个字母字符替换密文中的相应字符；非字母字符应原样输出。
- **您的程序必须保留大小写：大写字母必须保持为大写字母；小写字母必须保留为小写字母。**
- 输出密文后，应打印换行符。然后，您的程序应通过 `0` 从返回，退出 `main` 。

### Method



## Reference

- [int main(int argc, const char * argv[])](https://www.jianshu.com/p/ab21bfe6a5b2)
- [My solution to CS50 pset2 - "Hail, Caesar!"](https://gist.github.com/CraigRodrigues/100ab759b6b6722ba3ad8acf12e26371)
- [CS50 Problem Set 2 (Fall 2019) - Substitution](https://gist.github.com/teeschorle/cc998f1ef47e270282d3d4fa3d217d31)

