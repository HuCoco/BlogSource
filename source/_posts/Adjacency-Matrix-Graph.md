---
title: Adjacency Matrix Graph
date: 2018-03-06 00:40:02
thumbnail: http://hucoco.com/img/Graph/UDG.png
tags: 
- C++
- Data Structure
- Graph
categories:
- Data Structure
- Graph
---

[Adjacency Matrix Graph Chinese version address](http://hucoco.coding.me/2016/11/16/Graph/GraphByMatrix/)

# Adjacency Matrix Graph

## Adjacency Matrix Undirected Graph

Adjacency matrix undirected graph refers to an undirected graph represented by an adjacency matrix.

![Adjacency Matrix Undirected Graph](http://hucoco.com/img/Graph/UDG.png)

The above graph contains 7 vertices of A, B, C, D, E, F, G, and it also contains<A,C>, <A,D>, <A,F>, <B,C>, <C,D>, <E,G>, <F,G>, in total 7 edges. Since this is an undirected graph, the edge <A,C> and the edge <C,A>are the same edges. The table of edges is listed in alphabetical order.

<!--more-->

|N/A|A|B|C|D|E|F|G|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|A|0|0|1|1|0|1|0|
|B|0|0|1|0|0|0|0|
|C|1|1|0|1|0|0|0|
|D|1|0|1|0|0|0|0|
|E|0|0|0|0|0|0|1|
|F|1|0|0|0|0|0|1|
|G|0|0|0|0|1|1|0|

The matrix above is a schematic of the adjacency matrix in memory. A[i][j] = 1 means that from the ith vertex to the jth vertex is a edge. A[i][j] = 0 indicates that they are not adjacent points.

### C++ Definition

```
// Work in progress
```

### C++ Implementation

```
// Work in progress
```

## Adjacency Matrix Directed Graph

Adjacency matrix undirected graph refers to an directed graph represented by an adjacency matrix.

![Adjacency Matrix Directed Graph](http://hucoco.com/img/Graph/DG.png)

The above graph contains 7 vertices of A, B, C, D, E, F, G, and it also contains <A,B>, <B,C>, <B,E>, <B,F>, <C,E>, <D,C>, <E,B>, <E,D>, <F,G>, in total 7 edges.

|N/A|A|B|C|D|E|F|G|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|A|0|0|1|0|0|0|0|
|B|0|0|1|0|1|1|0|
|C|0|0|0|0|1|0|0|
|D|0|0|1|0|0|0|0|
|E|0|1|0|1|0|0|1|
|F|0|0|0|0|0|0|1|
|G|0|0|0|0|0|0|0|

The matrix above is a schematic of the adjacency matrix in memory. A[i][j] = 1 means that the ith vertex and the jth vertex are adjacent points. A[i][j] = 0 indicates that not a edge.

### C++ Definition

```
// Work in progress
```

### C++ Implementation

```
// Work in progress
```