---
layout:     post
title:      "HDU 2222 Keywords Search"
subtitle:   " \"AC自动机\""
date:       2019-1-23
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - AC自动机
    - 2019寒假集训
---

## Keywords Search

**Problem Description**

In the modern time, Search engine came into the life of everybody like Google, Baidu, etc.
Wiskey also wants to bring this feature to his image retrieval system.
Every image have a long description, when users type some keywords to find the image, the system will match the keywords with description of image and show the image which the most keywords be matched.
To simplify the problem, giving you a description of image, and some keywords, you should tell me how many keywords will be match.
 
**Input**

First line will contain one integer means how many cases will follow by.
Each case will contain two integers N means the number of keywords and N keywords follow. (N <= 10000)
Each keyword will only contains characters 'a'-'z', and the length will be not longer than 50.
The last line is the description, and the length will be not longer than 1000000.
 
**Output**

Print how many keywords are contained in the description.

**Sample Input**

>1<br>
5<br>
she<br>
he<br>
say<br>
shr<br>
her<br>
yasherhs<br>

**Sample Output**

>3

**题意**

给出一串模式串，然后一个文本串，求在文本串中出现多少次模式串

**题解**

AC自动机模板。。。迷茫中~~~

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
const int maxn = 1000100;
const int INF = 0x3f3f3f3f;
const int MOD = 998244353;

struct Trie{
    int count;
    Trie *fail;
    Trie *Next[26];
    Trie(){
        for(int i = 0; i < 26; i++){
            Next[i] = NULL;
        }
        fail = NULL;
        count = 0;
    }
} *root;

char str[maxn];

void build_trie_tree(char c[]){
    int len = strlen(c);
    Trie *temp = root;

    for(int i = 0; i < len; i++){
        int index = c[i] - 'a';
        if(temp->Next[index] == NULL){
            temp->Next[index] = new Trie();
        }
        temp = temp->Next[index];
    }
    temp->count++;
}

void build_ac_automation(){
    queue<Trie*> Q;
    Q.push(root);
    root->fail = NULL;
    while(!Q.empty()){
        Trie *temp = Q.front();
        Trie *p = NULL;
        Q.pop();
        for(int i = 0; i < 26; i++){
            if(temp->Next[i] != NULL){
                if(temp == root){
                    temp->Next[i]->fail = root;
                }
                else{
                    p = temp->fail;
                    while(p != NULL){
                        if(p->Next[i] != NULL){
                            temp->Next[i]->fail = p->Next[i];
                            break;
                        }
                        p = p->fail;
                    }
                    if(p == NULL){
                        temp->Next[i]->fail = root;
                    }
                }
                Q.push(temp->Next[i]);
            }
        }
    }
}

int query(){
    int cnt = 0;
    int len = strlen(str);
    Trie *temp = root;
    Trie *p = NULL;
    for(int i = 0; i < len; i++){
        int index = str[i] - 'a';
        while(temp->Next[index] == NULL && temp != root){
            temp = temp->fail;
        }
        temp = temp->Next[index];
        if(temp == NULL){
            temp = root;
        }
        p = temp;
        while(p != root && p->count != -1){
            cnt += p->count;
            p->count = -1;
            p = p->fail;
        }
    }
    return cnt;
}

int main()
{
    int t;
    cin>>t;
    while(t--){
        root = new Trie();
        int n;
        scanf("%d",&n);
        char s[110];
        for(int i = 1; i <= n; i++){
            scanf("%s",s);
            build_trie_tree(s);
        }
        build_ac_automation();
        scanf("%s",str);
        printf("%d\n",query());
    }
    return 0;
}
```