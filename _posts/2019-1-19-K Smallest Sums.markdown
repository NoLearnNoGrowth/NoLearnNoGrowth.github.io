---
layout:     post
title:      "UVa 11997 K Smallest Sums"
subtitle:   " \"多路归并\""
date:       2019-1-19
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - 2019寒假集训
---

## K Smallest Sums

You're given k arrays, each array has k integers. There are kk ways to pick exactly one element in each array and calculate the sum of the integers. Your task is to find the k smallest sums among them.

**Input**

There will be several test cases. The first line of each case contains an integer k (2<=k<=750). Each of the following k lines contains k positive integers in each array. Each of these integers does not exceed 1,000,000. The input is terminated by end-of-file (EOF). The size of input file does not exceed 5MB.

**Output**

For each test case, print the k smallest sums, in ascending order.

**Sample Input**

>3<br>
1 8 5<br>
9 2 5<br>
10 7 6<br>
2<br>
1 1<br>
1 2<br>

**Output for the Sample Input**

>9 10 12<br>
2 2

**题意**

每行选一个数字相加，输出最小的n个

**题解**

使用优先队列的多路归并，每次处理两行，两两合并

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

typedef long long ll;
const int maxn = 100500;
const int INF = 0x3f3f3f3f;
const int MOD = 998244353;

struct Node{
    int s,b;
    bool operator < (Node a) const {
        return a.s < s;
    }
}node;

int t[755][755];
int n;
void merge(int *a,int *b,int *c){
    priority_queue<Node> Q;
    for(int i = 0; i < n; i++){
        Node r;
        r.s = a[i]+b[0];
        r.b = 0;
        Q.push(r);
    }
    for(int i = 0; i < n; i++){
        Node s = Q.top();
        Q.pop();
        c[i] = s.s;
        int j = s.b;
        if(j + 1 < n){
            Node r;
            r.s = s.s-b[j]+b[j+1];
            r.b = j+1;
            Q.push(r);
        }
    }
}
int main()
{
    
    while(cin>>n){
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                scanf("%d",&t[i][j]);
            }
            sort(t[i],t[i]+n);
        }

        for(int i = 1; i < n; i++){
            merge(t[0],t[i],t[0]);
        }
        for(int i = 0; i < n; i++){
            printf("%d%c",t[0][i],(i == n-1) ? '\n' : ' ');
        }
    }
    return 0;
}

```