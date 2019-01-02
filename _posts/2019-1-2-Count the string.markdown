---
layout:     post
title:      "HDU 3336 Count the string "
subtitle:   " \"KMP\""
date:       2019-1-2
author:     "TanJX"
comments: true
header-img: "img/acm.jpg"
tags:
    - ACM
    - KMP
---

## Count the string 

It is well known that AekdyCoin is good at string problems as well as number theory problems. When given a string s, we can write down all the non-empty prefixes of this string. For example: 
s: "abab" 
The prefixes are: "a", "ab", "aba", "abab" 
For each prefix, we can count the times it matches in s. So we can see that prefix "a" matches twice, "ab" matches twice too, "aba" matches once, and "abab" matches once. Now you are asked to calculate the sum of the match times for all the prefixes. For "abab", it is 2 + 2 + 1 + 1 = 6. 
The answer may be very large, so output the answer mod 10007. 

**Input**

The first line is a single integer T, indicating the number of test cases. 
For each case, the first line is an integer n (1 <= n <= 200000), which is the length of string s. A line follows giving the string s. The characters in the strings are all lower-case letters. 

**Output**

For each case, output only one number: the sum of the match times for all the prefixes of s mod 10007.

**Sample Input**

>1<br>
4<br>
abab

**Sample Output**

>6

**题意**

求每个前缀在字符串里出现次数和

**题解**

回溯NEXT，sum+=每次回溯的次数

**代码**

```
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cmath>
#include <cstring>
#include <string>

using namespace std;
#define me(x,y) memset(x,y,sizeof x);

typedef long long ll;
const int maxn = 200100;
const int INF = 0x3f3f3f3f;
const int MOD = 10007;

int NEXT[maxn];
int n;
char str[maxn];

void GETNEXT()
{
    int i = 0,j = -1;
    NEXT[0] = -1;
    while(i < n){
        if(j == -1 || str[i] == str[j]){
            NEXT[++i] = ++j;
        }
        else{
            j = NEXT[j];
        }
    }
}

int main()
{
    int k = 1;
    int t;
    cin>>t;
    while(t--){
        int sum = 0;
        cin>>n>>str;
        GETNEXT();
        for(int i = 1;i <= n; i++){
            int k = i;
            while(k){
                sum = (sum+1)%MOD;
                k = NEXT[k];
            }
        }
        cout<<sum<<endl;
    }
    return 0;
}
```