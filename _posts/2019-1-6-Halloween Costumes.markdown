---
layout:     post
title:      "LightOJ 1422 Halloween Costumes "
subtitle:   " \"DP\""
date:       2019-1-6
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
    - 2019寒假集训
---

## Halloween Costumes 

Gappu has a very busy weekend ahead of him. Because, next weekend is Halloween, and he is planning to attend as many parties as he can. Since it's Halloween, these parties are all costume parties, Gappu always selects his costumes in such a way that it blends with his friends, that is, when he is attending the party, arranged by his comic-book-fan friends, he will go with the costume of Superman, but when the party is arranged contest-buddies, he would go with the costume of 'Chinese Postman'.

Since he is going to attend a number of parties on the Halloween night, and wear costumes accordingly, he will be changing his costumes a number of times. So, to make things a little easier, he may put on costumes one over another (that is he may wear the uniform for the postman, over the superman costume). Before each party he can take off some of the costumes, or wear a new one. That is, if he is wearing the Postman uniform over the Superman costume, and wants to go to a party in Superman costume, he can take off the Postman uniform, or he can wear a new Superman uniform. But, keep in mind that, Gappu doesn't like to wear dresses without cleaning them first, so, after taking off the Postman uniform, he cannot use that again in the Halloween night, if he needs the Postman costume again, he will have to use a new one. He can take off any number of costumes, and if he takes off k of the costumes, that will be the last k ones (e.g. if he wears costume A before costume B, to take off A, first he has to remove B).

Given the parties and the costumes, find the minimum number of costumes Gappu will need in the Halloween night.

**Input**

Input starts with an integer T (≤ 200), denoting the number of test cases.

Each case starts with a line containing an integer N (1 ≤ N ≤ 100) denoting the number of parties. Next line contains N integers, where the ith integer ci (1 ≤ ci ≤ 100) denotes the costume he will be wearing in party i. He will attend party 1 first, then party 2, and so on.

**Output**

For each case, print the case number and the minimum number of required costumes.

**Sample Input**

>2<br>
4<br>
1 2 1 2<br>
7<br>
1 2 1 1 3 2 1

**Sample Output**

>Case 1: 3<br>
Case 2: 4<br>

**题意**

为舞会准备衣服，每次可以选择穿上衣服或者脱下衣服，脱下来的衣服不能再穿，求最小需要穿的衣服数量

**题解**

区间DP,dp[j][k]表示j-k穿的衣服数量，当k是选择穿上衣服的时候，dp[j][k] = dp[j][k-1]+1
在j-k间搜索是否有l满足c[l] == c[k],如果有就判断k穿或是不穿，k穿已经有了求不穿就可以了
dp[j][k] = min(dp[j][k],dp[j][l]+dp[l+1][k-1])

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
int dp[maxn][maxn];
int c[maxn];

int main()
{
    int t,ca = 1;
    cin>>t;
    while(t--){
        int n;
        cin>>n;
        me(dp,0);
        for(int i = 1; i <= n; i++){
            scanf("%d",&c[i]);
        }
        for(int i = 1; i <= n; i++){
            dp[i][i] = 1;
        }
        for(int i = 1; i <= n; i++){
            for(int j = 1,k = i+j; k <= n; j++,k++){
                dp[j][k] = dp[j][k-1]+1;
                for(int l = j; l < k; l++){
                    if(c[k] == c[l]){
                        dp[j][k] = min(dp[j][k],dp[j][l]+dp[l+1][k-1]);
                    }
                }
            }
        }
        printf("Case %d: %d\n",ca++,dp[1][n]);
    }
    return 0;
}

```