---
title: Subsets Property Merge
date: 2018-07-07 10:47:37
categories:
- competitive programming
tags:
- algorithm
- dynamic programming
---

Given such a problem:
There is a sequence of length N: $ A_{0} $, $ A_{1} $,..., $ A_{N} $. For every integer K satisfying $ 1 \le K \le N $, find the maximum value of $A_{i}+A_{j}$ where $i\subseteq K$ and $j \subseteq K$. Here, **$a \subseteq b$** means that, in binary representation, the set of positions of a is a subset of that of b.
This problem is actually a subproblem of [ARC 100 E](https://beta.atcoder.jp/contests/arc100/tasks/arc100_c). The solution to such problem is very similar to dynamic programming and I think it could be used in many cases.
Code:
{% codeblock lang:cpp %}

#include <iostream>
#include <algorithm>
#include <string.h>
#include <math.h>
#include <vector>

using namespace std;

#define pb push_back
#define mp make_pair
#define ll long long
#define ull unsigned ll
#define db double
#define INF 0x3f3f3f3f
#define MOD 1000000007
#define PII pair<int, int>

const int N=1<<18;
int a[N];
PII mx[N];
int n;    // n is the count of digits

bool cmp(int i,int j) {
    return a[i]<a[j];
}

void solve() {
    vector<int> cur;
    for (int i=0;i<(1<<n);i++) {
        cur.clear();
        cur.pb(i);
        for (int j=0;j<n;j++) {
            if ((i>>j)&1) {
                int x=i^(1<<j);
                cur.pb(mx[x].first);
                if (mx[i].second!=-1) cur.pb(mx[x].second);
            }
        }
        sort(cur.begin(),cur.end());
        cur.resize(unique(cur.begin(),cur.end())-cur.begin());
        if (cur.size()==1) {
            mx[i]=mp(cur[0],-1);
        } else {
            mx[i]=mp(cur[0],cur[1]);
        }
    }
}

{% endcodeblock %}
The two elements of `mx[k]` are the two largest elements satisfying $i \subseteq k$ and $j \subseteq k$. So we can get the maximum value of $A_{i} + A_{j}$.
Here we compute the result of k from the result of its *subsets*(has only one bit different).

