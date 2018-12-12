---
layout:     post
title:      "HDU - 3037 Saving Beans (JAVA)"
subtitle:   " \"JAVA\""
date:       2018-12-12
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
---

## Saving Beans 

Although winter is far away, squirrels have to work day and night to save beans. They need plenty of food to get through those long cold days. After some time the squirrel family thinks that they have to solve a problem. They suppose that they will save beans in n different trees. However, since the food is not sufficient nowadays, they will get no more than m beans. They want to know that how many ways there are to save no more than m beans (they are the same) in n trees. 

Now they turn to you for help, you should give them the answer. The result may be extremely huge; you should output the result modulo p, because squirrels can’t recognize large numbers.

**Input**

The first line contains one integer T, means the number of cases. 

Then followed T lines, each line contains three integers n, m, p, means that squirrels will save no more than m same beans in n different trees, 1 <= n, m <= 1000000000, 1 < p < 100000 and p is guaranteed to be a prime.

**Output**

You should output the answer modulo p.

**Sample Input**

>2<br>
1 2 5<br>
2 1 5

**Sample Output**

>3<br>
3

        
  
**Hint**

For sample 1, squirrels will put no more than 2 beans in one tree. Since trees are different, we can label them as 1, 2 … and so on. 
The 3 ways are: put no beans, put 1 bean in tree 1 and put 2 beans in tree 1. For sample 2, the 3 ways are:
 put no beans, put 1 bean in tree 1 and put 1 bean in tree 2.

**题意**

求n个数的和不超过m的方案数。

**题解**

Lucas模板
无聊，正好又在学java.反正下学期也是一门课，提前学一学..所以....纯粹是把模板弄成了java版...刚学java遇到的坑，我的天....
while里面不像c++可以while(t),必须while(t>0)。输入也麻烦，定义Long,还要Long k = 1L.Long k = 1是错的，尤其是在vscode它不报错是最骚的，直到我用终端命令编译了一下才发现。麻烦的要死噻，也就是学java的时候顺便敲一敲了...

**代码**


```
import java.io.BufferedInputStream;
import java.math.BigInteger;
import java.util.Scanner;

public class Main {

    public static Long POW(Long x, Long y, Long p) {
        Long res = 1L;
        Long k = x;
        while (y != 0) {
            if ((y & 1) > 0)
                res = (res * k) % p;
            y >>= 1;
            k = k * k % p;
        }
        return res;
    }

    public static Long C(Long n, Long m, Long p) {
        Long k = 1L, t = 1L;
        if (m > n)
            return 0L;
        while (m != 0) {
            k = (k * n) % p;
            t = (t * m) % p;
            m--;
            n--;
        }
        return (k * POW(t, p - 2, p)) % p;
    }

    public static Long Lucas(Long n, Long m, Long p) {
        if (m == 0)
            return 1L;
        return ((Long) C(n % p, m % p, p) * (Long) Lucas(n / p, m / p, p)) % p;
    }

    public static void main(String[] args) {

        Scanner cin = new Scanner(new BufferedInputStream(System.in));
        int t = cin.nextInt();
        while (t != 0) {
            Long n = cin.nextLong();
            Long m = cin.nextLong();
            Long p = cin.nextLong();
            System.out.println(Lucas(n + m, m, p));
            t--;
        }

        cin.close();
    }
}
```