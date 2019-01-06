---
layout:     post
title:      "HDU 4607 Park Visit"
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

## Park Visit

Claire and her little friend, ykwd, are travelling in Shevchenko's Park! The park is beautiful - but large, indeed. N feature spots in the park are connected by exactly (N-1) undirected paths, and Claire is too tired to visit all of them. After consideration, she decides to visit only K spots among them. She takes out a map of the park, and luckily, finds that there're entrances at each feature spot! Claire wants to choose an entrance, and find a way of visit to minimize the distance she has to walk. For convenience, we can assume the length of all paths are 1. 
Claire is too tired. Can you help her? 

**Input**

An integer T(T≤20) will exist in the first line of input, indicating the number of test cases. 
Each test case begins with two integers N and M(1≤N,M≤10 5), which respectively denotes the number of nodes and queries. 
The following (N-1) lines, each with a pair of integers (u,v), describe the tree edges. 
The following M lines, each with an integer K(1≤K≤N), describe the queries. 
The nodes are labeled from 1 to N. 

**Output**

For each query, output the minimum walking distance, one per line.

**Sample Input**

>1<br>
4 2<br>
3 2<br>
1 2<br>
4 2<br>
2<br>
4<br>

**Sample Output**

>1<br>
4<br>

**题意**

莱克尔和她的朋友到公园玩，公园很大也很漂亮。公园包含n个景点通过n-1条边相连。克莱尔太累了，所以不能去参观所有点景点。经过深思熟虑，她决定只访问其中的k个景点。她拿出地图发现所有景点的入口都很特殊。所以她想选择一个入口，并找到一条最短的路来参观k个景点。假设景点之间的距离为1。

**题解**

树的直径，两遍dfs跑出来的最大距离一定是直径。虽然说正在学树形DP，而且这题可以用，但是不自觉的就去敲了DFS...

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
const int maxn = 100100;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
int n,m,cnt;
int head[maxn],d[maxn];
struct Edge{
    int to,next;
} e[maxn*2];

void add(int u,int v){
    e[++cnt].to = v;
    e[cnt].next = head[u];
    head[u] = cnt;
}

void dfs(int u,int last){
    for(int i = head[u]; i != -1; i = e[i].next){
        int v = e[i].to;            //智障的我手快把这里的i写成了u找bug找了一个小时
        if(v != last){
            d[v] = d[u] + 1;
            dfs(v,u);
        }
    }
}

int main()
{
    int t;
    cin>>t;
    while(t--){
        cin>>n>>m;
        me(head,-1);
        cnt = 0;
        for(int i = 1; i < n; i++){
            int u,v;
            scanf("%d%d",&u,&v);
            add(u,v);
            add(v,u);
        }
        d[1] = 0;
        dfs(1,0);
        int root = 0;
        d[root] = 0;
        for(int i = 1; i <= n; i++){
            if(d[root] < d[i]){
                root = i;
            }
        }
        d[root] = 0;
        dfs(root,0);
        int len = 0;
        for(int i = 1; i <= n; i++){
            len = max(len,d[i]);
        }
        for(int i = 1; i <= m; i++){
            int k;
            scanf("%d",&k);
            printf("%d\n",(k <= len) ? k-1 : len+(k-1-len)*2);
        }
    }
    return 0;
}

```