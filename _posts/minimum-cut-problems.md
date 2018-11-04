---
title: Minimum Cut Problems
date: 2018-07-15 11:25:19
categories:
- competitive programming
tags:
- algorithm
- max-flow
- minimum-cut
---

I think these problems are difficult because they are obscure. I mean, we can hardly recognize them and adopt a minimum-cut solution, at least for me. Maybe solving a great many of these problems would help. One characteristic of these problems is that we need to **divide some elements into two sets(source and sink) to get some optimal result**.
Let's see some problems:

### POJ 3469 - Dual Core CPU
Problem: [POJ 3469 - Dual Core CPU](http://poj.org/problem?id=3469)
There are 2 cores A, B and N tasks to be processed. The time cost for the i-th task on A, B are $a_{i}$, $b_{i}$ respectively. And for some tasks, when processed on two different cores, will exert an extra cost. For each task, decide which core should it be processed by to make the total cost minimized.
In this problem, we divide the tasks into two sets(processed on A, and on B). So we try to reduce it to a minimum-cut problem. Apparently, the minimized cost corresponds to the minimum cut.
For each task `i`, we add an edge from `source` to `i` whose weight equals to $b_{i}$. Then if this edge is among the cut set, task `i` is processed on core B. For the same reason, we add an edge from `i` to `sink` whose weight equals to $a_{i}$. Finally, for those tasks who effect each other, say `i` and `j`, we add two edges between `i` and `j`(in two directions), whose weight equals to the extra cost. Now the minumum cut of the graph is equal to the minimized cost we want to get.
Pseudo code:
{% codeblock lang:cpp %}
int main() {
    for (int i=1;i<=n;i++) {
        scanf("%d%d",&a,&b);
        add_edge(0,i,b);
        add_edge(i,0,0);    // reverse edge
        add_edge(i,n+1,a);
        add_edge(n+1,i,0);  // reverse edge
    }
    for (int i=1;i<=m;i++) {
        scanf("%d%d%d",&a,&b,&w);    // extra cost
        add_edge(a,b,w);
        add_edge(b,a,0);    // reverse edge
        add_edge(b,a,w);
        add_edge(a,b,0);    // reverse edge
    }
    ll mx_flow=max_flow();  // get the answer
}
{% endcodeblock %}

### Code Jam - World Finals 2009 - Problem D. Wi-fi Towers
Problem: [Wi-fi Towers](https://code.google.com/codejam/contest/311101/dashboard#s=p3)
Given N wi-fi towers, the i-th is located at $x_{i}$, $y_{i}$, and has a range $r_{i}$. Now we want to upgrade the protocol of them. The score of upgrading the i-th tower is $s_{i}$. When the i-th tower is upgraded, all towers within its range have to be upgraded as well. Decide which towers to be upgraded to make the total score maximized.
Wow, world final problem!
It's quite difficult to associate this with minimum-cut. The two sets are **towers to be upgraded** and **towers not to be upgraded**.
We want maximum score, so we calculate the minimum **lost**. If $s_{i}$ is negative, the absolute value of it is a lost. We add an edge from `source` to `i` whose weight is $|s_{i}|$. If $s_{i}$ is positive, we can imagine that tower i has already been upgraded at first, and "upgrading" it to the origin exerts a lost. We add an edge from `i` to `sink` whose weight is $|s_{i}|$. We need to add the positive values to the answer, of course.
Then consider the constraints. If `j` has to be upgraded if `i` is upgraded, we add an edge from j to i, whose weight is $\infty$. As a result, if we upgrade `i` without upgrading `j`, the cut value will be infinity.
Now we reduce the minimized lost from the answer, and we get the maximum score.
Pseudo code:
{% codeblock lang:cpp %}
int main() {
    int src=0,sink=n+1;
    int ans=0;
    for (int i=1;i<=n;i++) {
        if (s[i]<0) {
            add_edge(0,i,-s[i]);
            add_edge(i,0,0);    // reverse edge
        } else {
            add_edge(i,n+1,s[i]);
            add_edge(n+1,i,0);  // reverse edge
            ans+=s[i];
        }
        for (int j=1;j<=n;j++) {
            if (i==j) continue;
            if (sqr(x[i]-x[j])+sqr(y[i]-y[j])<=sqr(r[i])) {    // within the range
                add_edge(j,i,INF);
                add_edge(i,j,0);    // reverse edge
            }
        }
    }
    int mx_flow=max_flow();
    ans-=mx_flow;
}
{% endcodeblock %}

Hope I will recognize the minimum-cut problem when I meet one next time...
