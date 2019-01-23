---
layout:     post
title:      "HDU 3746 Cyclic Nacklace"
subtitle:   " \"KMP\""
date:       2019-1-19
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - 2019寒假集训
---

## Cyclic Nacklace

```Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)```

**Problem Description**

CC always becomes very depressed at the end of this month, he has checked his credit card yesterday, without any surprise, there are only 99.9 yuan left. he is too distressed and thinking about how to tide over the last days. Being inspired by the entrepreneurial spirit of "HDU CakeMan", he wants to sell some little things to make money. Of course, this is not an easy task.

As Christmas is around the corner, Boys are busy in choosing christmas presents to send to their girlfriends. It is believed that chain bracelet is a good choice. However, Things are not always so simple, as is known to everyone, girl's fond of the colorful decoration to make bracelet appears vivid and lively, meanwhile they want to display their mature side as college students. after CC understands the girls demands, he intends to sell the chain bracelet called CharmBracelet. The CharmBracelet is made up with colorful pearls to show girls' lively, and the most important thing is that it must be connected by a cyclic chain which means the color of pearls are cyclic connected from the left to right. And the cyclic count must be more than one. If you connect the leftmost pearl and the rightmost pearl of such chain, you can make a CharmBracelet. Just like the pictrue below, this CharmBracelet's cycle is 9 and its cyclic count is 2:
![](/img/_post/Cyclic Nacklace.jpg)

Now CC has brought in some ordinary bracelet chains, he wants to buy minimum number of pearls to make CharmBracelets so that he can save more money. but when remaking the bracelet, he can only add color pearls to the left end and right end of the chain, that is to say, adding to the middle is forbidden.
CC is satisfied with his ideas and ask you for help.
 

**Input**

The first line of the input is a single integer T ( 0 < T <= 100 ) which means the number of test cases.
Each test case contains only one line describe the original ordinary chain to be remade. Each character in the string stands for one pearl and there are 26 kinds of pearls being described by 'a' ~'z' characters. The length of the string Len: ( 3 <= Len <= 100000 ).
 

**Output**

For each case, you are required to output the minimum count of pearls added to make a CharmBracelet.
 

**Sample Input**

>3<br>
aaa<br>
abca<br>
abcde
 
**Sample Output**

>0<br>
2<br>
5<br>

**题意**

需要添加多少珠子使手串由某一前缀颜色循环构成

**题解**

KMP的next应用，n-next[n]表示的是最小循环周期，所以如果周期等于数组长度，则代表没有循环，后面需要加等于数组的len长度的珠子才能达成条件。如果有循环节并且len%(len-NEXT[len]) == 0则表示不需要添加，已经满足条件。否则用最小的循环周期大小cycle减去多出的last,即为所求

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
#define me(x,y) memset(x,y,sizeof x);
#define max(x,y) (x > y ? x : y);
#define min(x,y) (x < y ? x : y);
#define lson rt<<1;
#define rson rt<<1|1;

typedef long long ll;
const int maxn = 100100;
const int INF = 0x3f3f3f3f;
const int MOD = 10007;

char s[maxn];
int NEXT[maxn];
int len;

void getnext(){
    me(NEXT,0);
    int i = 0,j = -1;
    NEXT[0] = -1;
    while(i < len){
        if(j == -1 || s[i] == s[j]){
            NEXT[++i] = ++j;
        }
        else{
            j = NEXT[j];
        }
    }
}

int main()
{
    int t;
    cin>>t;
    while(t--){
        scanf("%s",s);
        len = strlen(s);
        getnext();
        int cycle = len - NEXT[len];
        int part = len / cycle;
        int last = len % cycle;
        int ans = 0;
        if(cycle == len){
            ans = len;
        }
        else{
            if(last == 0){
                ans = 0;
            }
            else{
                ans = cycle - last;
            }
        }
        printf("%d\n",ans);
    }
    return 0;
}

```