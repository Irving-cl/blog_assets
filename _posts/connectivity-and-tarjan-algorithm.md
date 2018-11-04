---
title: Connectivity & Tarjan Algorithm
date: 2018-09-27 22:51:21
categories:
- competitive programming
tags:
- algorithm
- graph theory
- dfs
- connectivity
---
Well, I think **Tarjan** algorithm is quite basic in graph theory. It's only an simple application of **dfs**. Inspite of this, I usually refer to the internet or past code of it when I need to use it. So I try to figure out how it works in this blog and hope that I can learn it by heart.

As we all know, the **Tarjan** algorithm enables us to find all the **articulation** points in $O(|V| + |E|)$ time complexity. Actually it's famous for find strongly connected components in DAG(directional acyclic graph). But here we only focus on using it to find articulation points in undirectional graph.

Articulation point, also called cut vertex, is any vertex whose removal increases the number of components.

Then we talk about how **Tarjan** algorithm works. It actually runs a depth-first-search and during the process it assigns two numbers to each vertex. Let's call them **dfn** and **low**. **dfn** is the order in which the vertex is visited during the whole dfs process. And **low** is the smallest **dfn** of any vertex that can be visited by current vertex in the depth-first-search tree. So when we found two vertices where $low[v] \ge dfn[u]$, that means we have to visit `u` before reaching `v` in the dfs tree. As a result, `u` is a articulation point. But be careful that there is a special case: `u` is the root vertex in the dfs tree. Then for any vertex v the above condition holds. A root would be a cut vertex when it has more than one sub trees in dfs tree. Since its sub trees are not connected apparently, cutting the root will make the sub trees seperated. So this is the main idea.

Now let's see how to calculate **dfn** and **low** and implement the algorithm. **dfn** is quite easy to get, we just maintain a counter and increase it by one in each step of the dfs and assign it to the current vertex.
For **low**, there are three cases:
1.    $low[u] = dfn[u]$, this is the base case.
2.    $low[u] = dfn[v]$, this happens when Edge(u,v) is a back edge.
3.    $low[u] = low[v]$, this happens when Edge(u,v) is a forward edge.

Here is the code:
{% codeblock lang:cpp %}
const int N=505,M=50000;
struct edge {
    int to,next;
} e[M];
int head[N],tot;
void add_edge(int u,int v) {
    e[tot].to=v;
    e[tot].next=head[u];
    head[u]=tot++;
}
int counter=0;
int dfn[N],low[N];
bool found=false;
int n,m;

void dfs(int u,int pa) {
    dfn[u]=low[u]=++counter;
    int son=0;
    for (int i=head[u];i!=-1;i=e[i].next) {
        int to=e[i].to;
        if (!dfn[to]) {
            dfs(to,u);
            if (pa!=-1&&dfn[u]<=low[to]) {
                found=true;
            }
            low[u]=min(low[u],low[to]);
            son++;
        } else if (to!=pa) {
            low[u]=min(low[u],dfn[to]);
        }
    }
    if (pa==-1&&son>1) {
        found=true;
    }
}
{% endcodeblock %}
It's just a simple dfs. When `found` variable is set to true, we get a cut vertex.

A graph that doesn't contain any cut vertex is called **2-connected**. Let's go a little further. A **3-connected** graph is such a graph that, after we remove any vertex from it, it's still **2-connected**. Then how to tell if a graph is **3-connected**? To check if a graph is 2-connected, we can simply run the **Tarjan** algorithm. If we find cut vertex, then it is not 2-connected. So for checking the property of **3-connected**, we can enumerate the vertices and remove it, and then run the **Tarjan** algorithm to find a cut vertex. If after the whole process, we can still not find a cut vertex, then the graph is **3-connected**.
Here is a sample question: [POJ 3713 - Transferring Sylla](http://poj.org/problem?id=3713)
And this is my code:
{% codeblock lang:cpp %}
#include <stdio.h>
#include <iostream>
#include <algorithm>
#include <string>
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

const int N=505,M=50000;
struct edge {
    int to,next;
} e[M];
int head[N],tot;
void add_edge(int u,int v) {
    e[tot].to=v;
    e[tot].next=head[u];
    head[u]=tot++;
}
int counter=0;
int dfn[N],low[N];
bool found=false;
int a[M],b[M],n,m;

void dfs(int u,int pa) {
    dfn[u]=low[u]=++counter;
    int son=0;
    for (int i=head[u];i!=-1;i=e[i].next) {
        int to=e[i].to;
        if (!dfn[to]) {
            dfs(to,u);
            if (pa!=-1&&dfn[u]<=low[to]) {
                found=true;
            }
            low[u]=min(low[u],low[to]);
            son++;
        } else if (to!=pa) {
            low[u]=min(low[u],dfn[to]);
        }
    }
    if (pa==-1&&son>1) {
        found=true;
    }
}

bool three_conn() {
    for (int i=0;i<n;i++) {
        memset(head,-1,sizeof(head));
        tot=0;
        found=false;
        counter=0;
        for (int j=0;j<m;j++) {
            if (a[j]!=i&&b[j]!=i) {
                add_edge(a[j],b[j]);
                add_edge(b[j],a[j]);
            }
        }
        memset(dfn,0,sizeof(dfn));
        memset(low,0,sizeof(low));
        if (i==0) {
            dfs(1,-1);
        } else {
            dfs(0,-1);
        }
        if (counter!=n-1||found) return false;
    }
    return true;
}

int main()
{
    while (true) {
        scanf("%d%d",&n,&m);
        if (n==0&&m==0) break;
        for (int i=0;i<m;i++) {
            scanf("%d%d",a+i,b+i);
        }
        if (three_conn()) printf("YES\n");
        else printf("NO\n");
    }
}
{% endcodeblock %}
