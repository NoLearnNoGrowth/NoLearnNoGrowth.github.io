---
layout:     post
title:      "HDU 1074 Doing Homework"
subtitle:   " \"DP\""
date:       2019-1-8
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
    - 2019寒假集训
---

## Doing Homework 

Ignatius has just come back school from the 30th ACM/ICPC. Now he has a lot of homework to do. Every teacher gives him a deadline of handing in the homework. If Ignatius hands in the homework after the deadline, the teacher will reduce his score of the final test, 1 day for 1 point. And as you know, doing homework always takes a long time. So Ignatius wants you to help him to arrange the order of doing homework to minimize the reduced score.

**Input**

The input contains several test cases. The first line of the input is a single integer T which is the number of test cases. T test cases follow. 
Each test case start with a positive integer N(1<=N<=15) which indicate the number of homework. Then N lines follow. Each line contains a string S(the subject's name, each string will at most has 100 characters) and two integers D(the deadline of the subject), C(how many days will it take Ignatius to finish this subject's homework). 

Note: All the subject names are given in the alphabet increasing order. So you may process the problem much easier. 

**Output**

For each test case, you should output the smallest total reduced score, then give out the order of the subjects, one subject in a line. If there are more than one orders, you should output the alphabet smallest one. 

**Sample Input**

>2<br>
3<br>
Computer 3 3<br>
English 20 1<br>
Math 3 2<br>
3<br>
Computer 3 3<br>
English 6 3<br>
Math 6 3<br>

**Sample Output**

>2<br>
Computer<br>
Math<br>
English<br>
3<br>
Computer<br>
English<br>
Math<br>

**Hint**

In the second test case, both Computer->English->Math and Computer->Math->English leads to reduce 3 points, but the 
word "English" appears earlier than the word "Math", so we choose the first order. That is so-called alphabet order.

**题意**

刚开始学状压dp，真的迷茫

给出n个作业的名称，截止时间，花费时间，做作业未按时完成，超过截止时间每一天扣一分

问完成n个作业最少要扣多少分，并输出扣分最少的做作业的顺序，有多种方法按字典序最小的输出

**题解**

状压DP,n个作业，情况有2^n种,且0为一个作业没做，2^n-1为作业全部完成

如果在状态i下j号作业还没做((i & (1 << j)) == 0)，则判断在这个状态下做这个作业的扣的分数

**代码**

```
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cmath>
#include <cstring>
#include <string>
#include <vector>

using namespace std;
#define me(x,y) memset(x,y,sizeof x);

typedef long long ll;
const int maxn = 1 << 16;
const int INF = 0x3f3f3f3f;
const int MOD = 100;

struct Node{
    int cost,pre,reduced;
}dp[maxn];
/*
cost花费时间
pre前一状态
reduced最少扣的分数
*/
struct Course{
    int deadline,cost;
    char name[200];
}c[16];

int n,vis[maxn];

void Output(int x){
    int a = dp[x].pre^x; // a判断和上次相比，这次做的是哪一个作业
    int id = 0;
    a >>= 1;
    while(a){
        id++;
        a >>= 1;
        
    }
    if(dp[x].pre != 0){
        Output(dp[x].pre);
    }
    printf("%s\n",c[id].name);
}

int main()
{
    int t;
    cin>>t;
    while(t--){
        scanf("%d",&n);
        int up = 1  << n;
        for(int i = 0; i < n; i++){
            scanf("%s%d%d",c[i].name,&c[i].deadline,&c[i].cost);
        }
        me(vis,0);
        dp[0].cost = 0,dp[0].pre = -1,dp[0].reduced = 0;
        vis[0] = 1;
        int tup = up-1;
        for(int i = 0; i < tup; i++){
            for(int j = 0; j < n; j++){
                int cnt = 1 << j;
                if((cnt & i) == 0){
                    int _cnt = cnt|i;
                    dp[_cnt].cost = dp[i].cost + c[j].cost;
                    int reduce = dp[_cnt].cost - c[j].deadline;
                    if(reduce < 0){
                        reduce = 0;
                    }
                    reduce += dp[i].reduced;
                    if(vis[_cnt]){
                        if(reduce < dp[_cnt].reduced){
                            dp[_cnt].reduced = reduce;
                            dp[_cnt].pre = i;
                        }
                    }
                    else{
                        vis[_cnt] = 1;
                        dp[_cnt].reduced = reduce;
                        dp[_cnt].pre = i;
                    }
                }
            }
        }
        printf("%d\n",dp[tup].reduced);
        Output(tup);

    }
    return 0;
}

```