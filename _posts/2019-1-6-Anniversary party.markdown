---
layout:     post
title:      "HDU 1520 Anniversary party"
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

## Anniversary party

There is going to be a party to celebrate the 80-th Anniversary of the Ural State University. The University has a hierarchical structure of employees. It means that the supervisor relation forms a tree rooted at the rector V. E. Tretyakov. In order to make the party funny for every one, the rector does not want both an employee and his or her immediate supervisor to be present. The personnel office has evaluated conviviality of each employee, so everyone has some number (rating) attached to him or her. Your task is to make a list of guests with the maximal possible sum of guests' conviviality ratings. 

**Input**

Employees are numbered from 1 to N. A first line of input contains a number N. 1 <= N <= 6 000. Each of the subsequent N lines contains the conviviality rating of the corresponding employee. Conviviality rating is an integer number in a range from -128 to 127. After that go T lines that describe a supervisor relation tree. Each line of the tree specification has the form: 
L K 
It means that the K-th employee is an immediate supervisor of the L-th employee. Input is ended with the line 
0 0

**Output**

Output should contain the maximal sum of guests' ratings. 

**Sample Input**

>7<br>
1<br>
1<br>
1<br>
1<br>
1<br>
1<br>
1<br>
1 3<br>
2 3<br>
6 4<br>
7 4<br>
4 5<br>
3 5<br>
0 0<br>

**Sample Output**

>5

**题意**

找到一个最大价值，使聚会时没有直接上下属。

**题解**

树形DP模板

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
const int maxn = 6100;
const int INF = 0x3f3f3f3f;
const int MOD = 100;
int dp[maxn][2],a[maxn],root[maxn];
vector<int> v[maxn];
int n;

void dfs(int root){
    int num = v[root].size();
    for(int i = 0; i < num; i++){
        dfs(v[root][i]);
        dp[root][0] += max(dp[v[root][i]][0],dp[v[root][i]][1]);
        dp[root][1] += dp[v[root][i]][0];
    }
}

int main()
{
    while(cin>>n){
        me(dp,0);
        me(a,0);
        me(root,-1);
        for(int i = 1; i <= n; i++){
            v[i].clear();
            scanf("%d",&a[i]);
            dp[i][1] = a[i];
            root[i] = i;
        }
        int l,k;
        while(~scanf("%d%d",&l,&k) && l && k){
            v[k].push_back(l);
            root[l] = k;
        }
        int r = 1;
        while(root[r] != r){
            r = root[r];
        }
        dfs(r);
        cout<<max(dp[r][0],dp[r][1])<<endl;
    }
    return 0;
}
```