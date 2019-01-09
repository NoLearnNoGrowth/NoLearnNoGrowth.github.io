---
layout:     post
title:      "POJ 3252 Round Numbers "
subtitle:   " \"DP\""
date:       2019-1-9
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
    - 2019寒假集训
---

## Round Numbers 

The cows, as you know, have no fingers or thumbs and thus are unable to play Scissors, Paper, Stone' (also known as 'Rock, Paper, Scissors', 'Ro, Sham, Bo', and a host of other names) in order to make arbitrary decisions such as who gets to be milked first. They can't even flip a coin because it's so hard to toss using hooves.

They have thus resorted to "round number" matching. The first cow picks an integer less than two billion. The second cow does the same. If the numbers are both "round numbers", the first cow wins,
otherwise the second cow wins.

A positive integer N is said to be a "round number" if the binary representation of N has as many or more zeroes than it has ones. For example, the integer 9, when written in binary form, is 1001. 1001 has two zeroes and two ones; thus, 9 is a round number. The integer 26 is 11010 in binary; since it has two zeroes and three ones, it is not a round number.

Obviously, it takes cows a while to convert numbers to binary, so the winner takes a while to determine. Bessie wants to cheat and thinks she can do that if she knows how many "round numbers" are in a given range.

Help her by writing a program that tells how many round numbers appear in the inclusive range given by the input (1 ≤ Start < Finish ≤ 2,000,000,000).

**Input**

Line 1: Two space-separated integers, respectively Start and Finish.

**Output**

Line 1: A single integer that is the count of round numbers in the inclusive range Start.. Finish

**Sample Input**

>2 12

**Sample Output**

>6

**题意**

给出一个区间，找到区间内满足将i变为二进制后0的数量 >= 1的数量的i的个数

**题解**

数位DP,dp[pos][n0][n1]表示在pos位有n0个0和n1个1的满足的条件的数的数量

要判断前导0，要不然前面无限补0，就全部满足条件了

**代码**

```
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cmath>
#include <cstring>
#include <string>
#include <vector>
#include <map>

using namespace std;
#define me(x,y) memset(x,y,sizeof x);

typedef long long ll;
const int maxn = 100100;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
ll dp[55][55][55];
ll n,m;
int a[200];
ll dfs(int pos,int n0,int n1,bool lead,bool limit){
    if(pos == -1){
        return n0 >= n1;
    }
    if(!limit && dp[pos][n0][n1] != -1){
        return dp[pos][n0][n1];
    }
    int ret = 0;
    int up = limit ? a[pos] : 1;
    for(int i = 0; i <= up; i++){
        if(lead){
            ret += dfs(pos-1,0,i == 1,i == 0 && lead,i == a[pos] && limit);
        }
        else{
            ret += dfs(pos-1,n0 + (i == 0),n1 + (i == 1),i == 0 && lead,i == a[pos] && limit);
        }
    }
    if(!limit){
        dp[pos][n0][n1] = ret;
    }
    return ret;
}

ll solve(ll x){
    int pos = 0;
    while(x){
        a[pos++] = x % 2;
        x /= 2;
    }
    return dfs(pos-1,0,0,true,true);
}
int main()
{
    me(dp,-1);
    while(cin>>n>>m){
        cout<<solve(m)-solve(n-1)<<endl;
    }
    return 0;
}

```