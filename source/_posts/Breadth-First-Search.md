---
title: Breadth First Search
date: 2018-03-07 23:18:11
thumbnail: http://hucoco.com/img/Graph/DGBFS.png
tags: 
- C++
- Algorithm
- Graph
categories:
- Algorithm
- Search
---

[Breadth First Search Chinese version address](http://hucoco.coding.me/2016/11/22/Graph/BFS/)

# Breadth First Search

## Overview

The breadth first search algorithm, also known as "width first search" or "horizontal priority search", is called BFS.

From a vertex in the graph, each of the not visited adjacent points of the V is accessed in turn after accessing the starting vertex, then the adjacency points are accessed in turn from these adjacent points, and the adjacent points of the vertex to be accessed are accessed before the adjacent points of the vertices that are accessed, until all the adjacent points of the vertices are accessed. If the vertices are not accessed at this time, we need to select another vertex which has never been visited as a new starting point, repeat the above process until all the vertices in the graph are accessed.

In other words, the process of breadth first search is the starting point and from near to far, access to the path and the path length of the starting vertex and the path length of 1,2...

## Diagram

### Breadth first search for undirected graphs

The following is an example of undirected graph, to demonstrate the depth-first search.

![Undirected Graphs](http://hucoco.com/img/Graph/UDG.png)

<!--more-->

Breadth first search of the graph above, starting with vertex A:

![Step Diagram](http://hucoco.com/img/Graph/UDGBFS.png)

#### Steps:

1. Access to A.
2. Access C, D, F in turn. After visiting A, next visit A's adjoining point. the vertices A,B,C,D,E,F,G are stored in order, so first visit C, after visiting C, next to visit D and F.
3. Access B, G, in turn. After C, D, and F are accessed in the second step, then their adjacency points are accessed in turn. Next first access the adjacency point B of C, and then access the adjacency point G of the F.
4. Access to E. After accessing B and G in the third step, the adjacent points are accessed in turn. Only G has a adjacency point E, so access to the adjacency point E of the G.

#### C++ Implememtation

##### Adjacency Matrix Version

```
// Work in progress 
```

##### Adjacency List Version

```
// Work in progress 
```


### Breadth first search for undirected graphs

The following is an example of directed graph, to demonstrate the Breadth first search.

![Directed Graphs](http://hucoco.com/img/Graph/DG.png)

<!--more-->

Breadth first search of the graph above, starting with vertex A:

![Step Diagram](http://hucoco.com/img/Graph/DGBFS.png)

#### Steps:

1. Access to A.
2. Access to B.
3. Access C, E, F in turn. After accessing the B, next to the other vertex of the B's out-edge, that is, C, E, and F.
4. Access D, G in turn. After accessing C, E, and F, the other vertices of the out-edge of their are accessed in turn.

#### C++ Implememtation

##### Adjacency Matrix Version

```
// Work in progress 
```

##### Adjacency List Version

```
// Work in progress 
```