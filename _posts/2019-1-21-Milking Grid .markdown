---
layout:     post
title:      "POJ 2185 Milking Grid "
subtitle:   " \"KMP\""
date:       2019-1-21
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - 2019寒假集训
---

## Milking Grid 

Every morning when they are milked, the Farmer John's cows form a rectangular grid that is R (1 <= R <= 10,000) rows by C (1 <= C <= 75) columns. As we all know, Farmer John is quite the expert on cow behavior, and is currently writing a book about feeding behavior in cows. He notices that if each cow is labeled with an uppercase letter indicating its breed, the two-dimensional pattern formed by his cows during milking sometimes seems to be made from smaller repeating rectangular patterns. 

Help FJ find the rectangular unit of smallest area that can be repetitively tiled to make up the entire milking grid. Note that the dimensions of the small rectangular unit do not necessarily need to divide evenly the dimensions of the entire milking grid, as indicated in the sample input below. 

**Input**

*Line 1: Two space-separated integers: R and C 

*Lines 2..R+1: The grid that the cows form, with an uppercase letter denoting each cow's breed. Each of the R input lines has C characters with no space or other intervening character. 

**Output**

*Line 1: The area of the smallest unit from which the grid is formed 

**Sample Input**

>2 5<br>
ABABA<br>
ABABA<br>

**Sample Output**

>2

**Hint**

The entire milking grid can be constructed from repetitions of the pattern 'AB'.

**题意**

给出一个矩阵，找到一个最小的矩阵，可以通过复制这个矩阵得到包含原矩阵的矩阵。求这个最小矩阵的面积

**题解**

二维KMP,最小矩阵覆盖

通过原矩阵得到一个转置矩阵，分两步分别计算行的最短循环部分和列的最小循部分，相乘即为答案

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
#include <queue>

using namespace std;
#define me(x,y) memset(x,y,sizeof x)
#define lson rt<<1
#define rson rt<<1|1
#define max(a,b) a>b?a:b
#define min(a,b) a<b?a:b
typedef long long ll;
const int maxn = 10100;
const int INF = 0x3f3f3f3f;
const int MOD = 998244353;

int Next[maxn];
int n,m;
char s[maxn][88];
char s0[88][maxn];

int main()
{
    scanf("%d%d",&n,&m);
    for(int i = 0; i < n; i++){
        scanf("%s",s[i]);
    }
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            s0[j][i] = s[i][j];
        }
    }
    int i = 0,j = -1;
    Next[0] = -1;
    while(i < n){
        if(j == -1 || strcmp(s[i],s[j]) == 0){
            Next[++i] = ++j;
        }
        else{
            j = Next[j];
        }
    }
    int mn = n-Next[n];
    i = 0,j = -1;
    Next[0] = -1;
    while(i < m){
        if(j == -1|| strcmp(s0[i],s0[j]) == 0){
            Next[++i] = ++j;
        }
        else{
            j = Next[j];
        }
    }
    int mm = m-Next[m];
    printf("%d\n",mm*mn);
    return 0;
}
```