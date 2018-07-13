---
title: Max Flow Algorithm
date: 2018-06-30 09:16:14
categories:
- competitive programming
tags:
- algorithm
- max-flow
---

**Maximum** flow is a important part of graph theory in competitive programming. There have been already thousands of articles and blogs teaching people the principle and the implementation of max flow. So I'm not going to elaborate these things again. This blog is just written to help me implement the max flow algorithm faster in a competition with less mistakes.

## Ford-Fulkson
The main idea of max flow algorithm is to find **augmented path** until there is no one. When we find an augmented path, we need to change the residual graph. We should reduce the capcity of each edge on the path and increase it on each reverse edge on it by the flow we found. So in the implementation we don't need to store the current flow of each edge, we just need to store the capcity of them.
{% codeblock lang:cpp %}

const int N=20005,M=2000005;
struct edge {
    int to,next,cap;
} e[N];
int head[N],tot;
void add_edge(int u,int v,int cap) {
    e[tot].to=v;
    e[tot].cap=cap;
    e[tot].next=head[u];
    head[u]=tot++;
}
bool vis[N];

int dfs(int u,int sink,int f) {
    vis[u]=true;
    if (u==sink) return f;
    for (int i=head[u];i!=-1;i=e[i].next) {
        int to=e[i].to;
        if (!vis[to]&&e[i].cap>0) {
            int ff=dfs(to,min(f,e[i].cap));
            if (ff>0) {
                e[i].cap-=cap;
                e[i^1].cap+=cap;
                return ff;
            }
        }
    }
    return 0;
}

int main()
{
    // Get input
    memset(head,-1,sizeof(head));
    // Build graph here
    int max_flow=0;
    while (true) {
        memset(vis,0,sizeof(vis));
        int ff=dfs(src,sink,INF);
        if (ff>0) max_flow+=ff;
        else break;
    }
    printf("max_flow:%d\n",max_flow);
}
{% endcodeblock %}

**Ford-Fulkson** is fast enough in most cases. But sometime it cannot meet our requirements. That's why we need **Dinic**.

## Dinic
Two optimizations are used in **Dinic** which make it faster. One is`level` array and the other is `arc optimization`. I'm not going to elaborate how they work here. Different from **Ford-Fulkson**, it runs a `bfs` to build the `level graph` and then run `dfs` a couple of times(without initializing).
{% codeblock lang:cpp %}

const int N=20005,M=2000005;
struct edge {
    int to,next,cap;
} e[N];
int head[N],tot;
void add_edge(int u,int v,int cap) {
    e[tot].to=v;
    e[tot].cap=cap;
    e[tot].next=head[u];
    head[u]=tot++;
}
int level[N],iter[N];

void bfs(int src) {
    queue<int> q;
    memset(level,-1,sizeof(level));
    level[src]=0;
    q.push(src);
    while (!q.empty()) {
        int cur=q.front();
        q.pop();
        for (int i=head[cur];i!=-1;i=e[i].next) {
            int to=e[i].to;
            if (e[i].cap>0&&level[to]==-1) {
                level[to]=level[cur]+1;
                q.push(to);
            }
        }
    }
}

int dfs(int u,int sink,int f) {
    if (u==sink) return f;
    for (int& i=iter[u];i!=-1;i=e[i].next) {
        int to=e[i].to;
        if (e[i].cap>0&&level[u]<level[to]) {
            int ff=dfs(to,sink,min(f,e[i].cap));
            if (ff>0) {
                e[i].cap-=ff;
                e[i^1].cap+=ff;
                return ff;
            }
        }
    }
    return 0;
}

int main()
{
    // Get input
    memset(head,-1,sizeof(head));
    // Build graph here
    int mx_flow=0;
    while (true) {
        bfs(src);
        if (level[sink]==-1) break;
        for (int i=1;i<=n;i++) iter[i]=head[i];
        int ff;
        while ((ff=dfs(src,sink,INF)>0)) {
            mx_flow+=ff;
        }
    }
    printf("%d\n",mx_flow);
}
{% endcodeblock %}
