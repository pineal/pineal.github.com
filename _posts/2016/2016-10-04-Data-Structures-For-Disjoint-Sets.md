---
layout: post
title: Data Structures for Disjoint Sets
categories:
- Data Structure
tags:
- Disjoint Sets
- Union Find
---

# Disjoint Sets

## 基本操作

1. make_set(x)
将一个vertex变成一个disjoint

2. union(x, y)
将包含vertex x 的 set 和 包含vertex y 的 set 并起来

3. find_set(x)
返回一个指针，指向包含这个vertex 的唯一的 set

## 简单的应用

最基本的应用是来确定一个 undirected graph 中的 connected components.

连接components:

```
connected_component(G)
    for each vertex v in G
        make_set(v)
    for each edge(u, v) e in G
        if (find_set(u) != find_set(v))
            union(u, v)
```        

判断两个vertices是否连接在同一component中:

```
same_component(u, v)
    if find_set(u) == find_set(v)
        return true
    else
        return false
```

## 并查集森林 (Disjoint-Set Forest)

除了可以用链表来实现 disjoint-set， disjoint-set forest是一种比链表实现更快的实现。我们将sets表示为rooted trees。 每一个 node 包括一个 vertex，每一棵树代表一个set. 所有的node都指向各自的parent. 在树中那个指向自己的显然就是tree root。这种数据结构高效的原因是用了
以下两种技巧："union by rank", "path compression".

### Union By Rank
为每个node维护一个变量rank。 rank代表这这个node的高度的上限。在并集的过程中，我们依据rank的高低，把低rank的root指向高rank的root。

### Path compression
路径压缩并不改变任何rank，只是把tree中的所有node都指向这颗树的root。这个过程在find_set这个操作中实现。

### 实现
```
make_set(x)
x.p = x
x.rank = 0
```

```
union(x, y)
    link(find_set(x), find_set(y))
```

```
link(x,y)
    if x.rank > y.rank
        y.p = x
    else if x.rank < y.rank
        x.p = y
    else
        x.p = y
        y.rank = y.rank + 1
```

```
find_set(x)
    if x != x.p
        x.p = find_set(x.p)
    return x.p
```

### 时间复杂度
时间复杂度为 O(m alpha(n))
TODO: 分析

## Example



## Detect Cycle in a undirected graph

图的相关数据结构表示和函数：

```cpp
typedef struct Edge_t {
	int src;
	int dest;
} Edge;

typedef struct Graph_t {
	int V, E;
	Edge* edges;
} Graph;

Graph* build_graph(int V, int E) {
	Graph* graph = (Graph*)malloc(sizeof(Graph));
	graph->V = V;
	graph->E = E;
	graph->edges = (Edge*)malloc(E * sizeof(Edge));
	return graph;
}
```

Disjoint-Set的相关数据结构和函数：

```cpp
typedef struct disjoint_set_t {
	int parent;
	int rank;
}Disjoint_Set;

int find_set(Disjoint_Set allsets[], int x) {
	if (allsets[x].parent != x) {
		allsets[x].parent = find_set(allsets, allsets[x].parent);
	}
	return allsets[x].parent;
}

void union_set(Disjoint_Set allsets[], int x, int y) {

	int xroot = find_set(allsets, x);
	int yroot = find_set(allsets, y);

	if (allsets[xroot].rank > allsets[yroot].rank) {
		allsets[yroot].parent = xroot;
	} else if (allsets[xroot].rank < allsets[yroot].rank) {
		allsets[xroot].parent = yroot;
	} else {
		allsets[xroot].parent = yroot;
		allsets[yroot].rank++;
	}
}

bool is_cycle(Graph* graph) {
	int V = graph->V;
	int E = graph->E;

	Disjoint_Set* allsets = (Disjoint_Set*)malloc(V * sizeof(Disjoint_Set));
	for (int v = 0;  v < V; v++) {
		allsets[v].parent = v;
		allsets[v].rank = 0;
	}

	for (int e = 0; e < E; e++) {
		int x = find_set(allsets, graph->edges[e].src);
		int y = find_set(allsets, graph->edges[e].dest);

	if(x == y) {
	return true;
	}
	union_set(allsets, x, y);
	}
	return false;
}
```

main函数测试：

```cpp
int main () {

	int V = 3;
	int E = 3;

	Graph* graph = build_graph(V, E);
	graph->edges[0].src = 0;
	graph->edges[0].dest = 1;
	graph->edges[1].src = 1;
	graph->edges[1].dest = 2;
	graph->edges[2].src = 0;
	graph->edges[2].dest = 2;
	if (is_cycle(graph)) {
		printf("Cycle detected.");
	} else {
		printf("No cycle detected.");
	}
	return 0;
}


```

## LeetCode 305 Number of Islands II
