---
layout:     post
title:      "LightOJ 1030  Discovering Gold"
subtitle:   " \"DP\""
date:       2019-1-7
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
    - 2019寒假集训
---

##  Discovering Gold 

You are in a cave, a long cave! The cave can be represented by a 1 x N grid. Each cell of the cave can contain any amount of gold.

Initially you are in position 1. Now each turn you throw a perfect 6 sided dice. If you get X in the dice after throwing, you add X to your position and collect all the gold from the new position. If your new position is outside the cave, then you keep throwing again until you get a suitable result. When you reach the Nth position you stop your journey. Now you are given the information about the cave, you have to find out the expected number of gold you can collect using the given procedure.

**Input**

Input starts with an integer T (≤ 100), denoting the number of test cases.

Each case contains a blank line and an integer N (1 ≤ N ≤ 100) denoting the dimension of the cave. The next line contains N space separated integers. The ith integer of this line denotes the amount of gold you will get if you come to the ith cell. You may safely assume that all the given integers will be non-negative and no integer will be greater than 1000.

**Output**

For each case, print the case number and the expected number of gold you will collect. Errors less than 10-6 will be ignored.

**Sample Input**

>3<br>
1<br>
101<br>
2<br>
10 3<br>
3<br>
3 6 9<br>

**Sample Output**

>Case 1: 101.0000000000<br>
Case 2: 13.000<br>
Case 3: 15<br>

**题意**

给出n个格子和每个格子的gold值，从第一个格子出发，每次掷骰子，值为1-6，前进到相应的格子并获得该格子的gold。若当前位置+掷出的值>n,则重新掷，最后一定到n。求获得gold的期望

**题解**

概率dp,dp[i]为到达第i个格子获得的gold的期望。dp[i] += (dp[i=1]/6+···+dp[i+6]/6).

求期望从后往前，求概率从前往后

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
const int maxn = 110;
const int INF = 0x3f3f3f3f;
const int MOD = 10007;
double dp[maxn];

int main()
{
    int t,ca = 1;
    cin>>t;
    while(t--){
        int n;
        me(dp,0);
        scanf("%d",&n);
        for(int i = 1; i <= n; i++){
            scanf("%lf",&dp[i]);
        }
        for(int i = n-1; i >= 1; i--){
            int k = min(6,n-i);
            for(int j = i+1; j <= i+k; j++){
                dp[i] += (double)dp[j]/k;
            }
        }
        printf("Case %d: %.10lf\n",ca++,dp[1]);  
    }    
    return 0;
}

```