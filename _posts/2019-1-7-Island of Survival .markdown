---
layout:     post
title:      "Light 1265 Island of Survival"
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

## Island of Survival 

You are in a reality show, and the show is way too real that they threw into an island. Only two kinds of animals are in the island, the tigers and the deer. Though unfortunate but the truth is that, each day exactly two animals meet each other. So, the outcomes are one of the following

a)      If you and a tiger meet, the tiger will surely kill you.

b)      If a tiger and a deer meet, the tiger will eat the deer.

c)      If two deer meet, nothing happens.

d)      If you meet a deer, you may or may not kill the deer (depends on you).

e)      If two tigers meet, they will fight each other till death. So, both will be killed.

If in some day you are sure that you will not be killed, you leave the island immediately and thus win the reality show. And you can assume that two animals in each day are chosen uniformly at random from the set of living creatures in the island (including you).

Now you want to find the expected probability of you winning the game. Since in outcome (d), you can make your own decision, you want to maximize the probability.

**Input**

Input starts with an integer T (≤ 200), denoting the number of test cases.

Each case starts with a line containing two integers t (0 ≤ t ≤ 1000) and d (0 ≤ d ≤ 1000) where t denotes the number of tigers and d denotes the number of deer.

**Output**

For each case, print the case number and the expected probability. Errors less than 10-6 will be ignored.

**Sample Input**

>4<br>
0 0<br>
1 7<br>
2 0<br>
0 10<br>

**Sample Output**

>Case 1: 1<br>
Case 2: 0<br>
Case 3: 0.3333333333<br>
Case 4: 1<br>

**题意**

岛上n只老虎m只鹿，每次有两只遇见，虎吃人和鹿，鹿和鹿和平发育，人遇见鹿可杀可不杀

情况：
1.虎虎相遇
2.虎鹿相遇
3.人鹿相遇
4.鹿鹿相遇
5.人虎相遇

求人活下去的概率

**题解**

dp[i][j]还剩i只老虎j只鹿时人活下去的概率，首先几种特殊情况，0只虎，dp[0][j] = 1,奇数只虎，dp[i][j] = 0（虎必定是两两相争而死，所以奇数是必定会有存活的虎，人不可能活下去）

也正是因为只有虎虎相争才能消灭虎，所以鹿不影响人生存的概率，不考虑鹿。

第一次虎虎相争，从i+1中选两个动物（包括人）：(i+1)*i,从i中选两个动物（不包括人）：i*(i-1),所以虎虎相争概率(i-1)/(i+1)

后面每次虎虎相争概率为(i-1)/(i+1)*dp[i-2][j]

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
const int maxn = 1100;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
double dp[maxn][maxn];

int main()
{
    int t,ca = 1;
    int n,m;
    me(dp,0);
    for(int i = 0; i <= 1000; i++){
        dp[0][i] = 1;
    }
    for(int i = 1; i <= 1000; i++){
        for(int j = 0; j <= 1000; j++){
            dp[i][j] = (double)(i-1)/(i+1)*dp[i-2][j];
        }
    }
    cin>>t;
    while(t--){
        
        scanf("%d%d",&n,&m);
        if(n % 2 == 1){
            dp[n][m] = 0;
        }
        printf("Case %d: %.10lf\n",ca++,dp[n][m]);
    }    
    return 0;
}

```