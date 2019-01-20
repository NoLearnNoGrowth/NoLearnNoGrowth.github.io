---
layout:     post
title:      "HDU 1542 Atlantis"
subtitle:   " \"扫描线\""
date:       2019-1-20
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - 线段树
    - 2019寒假集训
---

## Atlantis

There are several ancient Greek texts that contain descriptions of the fabled island Atlantis. Some of these texts even include maps of parts of the island. But unfortunately, these maps describe different regions of Atlantis. Your friend Bill has to know the total area for which maps exist. You (unwisely) volunteered to write a program that calculates this quantity. 

**Input**

The input file consists of several test cases. Each test case starts with a line containing a single integer n (1<=n<=100) of available maps. The n following lines describe one map each. Each of these lines contains four numbers x1;y1;x2;y2 (0<=x1 < x2 <= 100000;0 <= y1 < y2 <= 100000), not necessarily integers. The values (x1; y1) and (x2;y2) are the coordinates of the top-left resp. bottom-right corner of the mapped area. 

The input file is terminated by a line containing a single 0. Don’t process it.

**Output**

For each test case, your program should output one section. The first line of each section must be “Test case #k”, where k is the number of the test case (starting with 1). The second one must be “Total explored area: a”, where a is the total explored area (i.e. the area of the union of all rectangles in this test case), printed exact to two digits to the right of the decimal point. 

Output a blank line after each test case. 

**Sample Input**

>2<br>
10 10 20 20<br>
15 15 25 25.5<br>
0<br>

**Sample Output**

>Test case #1<br>
Total explored area: 180.00 <br>

**题意**

给出n个矩形的对角坐标，求面积并

**题解**

线段树 扫描线 离散化 

扫描线基础网上的好像有些不同=.=  我是从左往右扫描，看了一天。。。还是有那么一点迷茫

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
#define me(x,y) memset(x,y,sizeof x)
#define lson rt<<1
#define rson rt<<1|1
#define max(a,b) a>b?a:b
#define min(a,b) a<b?a:b
typedef long long ll;
const int maxn = 210;
const int INF = 0x3f3f3f3f;
const int MOD = 998244353;

int n;
double Hash[maxn],len[maxn<<2],col[maxn<<2];

struct Node{
    double l,r,x,fg; //l就是线段的下端点，r是上端点，x是横坐标，fg是1,-1表示其是出还是入
    Node(){}
    Node(double _l,double _r,double _x,double _fg) : l(_l),r(_r),x(_x),fg(_fg){}
    bool operator < (const Node &a) const{  //后面按x排序，才按顺序扫描
        return x < a.x;
    }
}node[maxn];

void Pushup(int rt,int l,int r){
    if(col[rt]){    //col不为0表示扫描线所在的线段可以作为底边
        len[rt] = Hash[r+1] - Hash[l];
    }
    else if(l == r){    //线段长度为0
        len[rt] = 0;
    }
    else{
        len[rt] = len[lson] + len[rson];
    }
}

void update(int rt,int L,int R,int l,int r,int val){
    if(L <= l && R >= r){   //区间为扫描线段一部分，则区间底边总长更新，且扫描线段出入个数即底边个数更新
        col[rt] += val;
        Pushup(rt,l,r);
        return ;
    }
    int mid = (l + r) >> 1;
    if(L <= mid){
        update(lson,L,R,l,mid,val);
    }
    if(R > mid){
        update(rson,L,R,mid+1,r,val);
    }
    Pushup(rt,l,r);
}
int main()
{
    int ca = 1;
    while(cin>>n && n){
        int m = 0;
        double sum = 0;
        for(int i = 1;i <= n; i++){
            double x1,x2,y1,y2;
            scanf("%lf%lf%lf%lf",&x1,&y1,&x2,&y2);
            Hash[m] = y1;
            node[m++] = Node(y1,y2,x1,1);
            Hash[m] = y2;
            node[m++] = Node(y1,y2,x2,-1);
        }
        int d = 1;
        sort(Hash,Hash+m);
        sort(node,node+m);
        for(int i = 1; i < m; i++){ //离散化去重
            if(Hash[i-1] != Hash[i]){
                Hash[d++] = Hash[i];
            }
        }
        me(len,0);
        me(col,0);
        for(int i = 0; i < m; i++){ //按顺序扫描
            int l = lower_bound(Hash,Hash+d,node[i].l)-Hash;    //l,r均为离散化后的数组下标
            int r = lower_bound(Hash,Hash+d,node[i].r)-Hash-1;  //减一是为了防止区间缺失
            update(1,l,r,0,d-1,node[i].fg);
            sum += len[1]*(node[i+1].x - node[i].x);    //每次扫描的面积相加
        }
        printf("Test case #%d\nTotal explored area: %.2lf\n\n",ca++,sum);
    }
    return 0;
}

```