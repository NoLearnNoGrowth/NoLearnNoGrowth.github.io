---
layout:     post
title:      "HDU 2594  Simpsons' Hidden Talents"
subtitle:   " \"KMP\""
date:       2019-1-19
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - 2019寒假集训
---

##  Simpsons' Hidden Talents

Homer: Marge, I just figured out a way to discover some of the talents we weren’t aware we had. 
Marge: Yeah, what is it? 
Homer: Take me for example. I want to find out if I have a talent in politics, OK? 
Marge: OK. 
Homer: So I take some politician’s name, say Clinton, and try to find the length of the longest prefix 
in Clinton’s name that is a suffix in my name. That’s how close I am to being a politician like Clinton 
Marge: Why on earth choose the longest prefix that is a suffix??? 
Homer: Well, our talents are deeply hidden within ourselves, Marge. 
Marge: So how close are you? 
Homer: 0! 
Marge: I’m not surprised. 
Homer: But you know, you must have some real math talent hidden deep in you. 
Marge: How come? 
Homer: Riemann and Marjorie gives 3!!! 
Marge: Who the heck is Riemann? 
Homer: Never mind. 
Write a program that, when given strings s1 and s2, finds the longest prefix of s1 that is a suffix of s2.

**Input**

Input consists of two lines. The first line contains s1 and the second line contains s2. You may assume all letters are in lowercase.

**Output**

Output consists of a single line that contains the longest string that is a prefix of s1 and a suffix of s2, followed by the length of that prefix. If the longest such string is the empty string, then the output should be 0. 
The lengths of s1 and s2 will be at most 50000.

**Sample Input**

>clinton<br>
homer<br>
riemann<br>
marjorie<br>

**Sample Output**

>0<br>
rie 3

**题意**

求s1前缀s2后缀的最大匹配

**题解**

将s1,s2连接在一起求next数组，其中注意最大匹配不能大于min(len1,len2)

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
    char s1[maxn>>1],s2[maxn>>1];
    while(scanf("%s%s",s1,s2) != EOF){
        int len1 = strlen(s1);
        int len2 = strlen(s2);
        int mlen = min(len1,len2);
        strcpy(s,s1);
        strcat(s,s2);
        len = len1+len2;
        getnext();
        int k = len;
        while(NEXT[k] > mlen){
            k = NEXT[k];
        }

        if(NEXT[len] == 0){
            printf("0\n");
        }
        else{
            for(int i = 0; i < NEXT[k]; i++){
                printf("%c",s[i]);
            }
            printf(" %d\n",NEXT[k]);
        }
    }
    return 0;
}
```