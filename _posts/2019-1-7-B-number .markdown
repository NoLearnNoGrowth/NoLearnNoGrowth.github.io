---
layout:     post
title:      "HDU 3652 B-number "
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

## B-number 

A wqb-number, or B-number for short, is a non-negative integer whose decimal form contains the sub- string "13" and can be divided by 13. For example, 130 and 2613 are wqb-numbers, but 143 and 2639 are not. Your task is to calculate how many wqb-numbers from 1 to n for a given integer n.

**Input**

Process till EOF. In each line, there is one positive integer n(1 <= n <= 1000000000).

**Output**

Print each answer in a single line.

**Sample Input**

>13<br>
100<br>
200<br>
1000<br>

**Sample Output**

>1<br>
1<br>
2<br>
2<br>

**题意**

求数字中含13且能除13

**题解**

数位dp,也是看到了大佬的题解才知道思路emmm

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
const int maxn = 25;
const int INF = 0x3f3f3f3f;
const int MOD = 10007;
ll dp[maxn][maxn][maxn];
/*
dp[pos][mod][flag]
flag
0:前一位不是1
1:前一位是1
2:出现过13
*/
int a[maxn];
ll n,m;

ll dfs(int pos,int mod,int flag,bool limit){        
    if(pos == -1){
        return mod == 0 && flag == 2;
    }
    if(!limit && dp[pos][mod][flag] != -1){
        return dp[pos][mod][flag];
    }
    int new_mod,new_flag;
    int up = limit ? a[pos] : 9;
    ll ans = 0;
    for(int i = 0; i <= up; i++){
        new_mod = (mod*10 + i) % 13;
        new_flag = flag;
        if(new_flag == 0 && i == 1){
            new_flag = 1;
        }
        if(new_flag == 1 && i == 3){
            new_flag = 2;
        }
        if(new_flag == 1 && i != 1){
            new_flag = 0;
        }
        
        ans += dfs(pos-1,new_mod,new_flag,limit && i == a[pos]);
    }
    if(!limit){
        dp[pos][mod][flag] = ans;
    }
    return ans;
}
ll slove(ll x){
    me(a,0);
    ll pos = 0;
    ll sum = 0;
    while(x){
        a[pos++] = x%10;
        x /= 10;
    }
    return dfs(pos-1,0,0,true);
}

int main()
{
    me(dp,-1);
    while(cin>>n){
        cout<<slove(n)<<endl;
    }
    return 0;
}

```