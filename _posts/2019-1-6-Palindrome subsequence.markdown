---
layout:     post
title:      "HDU 4632 Palindrome subsequence"
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

## Palindrome subsequence

In mathematics, a subsequence is a sequence that can be derived from another sequence by deleting some elements without changing the order of the remaining elements. For example, the sequence <A, B, D> is a subsequence of <A, B, C, D, E, F>. 
(http://en.wikipedia.org/wiki/Subsequence) 

Given a string S, your task is to find out how many different subsequence of S is palindrome. Note that for any two subsequence X = <S x1, S x2, ..., S xk> and Y = <S y1, S y2, ..., S yk> , if there exist an integer i (1<=i<=k) such that xi != yi, the subsequence X and Y should be consider different even if S xi = S yi. Also two subsequences with different length should be considered different.

**Input**

The first line contains only one integer T (T<=50), which is the number of test cases. Each test case contains a string S, the length of S is not greater than 1000 and only contains lowercase letters.

**Output**

For each test case, output the case number first, then output the number of different subsequence of the given string, the answer should be module 10007.

**Sample Input**

>4<br>
a<br>
aaaaa<br>
goodafternooneveryone<br>
welcometoooxxourproblems<br>

**Sample Output**

>Case 1: 1<br>
Case 2: 31<br>
Case 3: 421<br>
Case 4: 960<br>

**题意**

求含回文串的子序列的数量

**题解**

DP,dp[j][k]存j-k中回文串的数量，转移方程
    dp[j][k] = dp[j+1][k]+dp[j][k-1]-dp[j+1][k-1] dp[j+1][k-1]是重复加的，所以需要减一下
if(s[j] == s[k])
    dp[j][k] = dp[j+1][k-1]+1

需要注意的是MOD,每次取MOD记得加上MOD  0.0  又是犯了小错误WA了

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
const int maxn = 1010;
const int INF = 0x3f3f3f3f;
const int MOD = 10007;
int dp[maxn][maxn];

int main()
{
    int t,ca = 1;
    cin>>t;
    while(t--){
        string s;
        cin>>s;
        int len = s.size();
        me(dp,0);
        for(int i = 0; i < len; i++){
            dp[i][i] = 1;
        }
        ll sum = 0;
        for(int i = 1; i <= len; i++){
            for(int j = 0,k = j+i;k < len;j++,k++){
                dp[j][k] = (dp[j][k-1]+dp[j+1][k]-dp[j+1][k-1]+MOD)%MOD;
                if(s[j] == s[k])
                    dp[j][k] = (dp[j][k]+dp[j+1][k-1]+1+MOD)%MOD;
            }
        }
        printf("Case %d: %d\n",ca++,dp[0][len-1]);
    }
    return 0;
}
```