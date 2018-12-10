---
layout:     post
title:      "HDU 1162 Eddy's picture "
subtitle:   " \"kruskal+并查集\""
date:       2018-12-10
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - 水题
---

## Eddy's picture

**Time limit** ```1000``` ms     **Memory limit** ```32768``` kB

Eddy begins to like painting pictures recently ,he is sure of himself to become a painter.Every day Eddy draws pictures in his small room, and he usually puts out his newest pictures to let his friends appreciate. but the result it can be imagined, the friends are not interested in his picture.Eddy feels very puzzled,in order to change all friends 's view to his technical of painting pictures ,so Eddy creates a problem for the his friends of you. 
Problem descriptions as follows: Given you some coordinates pionts on a drawing paper, every point links with the ink with the straight line, causes all points finally to link in the same place. How many distants does your duty discover the shortest length which the ink draws? 

**Input**

The first line contains 0 < n <= 100, the number of point. For each point, a line follows; each following line contains two real numbers indicating the (x,y) coordinates of the point. 

Input contains multiple test cases. Process to the end of file. 

**Output**

Your program prints a single real number to two decimal places: the minimum total length of ink lines that can connect all the points. 

**Sample Input**

>3<br>
1.0 1.0<br>
2.0 2.0<br>
2.0 4.0

**Sample Output**

>3.41

**题意**
---
最小生成树

**思路**
---
kruskal算法+并查集

模板题

**代码**

```
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <math.h>
#include <cstring>
#include <string>

using namespace std;

const int N = 105;
int root[N*(N-1)];

int FIND(int n)
{
    if(root[n] == -1)
        return n;
    return n == root[n] ? n : root[n] = FIND(root[n]);
}

bool MERGE(int a,int b)
{
    int r1 = FIND(a);
    int r2 = FIND(b);
    if(r1 == r2)
        return false;
    else
        root[r1] = r2;
    return true;
}

struct Point
{
    double x,y;
} p[N];

struct Edge
{
    int u,v;
    double w;
    friend bool operator<(Edge a,Edge b)
    {
        return a.w < b.w;
    }
} e[N*(N-1)];

int main()
{
    int n;
    while(cin>>n)
    {
        memset(root,-1,sizeof root);
        for(int i = 1; i <= n; i++)
            root[i] = i;
        for(int i = 1; i <= n; i++)
            scanf("%lf%lf",&p[i].x,&p[i].y);
        int k = 0;
        for(int i = 1; i <= n; i++)
        {
            for(int j = i; j <= n; j++)
            {
                    e[k].u = i;
                    e[k].v = j;
                    e[k++].w = sqrt((p[i].x - p[j].x)*(p[i].x - p[j].x) + (p[i].y - p[j].y)*(p[i].y - p[j].y));
            }
        }
        sort(e+1,e+k+1);
        double res = 0;
        for(int i = 0; i < k; i++)
        {
            if(MERGE(e[i].u,e[i].v))
            {
                res += e[i].w;
            }
        }
        printf("%.2lf\n",res);
    }
    return 0;
}
```