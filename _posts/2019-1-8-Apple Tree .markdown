---
layout:     post
title:      "POJ 2486 Apple Tree "
subtitle:   " \"DP\""
date:       2019-1-8
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - DP
    - 2019寒假集训
---

## Apple Tree 

Wshxzt is a lovely girl. She likes apple very much. One day HX takes her to an apple tree. There are N nodes in the tree. Each node has an amount of apples. Wshxzt starts her happy trip at one node. She can eat up all the apples in the nodes she reaches. HX is a kind guy. He knows that eating too many can make the lovely girl become fat. So he doesn’t allow Wshxzt to go more than K steps in the tree. It costs one step when she goes from one node to another adjacent node. Wshxzt likes apple very much. So she wants to eat as many as she can. Can you tell how many apples she can eat in at most K steps.

**Input**

There are several test cases in the input 
Each test case contains three parts. 
The first part is two numbers N K, whose meanings we have talked about just now. We denote the nodes by 1 2 ... N. Since it is a tree, each node can reach any other in only one route. (1<=N<=100, 0<=K<=200) 
The second part contains N integers (All integers are nonnegative and not bigger than 1000). The ith number is the amount of apples in Node i. 
The third part contains N-1 line. There are two numbers A,B in each line, meaning that Node A and Node B are adjacent. 
Input will be ended by the end of file. 

Note: Wshxzt starts at Node 1.

**Output**

For each test case, output the maximal numbers of apples Wshxzt can eat at a line.

**Sample Input**

>2 1<br> 
0 11<br>
1 2<br>
3 2<br>
0 1 2<br>
1 2<br>
1 3<br>

**Sample Output**

>11<br>
2<br>

**题意**

给n个节点，有n-1条边相连，每个节点有自己的权值，可以走k步，求能获得的最大权值

**题解**

想不懂。。。面相大佬题解编程。

dp[i][j][0] 表示从i节点往下走j步且```不回来```获得的最大权值

dp[i][j][1] 表示从i节点往下走j步且```回来```获得的最大权值

用t表示分配给子节点的步数，又有t-1的1表示从i出去到子节点需要走一步，t-2的2表示走到子节点并回来需要走两步

状态转移方程：
dp[u][j][0] = max(dp[u][j][0],dp[u][j-t][1] + dp[v][t-1][0])        //先从别的子节点转回来，再走一次v子节点
dp[u][j][0] = max(dp[u][j][0],dp[u][j-t][0] + dp[v][t-2][1])        //先从v子节点转回来，再去走别的子节点
dp[u][j][1] = max(dp[u][j][1],dp[u][j-t][1] + dp[v][t-2][1])        //不管走了哪个子节点都要回来

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
const int maxn = 210;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
int dp[maxn][maxn][2];

struct Node{
    int to,next;
} e[maxn*2];

int cnt,n,k;
int head[maxn];

void add(int u,int v){
    e[cnt].to = v;
    e[cnt].next = head[u];
    head[u] = cnt++;
}

void dfs(int u,int pre){
    for(int i = head[u]; i != -1; i = e[i].next){
        int v = e[i].to;
        if(v == pre){
            continue;
        }
        dfs(v,u);
        for(int j = k ; j >= 1; j--){
            for(int t = 1; t <= j; t++){
                dp[u][j][0] = max(dp[u][j][0],dp[u][j-t][1] + dp[v][t-1][0]);
                dp[u][j][0] = max(dp[u][j][0],dp[u][j-t][0] + dp[v][t-2][1]);
                dp[u][j][1] = max(dp[u][j][1],dp[u][j-t][1] + dp[v][t-2][1]);
            }
        }
    }
}

int main()
{
    while(cin>>n>>k){
        me(dp,0);
        me(head,-1);
        cnt = 0;
        for(int i = 1; i <= n; i++){
            int a;
            scanf("%d",&a);
            for(int j = 0; j <= k; j++){
                dp[i][j][0] = dp[i][j][1] = a;
            }
        }

        for(int i = 1; i < n; i++){
            int a,b;
            scanf("%d%d",&a,&b);
            add(a,b);
            add(b,a);
        }
        dfs(1,0);
        printf("%d\n",max(dp[1][k][1],dp[1][k][0]));
    }
    return 0;
}

```