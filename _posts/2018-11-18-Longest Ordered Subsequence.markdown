---
layout:     post
title:      "POJ 2533 Longest Ordered Subsequence "
subtitle:   " \"简单dp\""
date:       2018-11-18 15:00
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
---

## Longest Ordered Subsequence

**Time limit** ```2000``` ms     **Memory limit** ```65536``` kB

A numeric sequence of ai is ordered if a1 < a2 < ... < aN. Let the subsequence of the given numeric sequence ( a1, a2, ..., aN) be any sequence ( ai1, ai2, ..., aiK), where 1 <= i1 < i2 < ... < iK <= N. For example, sequence (1, 7, 3, 5, 9, 4, 8) has ordered subsequences, e. g., (1, 7), (3, 4, 8) and many others. All longest ordered subsequences are of length 4, e. g., (1, 3, 5, 8). 

Your program, when given the numeric sequence, must find the length of its longest ordered subsequence.

**Input**

The first line of input file contains the length of sequence N. The second line contains the elements of sequence - N integers in the range from 0 to 10000 each, separated by spaces. 1 <= N <= 1000

**Output**

Output file must contain a single integer - the length of the longest ordered subsequence of the given sequence.

**Sample Input**
<div class="zh post-container">
    <blockquote>
    7<br>
    1 7 3 5 9 4 8
    </blockquote>
</div>

**Sample Output**
<div class="zh post-container">
    <blockquote>
    4
    </blockquote>
</div>

**题意**

    最大递增子序列的长度

**思路**

    简单DP
    dp[i]表示1-i最大递增子序列的长度

**代码**

```
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
using namespace std;
const int maxn = 1010;
int dp[maxn],a[maxn];
int main()
{
    int n;
    cin>>n;
    for(int i =1; i <= n; i++)
        scanf("%d",&a[i]);
    int ans = 0;
    for(int i = 1; i <= n; i++)
    {
        int maxm = 0;
        for(int j = 1; j < i; j++)
            if(a[j] < a[i])
                maxm = max(dp[j],maxm);
        dp[i] = maxm + 1;
        ans = max(ans,dp[i]);    //最后一个不一定是最长的一个
    }
    cout<<ans<<endl;
    return 0;
}

```