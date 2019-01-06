---
layout:     post
title:      "POJ 3107 Godfather "
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

## Godfather

Last years Chicago was full of gangster fights and strange murders. The chief of the police got really tired of all these crimes, and decided to arrest the mafia leaders.

Unfortunately, the structure of Chicago mafia is rather complicated. There are n persons known to be related to mafia. The police have traced their activity for some time, and know that some of them are communicating with each other. Based on the data collected, the chief of the police suggests that the mafia hierarchy can be represented as a tree. The head of the mafia, Godfather, is the root of the tree, and if some person is represented by a node in the tree, its direct subordinates are represented by the children of that node. For the purpose of conspiracy the gangsters only communicate with their direct subordinates and their direct master.

Unfortunately, though the police know gangsters’ communications, they do not know who is a master in any pair of communicating persons. Thus they only have an undirected tree of communications, and do not know who Godfather is.

Based on the idea that Godfather wants to have the most possible control over mafia, the chief of the police has made a suggestion that Godfather is such a person that after deleting it from the communications tree the size of the largest remaining connected component is as small as possible. Help the police to find all potential Godfathers and they will arrest them.

**Input**

The first line of the input file contains n — the number of persons suspected to belong to mafia (2 ≤ n ≤ 50 000). Let them be numbered from 1 to n.

The following n − 1 lines contain two integer numbers each. The pair ai, bi means that the gangster ai has communicated with the gangster bi. It is guaranteed that the gangsters’ communications form a tree.

**Output**

Print the numbers of all persons that are suspected to be Godfather. The numbers must be printed in the increasing order, separated by spaces.

**Sample Input**

>6<br>
1 2<br>
2 3<br>
2 5<br>
3 4<br>
3 6<br>

**Sample Output**

>2 3

**题意**

找疑似黑帮教父的人，按顺序输出

**题解**

树形DP,树的重心

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
const int maxn = 50100;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
int num[maxn],head[maxn],ans[maxn];
struct Edge{
    int to,next;
} edge[maxn*2];

int n,cnt,minn,ans_n;

void add(int u,int v){
    edge[cnt].to = v;
    edge[cnt].next = head[u];
    head[u] = cnt++;
}

void dfs(int u,int root){
    num[u] = 1;
    int tmp = 0;
    for(int i = head[u]; i != -1; i = edge[i].next){
        int v = edge[i].to;
        if(v != root){
            dfs(v,u);
            num[u] += num[v];
            tmp = max(tmp,num[v]);  //子节点所形成的最大块的子节点数
        }
    }
    int k = max(tmp,n-num[u]);       //去掉点u后最大树的子节点数
    if(k < minn){
        minn = k;
        ans_n = 1;
        ans[ans_n++] = u;
    }
    else if(k == minn){
        ans[ans_n++] = u;
    }
}

int main()
{
    while(cin>>n){
        me(num,0);
        me(head,-1);
        cnt = 0;
        for(int i = 1; i < n; i++){
            int a,b;
            scanf("%d%d",&a,&b);
            add(a,b);
            add(b,a);
        }
        minn = INF;
        ans_n = 1;
        dfs(1,-1);
        sort(ans+1,ans+ans_n);
        for(int i = 1; i < ans_n; i++){
            printf("%d%c",ans[i],(i == ans_n) ? '\n' : ' ');
        }
    }
    return 0;
}

```