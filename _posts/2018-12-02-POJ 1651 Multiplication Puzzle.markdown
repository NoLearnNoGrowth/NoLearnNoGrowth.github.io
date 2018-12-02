---
layout:     post
title:      "POJ 1651 Multiplication Puzzle"
subtitle:   " \"区间DP\""
date:       2018-12-02
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
---

## Multiplication Puzzle

The multiplication puzzle is played with a row of cards, each containing a single positive integer. During the move player takes one card out of the row and scores the number of points equal to the product of the number on the card taken and the numbers on the cards on the left and on the right of it. It is not allowed to take out the first and the last card in the row. After the final move, only two cards are left in the row. 

The goal is to take cards in such order as to minimize the total number of scored points. 

For example, if cards in the row contain numbers 10 1 50 20 5, player might take a card with 1, then 20 and 50, scoring 
10*1*50 + 50*20*5 + 10*50*5 = 500+5000+2500 = 8000

If he would take the cards in the opposite order, i.e. 50, then 20, then 1, the score would be 
1*50*20 + 1*20*5 + 10*1*5 = 1000+100+50 = 1150.

**Input**

The first line of the input contains the number of cards N (3 <= N <= 100). The second line contains N integers in the range from 1 to 100, separated by spaces.

**Output**

Output must contain a single integer - the minimal score.

**Sample Input**

>6<br>
10 1 50 50 20 5

**Sample Output**

>3650

**题意**

给n个数字，第一个和最后一个不能取，取第i个数字的代价为a[i-1] * a[i] * a[i+1],求最小代价

**思路**

状态转移方程：dp[i][j] = min(dp[i][j],dp[i][k]+dp[k][j]+line[i] * line[k] * line[j])

**代码**

```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<string>
#include<math.h>
#include<algorithm>

using namespace std;
const int N = 105;
const int INF = 0x3f3f3f3f;
int dp[N][N],line[N];

int main()
{
    int n;
    while(cin>>n)
    {
        for(int i = 1; i <= n; i++)
                cin>>line[i];
        for(int i = 1; i <= n; i++)
        {
            for(int j = i+2; j <= n; j++)
            {
                if(j == i+2)
                    dp[i][j] = line[i]*line[i+1]*line[i+2];
                else
                    dp[i][j] = INF;
            }
        }
        for(int len = 4; len <= n; len++)       //从4开始是因为前面有了初始化
        {
            for(int i = 1; i+len-1 <= n; i++)
            {
                int j = len+i-1;
                for(int k = i; k <= j; k++)
                    dp[i][j] = min(dp[i][j],dp[i][k]+dp[k][j]+line[i]*line[k]*line[j]);
            }
        }
        printf("%d\n",dp[1][n]);
    }
    return 0;
}
```