---
title: Fractional programming & Dinkelbach theorem
date: 2018-10-14 10:21:56
cotegories:
- competitive programming
tags:
- algorithm
- binary search
- fractional programming
- dinkelbach theorem
---

Let's first consider such a problem: Given n objects, the i-th of them has value $v_i$ and has weight $w_i$. Now we want to pick some of them, say, the set of objects we pick is $S$. And we want to make $ f(S) = \frac{\sum_{i \in S} v_i}{\sum_{i \in S} w_i} $ maximized. How do we calculate that? 
This is actually a fundamental example of **fractional programming**. Here is normal form of **fractional programming**: 
$$ Minimize \ \ \lambda = f(x) = \frac{a(x)}{b(x)} \ (x \in S) $$ 
$x$ here is just an input among all the inputs in the whole space $S$, it can be anything, perhaps not only a simple variable. We can solve the problem by passing all $x$ from $S$ and get the result. But the size of $S$ could be usually very large or even infinite. So such solution is impractical. 

Then we calls for the **Dinkelbach** theorem. Now let's introduce a new function:
$$ g(\lambda) = \min_{x \in S}{ (a(x) - \lambda b(x)) } $$
Suppose $\lambda^{*}$ is the optimized value. The **Dinkelbach algorithm** says that:
1.    $ g(\lambda) = 0, \lambda = \lambda^{*} $
2.    $ g(\lambda) < 0, \lambda > \lambda^{*} $
3.    $ g(\lambda) > 0, \lambda < \lambda^{*} $ 

I won't give detailed proof here, it's actually easy to understand. $ g(\lambda) < 0 $ means that there exists some $x$ that satisfy $ a(x) - \lambda b(x) = 0$. So we can make $\lambda$ smaller. And $ g(\lambda) > 0 $ means that there is no such $x$, so this $\lambda$ is too small. 
With this, we can get the answer by binary search. We can fix a $\lambda$ and calculate the value of $g(\lambda)$ and finally approach the answer. **Dinkelbach** also works when we want to get the maximized value instead of the minimized one. But this time the function changes in this way:
$$ g(\lambda) = \max_{ x \in S }{(a(x) - \lambda b(x))} $$

Let's solve a sample problem of **fractional programming**: [poj 3111 - K Best](http://poj.org/problem?id=3111)
My code:
{% codeblock lang:cpp %}
#include <stdio.h>
#include <iostream>
#include <algorithm>
#include <string.h>
#include <vector>
#include <math.h>

using namespace std;

#define pb push_back
#define mp make_pair
#define ll long long
#define ull unsigned ll
#define db double
#define INF 0x3f3f3f3f
#define MOD 1000000007
#define PII pair<int, int>

const int N=100010;
int n,k;
int v[N],w[N];
struct node {
    db val;
    int idx;
    node(db a,int b):val(a),idx(b){}
    bool operator<(const node& rhs) const {
        return val>rhs.val;
    }
};
vector<node> vec;

bool check(db val) {
    vec.clear();
    for (int i=1;i<=n;i++) {
        vec.pb(node(v[i]-val*w[i],i));
    }
    sort(vec.begin(),vec.end());
    db tot=0.0;
    for (int i=0;i<k;i++) tot+=vec[i].val;
    return tot>0.0;
}

int main()
{
    scanf("%d%d",&n,&k);
    for (int i=1;i<=n;i++) {
        scanf("%d%d",v+i,w+i);
    }
    db lo=0.0,hi=1e11;
    for (int i=0;i<50;i++) {
        db mid=(lo+hi)*0.5;
        if (check(mid)) lo=mid;
        else hi=mid;
    }
    for (int i=0;i<k;i++) {
        printf("%d ",vec[i].idx);
    }
    printf("\n");
}
{% endcodeblock %}
This is almost an direct application of **Dinkelbach**. When we fix $\lambda$, we can get $g(\lambda)$ by sorting all items of $ v_i - \lambda w_i $, and sum k biggest items up. And finally we got the answer. 

