---
layout:     post
title:      "HDU 2196 Computer"
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

## Computer 

A school bought the first computer some time ago(so this computer's id is 1). During the recent years the school bought N-1 new computers. Each new computer was connected to one of settled earlier. Managers of school are anxious about slow functioning of the net and want to know the maximum distance Si for which i-th computer needs to send signal (i.e. length of cable to the most distant computer). You need to provide this information. 

![computer](/img/in_post/computer.jpg)

Hint: the example input is corresponding to this graph. And from the graph, you can see that the computer 4 is farthest one from 1, so S1 = 3. Computer 4 and 5 are the farthest ones from 2, so S2 = 2. Computer 5 is the farthest one from 3, so S3 = 3. we also get S4 = 4, S5 = 4.

**Input**

Input file contains multiple test cases.In each case there is natural number N (N<=10000) in the first line, followed by (N-1) lines with descriptions of computers. i-th line contains two natural numbers - number of computer, to which i-th computer is connected and length of cable used for connection. Total length of cable does not exceed 10^9. Numbers in lines of input are separated by a space.

**Output**

For each case output N lines. i-th line must contain number Si for i-th computer (1<=i<=N).

**Sample Input**

>5<br>
1 1<br>
2 1<br>
3 1<br>
1 1<br>

**Sample Output**

>3<br>
2<br>
3<br>
4<br>
4<br>

**题意**

给一个无向树，找到距离i节点最远的那个节点。

**题解

第一眼。。dfs，但是在集训，放在了树状dp里面，而且看着数据比较大，当时估计会超时。。。但是交完树状数组后还是没忍住敲了dfs,果不其然t了。树状dp解法，[here](https://www.cnblogs.com/dilthey/p/7186556.html)

**AC代码**

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
const int maxn = 10100;
const int INF = 0x3f3f3f3f;
const int MOD = 10007;
int n;
int cnt,dp[maxn][3],a[maxn];

int head[maxn];
struct Node{
    int to,next;
    int w;
} e[maxn*2];

void add(int u,int v,int w){
    e[cnt].to = v;
    e[cnt].w = w;
    e[cnt].next = head[u];
    head[u] = cnt++;
}

void dfs1(int u){
    if(head[u] == -1){
        dp[u][0] = 0;
        dp[u][1] = 0;
        return ;
    }
    for(int i = head[u]; i != -1; i = e[i].next){
        int v = e[i].to;
        dfs1(v);
        if(dp[u][0] < dp[v][0]+e[i].w){
            dp[u][1] = dp[u][0];
            dp[u][0] = dp[v][0] + e[i].w;
            a[u] = v;
        }
        else if(dp[u][1] < dp[v][0]+e[i].w){
            dp[u][1] = dp[v][0]+e[i].w;
        }
    }
}

void dfs2(int u){
    for(int i = head[u]; i != -1; i = e[i].next){
        int v = e[i].to;
        if(a[u] == v){
            dp[v][2] = max(dp[u][2],dp[u][1])+e[i].w;
        }
        else{
            dp[v][2] = max(dp[u][2],dp[u][0])+e[i].w;
        }
        dfs2(v);
    }
}

int main()
{
    while(cin>>n){
        cnt = 0;
        me(head,-1);
        me(dp,0);
        me(a,0);
        for(int i = 2; i <= n; i++){
            int a,b;
            scanf("%d%d",&a,&b);
            add(a,i,b);
        }
        dfs1(1);
        dfs2(1);
        for(int i = 1; i <= n; i++){
            printf("%d\n",max(dp[i][0],dp[i][2]));
        }
    }
    return 0;
}
```

**T掉的dfs**

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
const int maxn = 10100;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
int n,m,cnt;
int head[maxn],d[maxn];
int maxx = 0;
struct Edge{
    int to,next;
    int w;
} e[maxn*2];

void add(int u,int v,int w){
    e[++cnt].to = v;
    e[cnt].w = w;
    e[cnt].next = head[u];
    head[u] = cnt;
}

void dfs(int u,int last){
    for(int i = head[u]; i != -1; i = e[i].next){
        int v = e[i].to;
        if(v != last){
            d[v] = d[u] + e[i].w;
            maxx = max(maxx,d[v]);
            dfs(v,u);
        }
    }
}

int main()
{
    while(cin>>n){
        me(head,-1);
        me(d,0);
        cnt = 0;
        for(int i = 2; i <= n; i++){
            int a,b;
            scanf("%d%d",&a,&b);
            add(i,a,b);
            add(a,i,b);
        }
        for(int i = 1; i <= n; i++){
            me(d,0);
            maxx = 0;
            dfs(i,0);
            printf("%d\n",maxx);
        }
    }
    return 0;
}
```