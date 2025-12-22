---
marp: true
title: Lecture 12 - Graphs, DFS, and BFS
paginate: true
theme: default
backgroundColor: white
author: Gabin An
color: black
size: 16:9
math: mathjax
footer: COSE214
---
![w:100](./images/KU_logo.png)

# Lecture 12 - Graphs, DFS, and BFS

_Fall 2025, Korea University_

Instructor: Gabin An ([gabin_an@korea.ac.kr](mailto:gabin_an@korea.ac.kr))


---

![center w:600](images/hapiness.png)

---

# Midterm Exam

![alt text](images/midterm-distribution.png)
- Average: 61 (std: 20.5)
- Median: 59
- Please come see me after class if you’d like to check your exam paper.


---

## Course Outline (After Midterm)

- Part 3: Data Structures - Continued
   - **Graphs, Graph Search and Applications** 👈
- Part 4: Dynamic Programming
   - Shortest-Path: Dijkstra, Bellman-Ford, Floyd-Warshall Algorithms
   - More General DP: Longest Common Subsequence, Knapsack Problem
- Part 5: Greedy Algorithms and Others
   - Scheduling Problem, Optimal Codes
   - Minimum Spanning Trees
   - Max Flow, Min Cut and Ford-Fulkerson Algorithms
   - Stable Matching, Gale-Shapley Algorithm


---

# Today's Objectives

- Learn the basics of **graphs**, including terminology like **vertices**, **edges**, and types of graphs.
- Understand different methods for representing graphs in memory: Adjacency Matrix and Adjacency List.
- Explore **Depth First Search (DFS)**
- Explore **Breadth First Search (BFS)**
- Compare and contrast DFS and BFS to understand their core differences.

---


# What are Graphs?

* A graph $G = (V, E)$ is a set of vertices (nodes) $V$ and edges $E$ connecting them.
* **$n$** = number of vertices ($|V|$), **$m$** = number of edges ($|E|$).
* **Directed vs. Undirected:**
    * **Undirected:** edges are bidirectional. $(i, j) \in E \iff (j, i) \in E$.
    * **Directed:** edges have a direction.
* **Connected Graph:** A path exists between any two nodes.
* **Sparse vs. Dense:**
    * **Sparse:** Few edges, e.g., $m = \Theta(n)$.
    * **Dense:** Many edges, e.g., $m = \Theta(n^2)$.


---

## Example: Directed Graph w/ Six Nodes and Six Edges

![w:500 center](images/graph-example.png)


---

# Degree of a Node

- The number of edges connected to that node.
- For undirected graphs:
  - $deg(u) = \text{number of edges incident to }u$
- For directed graphs:
  Each node has two types of degrees:
  - In-degree: number of incoming edges (edges ending at the node)
  - Out-degree: number of outgoing edges (edges starting from the node)


---

## Representing Graphs

![w:500 center](images/graph-store.png)

* How to store a graph in memory? Two main methods:
    1.  **Adjacency Matrix**
    2.  **Adjacency List**


---

### Representing Graphs - 1. Adjacency Matrix

* An $n \times n$ binary matrix.
* Matrix element $(i, j)$ is **1** if edge $(i, j)$ exists, **0** otherwise.

![w:700 center](images/graph-adjacency-matrix.png)


---

### Representing Graphs - 1. Adjacency Matrix - Continued

![w:700 center](images/graph-adjacency-matrix.png)

* **Space:** $\Theta(n^2)$ (why? $n \times n$).
* **Edge Lookup:** $O(1)$ time to check if an edge exists.
* **Neighbor Enumeration:** $\Theta(n)$ time.



---

### Representing Graphs - 2. Adjacency List

* An array $A$ of $n$ lists.
* $A[u]$ contains a list of all vertices $v$ where $(u, v) \in E$.

![w:800 center](images/graph-adjacency-list.png)


---

### Representing Graphs - 2. Adjacency List - Continued

![w:800 center](images/graph-adjacency-list.png)

* **Space:** $\Theta(n + m)$.
* **Edge Lookup:** $O(\text{deg}(u))$ time.
* **Neighbor Enumeration:** $O(\text{deg}(u))$ time.





---

## Representing Graphs - Summary

| Representation       | Space Complexity | Check if edge $(u,v)$ exists      | List all neighbors of u  | Notes                                     |
| -------------------- | ---------------- | --------------------------------- | -------------------------| ----------------------------------------- |
| **Adjacency Matrix** | $\Theta(n^2)$    | $\Theta(1)$                       | $\Theta(n)$                   | Fast edge lookup but uses lots of memory. |
| **Adjacency List**   | $\Theta(n + m)$  | $O(deg(u))$ (≈ $O(V)$ worst case) | $\Theta(deg(u))$              | Efficient for sparse graphs.              |


---

# Graph Traversal Algorithms

-  Depth-First Search (DFS)
-  Breadth-First Search (BFS)


---

## Depth First Search (DFS)

