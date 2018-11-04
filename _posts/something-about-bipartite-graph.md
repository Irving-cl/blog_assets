---
title: Something About Bipartite Graph
date: 2018-11-04 07:53:32
categories:
- competitive programming
tags:
- algorithm
- graph theory
- bipartite graph
---

In this article, I will mention some concepts in graph theory. They usually appear in problems related to **bipartite graph**. To solve such problems, we need to know these things and some conclusions. 

Let's denote the graph $ G = (V, E) $, where $V$ is the set of vertices and $E$ is the set of edges in graph $G$. 
1.    Match: A set of edges $ M \subseteq E $, where any two edges in $M$ do not have common endpoints. 
2.    Edge Cover: A set of edges $ F \subseteq E $, where any vertex in $V$ is an endpoint of some edge in $F$. 
3.    Independent Set: A set of vertices $ S \subseteq V $, where any vertices in $S$ are not connected with each other. 
4.    Vertex Cover: A set of vertices $ S \subseteq V $, where each edge in $E$ has at least one endpoint in $S$ .

Now let's see some important conclusions which help us solve problems about bipartite graph. 
**Conclusion (1)** 
$ | Maximum \ Match | + |Minimum \ Edge \ Cover | = |V| $. 
Suppose there are $X$ edges in the Maximum Match, $2X$ vertices are covered by these edges. And Suppose there are $Y$ vertices remain uncovered by any edge. To get the Minimum Edge Cover, we can pick the $X$ edges in the Maximum Match and take extra $Y$ edges which connect all the remain vertices. So $ |Minimum \ Edge \ Cover| = X + Y $, as we defined, $ 2X + Y = |V| $. 
**Conclusion (2)** 
$ | Maximum \ Independent \ Set | + | Minimum \ Vertex \ Cover | = |V| $. 
Let's denote $V_{1}$ as the Maximum Independent Set. $V_{2} = V - V_{1}$. Since there are no edges between any two vertices in $V_{1}$ and such set is maximized, $V_{2}$ covers all the edges and $V_{2}$ is minimized. This means $V_{2}$ is the Minimum Vertex Cover. 
**Conclusion(3)** 
In a bipartite graph, $ |Maximum \ Match| = |Minimum \ Vertex \ Cover| $. 

Actually, to get a **Minimum Vertex Cover** or a **Maximum Independent Set** is a **NP-Hard** problem. But in a bipartite graph, we get can them by using the above conclusions in linear time complexity.

