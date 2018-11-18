---
layout:     post
title:      "Codeforces-816B-Karen and Coffee"
date:       2018-11-18 15:00
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    
---

##Karen and Coffee 

[原题链接](https://codeforces.com/problemset/problem/816/B)
**Time limit** ```2500``` ms     **Memory limit** ```524288``` kB

To stay woke and attentive during classes, Karen needs some coffee!


Karen, a coffee aficionado, wants to know the optimal temperature for brewing the perfect cup of coffee. Indeed, she has spent some time reading several recipe books, including the universally acclaimed "The Art of the Covfefe".

She knows n coffee recipes. The i-th recipe suggests that coffee should be brewed between li and ri degrees, inclusive, to achieve the optimal taste.

Karen thinks that a temperature is admissible if at least k recipes recommend it.

Karen has a rather fickle mind, and so she asks q questions. In each question, given that she only wants to prepare coffee with a temperature between a and b, inclusive, can you tell her how many admissible integer temperatures fall within the range?

Input
The first line of input contains three integers, n, k (1 ≤ k ≤ n ≤ 200000), and q (1 ≤ q ≤ 200000), the number of recipes, the minimum number of recipes a certain temperature must be recommended by to be admissible, and the number of questions Karen has, respectively.

The next n lines describe the recipes. Specifically, the i-th line among these contains two integers li and ri (1 ≤ li ≤ ri ≤ 200000), describing that the i-th recipe suggests that the coffee be brewed between li and ri degrees, inclusive.

The next q lines describe the questions. Each of these lines contains a and b, (1 ≤ a ≤ b ≤ 200000), describing that she wants to know the number of admissible integer temperatures between a and b degrees, inclusive.

Output
For each question, output a single integer on a line by itself, the number of admissible integer temperatures between a and b degrees, inclusive.

**Input**

The first line of input contains three integers, n, k (1 ≤ k ≤ n ≤ 200000), and q (1 ≤ q ≤ 200000), the number of recipes, the minimum number of recipes a certain temperature must be recommended by to be admissible, and the number of questions Karen has, respectively.

The next n lines describe the recipes. Specifically, the i-th line among these contains two integers l~i~ and r~i~ (1 ≤ l~i~ ≤ r~i~ ≤ 200000), describing that the i-th recipe suggests that the coffee be brewed between l~i~ and r~i~ degrees, inclusive.

The next q lines describe the questions. Each of these lines contains a and b, (1 ≤ a ≤ b ≤ 200000), describing that she wants to know the number of admissible integer temperatures between a and b degrees, inclusive.

**Output**

For each question, output a single integer on a line by itself, the number of admissible integer temperatures between a and b degrees, inclusive.

**Examples**

**Input**
>3 2 4
91 94
92 97
97 99
92 94
93 97
95 96
90 100

**Output**
>3
3
0
4

**Input**
>2 1 1
1 1
200000 200000
90 100

**Output**
>0

**Note**

In the first test case, Karen knows 3 recipes.

1.The first one recommends brewing the coffee between 91 and 94 degrees, inclusive.
2.The second one recommends brewing the coffee between 92 and 97 degrees, inclusive.
3.The third one recommends brewing the coffee between 97 and 99 degrees, inclusive.
A temperature is admissible if at least 2 recipes recommend it.

She asks 4 questions.

In her first question, she wants to know the number of admissible integer temperatures between 92 and 94 degrees, inclusive. There are 3: 92, 93 and 94 degrees are all admissible.

In her second question, she wants to know the number of admissible integer temperatures between 93 and 97 degrees, inclusive. There are 3: 93, 94 and 97 degrees are all admissible.

In her third question, she wants to know the number of admissible integer temperatures between 95 and 96 degrees, inclusive. There are none.

In her final question, she wants to know the number of admissible integer temperatures between 90 and 100 degrees, inclusive. There are 4: 92, 93, 94 and 97 degrees are all admissible.

In the second test case, Karen knows 2 recipes.

1.The first one, "wikiHow to make Cold Brew Coffee", recommends brewing the coffee at exactly 1 degree.
2.The second one, "What good is coffee that isn't brewed at at least 36.3306 times the temperature of the surface of the sun?", recommends brewing the coffee at exactly 200000 degrees.
A temperature is admissible if at least 1 recipe recommends it.

In her first and only question, she wants to know the number of admissible integer temperatures that are actually reasonable. There are none.

**题意**

给n个区间，区间内每个点都+1，q次询问，输出每次询问的区间内有多少个点不小于k

**思路**

给n个区间，每次给出的区间l点+1,r+1点-1.n次更改后从1遍历到200000处理每个数字前面有多少个是不小于k的。复杂度为O(n).

**代码**

```
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>

using namespace std;
const int N = 2e5+10;
int point[N],sum[N];
int main()
{
    int l,r,n,k,q;
    while(cin>>n>>k>>q)
    {
        memset(point,0,sizeof point);
        memset(sum,0,sizeof sum);
        for(int i = 1; i <= n; i++)
        {
            scanf("%d%d",&l,&r);
            point[l]++;
            point[r+1]--;
        }
        int ans = 0,s = 0;
        for(int i = 1; i <= 200000; i++)
        {
            ans += point[i];
            if(ans >= k)
                s++;
            sum[i] = s;
        }
        while(q--)
        {
            scanf("%d%d",&l,&r);
            printf("%d\n",sum[r]-sum[l-1]);
        }
    }
    return 0;
}

```