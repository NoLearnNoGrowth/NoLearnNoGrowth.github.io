---
layout:     post
title:      "HDU 4405 Aeroplane chess  "
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

## Aeroplane chess 

Hzz loves aeroplane chess very much. The chess map contains N+1 grids labeled from 0 to N. Hzz starts at grid 0. For each step he throws a dice(a dice have six faces with equal probability to face up and the numbers on the faces are 1,2,3,4,5,6). When Hzz is at grid i and the dice number is x, he will moves to grid i+x. Hzz finishes the game when i+x is equal to or greater than N. 

There are also M flight lines on the chess map. The i-th flight line can help Hzz fly from grid Xi to Yi (0<Xi<Yi<=N) without throwing the dice. If there is another flight line from Yi, Hzz can take the flight line continuously. It is granted that there is no two or more flight lines start from the same grid. 

Please help Hzz calculate the expected dice throwing times to finish the game. 

**Input**

There are multiple test cases. 
Each test case contains several lines. 
The first line contains two integers N(1≤N≤100000) and M(0≤M≤1000). 
Then M lines follow, each line contains two integers Xi,Yi(1≤Xi<Yi≤N).   
The input end with N=0, M=0. 

**Output**

For each test case in the input, you should output a line indicating the expected dice throwing times. Output should be rounded to 4 digits after decimal point. 

**Sample Input**

>2 0<br>
8 3<br>
2 4<br>
4 5<br>
7 8<br>
0 0<br>

**Sample Output**

>1.1667<br>
2.3441<br>

**题意**

给出0-n的n+1个格。其中有一些通道连接，在入口处可以选择直接到出口而```不需要```掷骰子，否则掷骰子(1-6)前进，当在i点时掷出的点数加i > n,就算过了终点n，求走到最后的掷骰子次数的期望

**题解**

概率DP，dp[i]表示到n还需要的步数的期望。

如果i处是通道入口，则dp[i] = dp[line[i]]

如果i不是通道入口，则dp[i] = (dp[i+1]+···+dp[i+6])/6.0

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

double dp[maxn];
int n,m;
int line[maxn];

int main()
{
    while(cin>>n>>m){
        if(n == 0 && m == 0){
            break;
        }
        me(dp,0);
        me(line,-1);
        for(int i = 1; i <= m; i++){
            int a,b;
            scanf("%d%d",&a,&b);
            line[a] = b;
        }
        for(int i = n-1; i >= 0; i--){
            if(line[i] != -1){
                dp[i] = dp[line[i]];
            }
            else{
                for(int j = i+1; j <= i+6; j++){
                    dp[i] += dp[j]/6.0;
                }
                dp[i] += 1.0;
            }
        }
        printf("%.4lf\n",dp[0]);
    }
    return 0;
}

```