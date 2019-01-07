---
layout:     post
title:      "CodeForces 148D Bag of mice"
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

## Bag of mice

The dragon and the princess are arguing about what to do on the New Year's Eve. The dragon suggests flying to the mountains to watch fairies dancing in the moonlight, while the princess thinks they should just go to bed early. They are desperate to come to an amicable agreement, so they decide to leave this up to chance.

They take turns drawing a mouse from a bag which initially contains w white and b black mice. The person who is the first to draw a white mouse wins. After each mouse drawn by the dragon the rest of mice in the bag panic, and one of them jumps out of the bag itself (the princess draws her mice carefully and doesn't scare other mice). Princess draws first. What is the probability of the princess winning?

If there are no more mice in the bag and nobody has drawn a white mouse, the dragon wins. Mice which jump out of the bag themselves are not considered to be drawn (do not define the winner). Once a mouse has left the bag, it never returns to it. Every mouse is drawn from the bag with the same probability as every other one, and every mouse jumps out of the bag with the same probability as every other one.

**Input**

The only line of input data contains two integers w and b (0 ≤ w, b ≤ 1000).

**Output**

Output the probability of the princess winning. The answer is considered to be correct if its absolute or relative error does not exceed10 - 9.

**Examples**

**input**

>1 3

**output**

>0.500000000

**input**

>5 5

**output**

>0.658730159


**题意**

恶龙与公主关于新年夜的计划产生了分歧，他们决定从一个装有w只白鼠和b只黑鼠的袋子里抓老鼠，谁先抓到白鼠谁就获胜，公主先手。恶龙每抓一只老鼠，会有另一只老鼠由于受到惊吓从袋子里逃出来，这只老鼠不被认定为抓出来的，公主抓则不会出现这种情况。如果最后袋子中没有老鼠并且没有人抓到白鼠，那么恶龙胜利。每只老鼠被抓到与从袋子中逃出都是等可能的。

**题解**

dp[i][j]存袋子中还剩i只白鼠j只黑鼠时公主赢得概率

显然没有黑鼠时dp[i][0] = 1,而没有白鼠dp[0][i] = 0

公主一次抓到白鼠胜利的概率：i/(i+j)
（前提黑鼠数量>=2）公主黑鼠，恶龙黑鼠，跑出白鼠概率：j/(i+j)*(j-1)/(i+j-1)*i/(i+j-2)*dp[i-1][j-2]
（前提黑鼠数量>=3）公主黑鼠，恶龙黑鼠，跑出黑鼠概率：j/(i+j)*(j-1)/(i+j-1)*(j-2)/(i+j-2)*dp[i][j-3]


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
const int MOD = 10007;
double dp[maxn][maxn];
int w,b;
void init(){
    me(dp,0);
    for(int i = 1; i <= w; i++){
        dp[i][0] = 1;
    }
    for(int i = 1; i <= b; i++){
        dp[0][i] = 0;
    }
}
int main()
{
    
    while(cin>>w>>b){
        
        init();
        for(int i = 1; i <= w; i++){
            for(int j = 1; j <= b; j++){
                    dp[i][j] += (double)i/(i+j);
                if(j >= 2){
                    dp[i][j] += ((double)j/(i+j)*(double)(j-1)/(i+j-1)*(double)i/(i+j-2)*dp[i-1][j-2]);
                }
                if(j >= 3){
                    dp[i][j] += ((double)j/(i+j)*(double)(j-1)/(i+j-1)*(double)(j-2)/(i+j-2)*dp[i][j-3]); 
                }
            }
        }
        printf("%.9f\n",dp[w][b]);
    }
    return 0;
}

```