* **Strategy:** Explores a path **as far as possible** before backtracking.
* **DFS Augmentations (to prevent loops):**
    * **Colors:**
        * **White:** Unvisited.
        * **Gray:** Visited, but not all neighbors explored.
        * **Green :** Completely processed.
    * **Parent Pointers:** Keeps track of the traversal path.
    * **Timestamps:**
        * **Discovery time ($d(v)$):** When a node is first visited.
        * **Finish time ($f(v)$):** When a node is completely processed.


---

## DFS Pseudocode

- Initially, every node is marked as unvisited (`white`).
    ```
    Algorithm DFS(s, t):
        color(s) ← gray
        d(s) ← t
        t ← t + 1
        foreach v in N(s) do
            if color(v) == white then
                p(v) ← s
                t ← DFS(v, t)
                t ← t + 1
        f(s) ← t
        color(s) ← green
        return f(s)
    ```

---

## DFS Example

![w:500 center](images/dfs-example-1.png)

---

- Start at a white node `a`.

![w:500 center](images/dfs-example-2.png)

---

- From node `a`, we will visit all of `a`’s children, namely node `b`.

![w:500 center](images/dfs-example-3.png)

---

- We now visit `b`’s child, node `c`.

![w:500 center](images/dfs-example-4.png)

---

- We will next search `c`’s second child, node `d`, as another child, node `a`, has already been visited (i.e., colored gray).

![w:500 center](images/dfs-example-5.png)

---

- Since node `d` has no children, we return back to its parent node, `c`, and continue to go back up the path we took, marking nodes with a finish time when we have searched all of their children.

![w:500 center](images/dfs-example-6.png)

---

![w:500 center](images/dfs-example-7.png)

---

![w:500 center](images/dfs-example-8.png)

---

![w:500 center](images/dfs-example-9.png)

---

- We start with a new source node `e` and run DFS to completion.

![w:500 center](images/dfs-example-10.png)

---

## DFS Runtime

* **Runtime:** $O(\text{\# of node visits} + \text{\# of edge scans})$.
* Each node is visited at most once, so node visits are $O(n)$.
* Each edge is scanned at most twice (or once for directed graphs), so edge scans are $O(m)$.
* **Total Runtime:** $O(m+n)$.


---

## DFS and Stack

![center](images/dfs-stack.png)

---

## Breadth First Search (BFS)

* **Strategy:** Expands the search frontier uniformly, visiting all nodes at distance $k$ before moving to nodes at distance $k+1$.
* **Data Structure:** A **queue** (First-In, First-Out) is used to explore nodes in order of their distance from the source.
* **Level Sets ($L_i$):** BFS computes sets $L_i$ containing all nodes at distance $i$ from the source.


---

## Level Sets?

![w:400 center](images/level-set.png)

---

## BFS Pseudocode

```
Algorithm BFS(s):
    vis[v] ← false for all v
    L_0 ← {s}
    vis[s] ← true
    for i = 0...n-1 do
        if L_i is empty then exit
        while L_i is not empty do
            u ← L_i.pop()
            foreach x in N(u) do 
                if vis[x] == false then
                    vis[x] ← true
                    L_{i+1}.insert(x)
                    p(x) ← u
```


---

## BFS Example

![w:500 center](images/bfs-example-1.png)

---

- We begin with the first level set, $L_0$, which contains only the source node `a`.

![w:500 center](images/bfs-example-2.png)

---

- Next, we explore the second level set, $L_1$, consisting of nodes directly reachable from $L_0$.

![w:500 center](images/bfs-example-3.png)

---

- We then move on to the third level set, $L_2$, which contains nodes directly reachable from $L_1$.

![w:500 center](images/bfs-example-4.png)

---

- Finally, we reach the last level set, $L_4$. With no further children to explore, the search terminates here.

![w:500 center](images/bfs-example-5.png)

---

## BFS Runtime

* **Runtime:** $O(\text{\# nodes visited} + \text{\# edges scanned})$.
* Each node is visited once, so node visits are $O(n)$.
* Each edge is scanned once, so edge scans are $O(m)$.
* **Total Runtime:** $O(m+n)$.


---

## BFS and Queue

![center](images/bfs-queue.png)


---

# BFS vs. DFS Simplified

* **DFS:** Explore as deep as possible before backtracking
  * Uses a **stack** to go as far down a path as possible before backtracking. It's like exploring a single winding tunnel to its end before coming back.

* **BFS:** Explore all neighbors level by level
  * Uses a **queue** to explore nodes closest to the source first. It's like ripples spreading out in a pond.
    ![center w:300](images/pond.png)


---

# BFS vs. DFS Simplified


![w:800 center](images/dfsbfs.jpg)

---

# Credits & Resources

Lecture materials adapted from:
- Stanford CS161 slides and lecture notes
  - https://stanford-cs161.github.io/winter2025/
- _Algorithms Illuminated_ by Tim Roughgarden
  - https://algorithmsilluminated.com/

<style>
  img[alt~='center'] {
    display: block;
    margin-left: auto;
    margin-right: auto;
  }
</style>