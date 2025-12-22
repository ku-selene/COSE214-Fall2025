---
marp: true
title: Lecture 15 - Bellman-Ford Algorithm and Dynamic Programming
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

# Lecture 15 - Bellman-Ford Algorithm and Dynamic Programming

_Fall 2025, Korea University_

Instructor: Gabin An ([gabin_an@korea.ac.kr](mailto:gabin_an@korea.ac.kr))


---

# Recap: SSSP and Dijkstra's Algorithm

![w:1000 center](images/dijkstra-3.png)


---

# Recap: Limitation of Dijkstra's Algorithm

- Requires **nonnegative edge weights**
- Not robust to **graph updates** (edge weight changes)

➡ For graphs with negative weights, we use **Bellman-Ford**, which we will learn today!


---


## Example: Negative Edge Weights

<div class="one-one-columns">

<div>

![w:500 center](images/bellman-ford-1.png)


</div>

<div>


</div>

</div>


---

## Example: Negative Edge Weights

<div class="one-one-columns">

<div>

![w:500 center](images/bellman-ford-1.png)


</div>

<div>

<br>
<br>

| target      | shortest path    | total cost |
|:------------|------------------|-----------:|
|         `b` | `a → b`          | -1         |
|         `c` | `a → b → c`      | 2          |
|         `d` | `a → b → d`      | 1          |
|         `e` | `a → b → d → e`  | -2         |

</div>

</div>


---

# Negative Edge Weights and **Negative Cycles**

- Negative weights are fine for Bellman-Ford.  
- However, the presence of a **negative cycle**, a cycle whose total edge weight is negative, causes a problem:
  - Each time we traverse the cycle, the path cost decreases.
  - As a result, the “shortest path” to any node on such a cycle is not well defined (it tends toward negative infinity). *E.g., What is the shortest path from 1 to 2?*

![w:250 center](images/negative_cycle.png)


---

## Example of Negative Cycle

![w:350 center](images/negative_cycle.png)

- `1 → 2`: `1`
- `1 → 2 → 3 → 4 → 2`: `0` 😨
- `1 → 2 → 3 → 4 → 2 → 3 → 4 → 2`: `-1` 😱


> The shortest path would be of infinite length and is not well-defined!


---

## **Bellman-Ford Algorithm**

**Input:** Graph $G=(V,E)$ with possibly **negative weights**, and a source vervex $s$
**Output:** ① Detects **negative cycles** (if reachable from $s$), or ② computes shortest paths $d(s,v)$ for all $v \in V$ (**SSSP**)

![center w:700](images/bellman-ford-2.png)


---

# **Bellman-Ford Algorithm**

The algorithm computes the shortest paths **in a bottom-up manner**:

1. It first computes the shortest paths that use **at most 0 edges**.
2. Then it computes the shortest paths that use **at most 1 edge**.
3. Then **at most 2 edges**, and so on.
4. After the **$i$-th iteration**, we have the shortest paths that use **at most $i$ edges**.

💡 Since any **simple path** can have **at most $|V| - 1$ edges**, after performing **$|V| - 1$ iterations**, all shortest paths are guaranteed to be found.


---

## **Bellman-Ford Algorithm**

1. Initialize $d[s]=0$, others $\infty$.  
2. **Repeat $|V|-1$ times:**
   Relax **every edge** $(u,v) \in E$: $d[v] \gets \min(d[v], d[u]+w(u,v))$
3. Extra pass:
   If any edge $(u,v)$ can still be relaxed, i.e., $d[v] > d[u] + w(u, v)$
   → **negative cycle detected** 🔁.


---

## Example 1: Negative Cycle Does Not Exist

![w:400 center](images/bellman-ford-example-2-1.png)


---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-2.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-3.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-4.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-5.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-6.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-7.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-8.png)

---

## Example 1: Negative Cycle Does Not Exist


![w:600 center](images/bellman-ford-example-2-9.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-10.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-11.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-12.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:600 center](images/bellman-ford-example-2-13.png)

---

## Example 1: Negative Cycle Does Not Exist

![w:400 center](images/bellman-ford-example-2-14.png)

---

## Example 2: Negative Cycle Exists

![w:400 center](images/bellman-ford-example-1-1.png)


---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-2.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-3.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-4.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-5.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-6.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-7.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-8.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-9.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-10.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-11.png)

---

## Example 2: Negative Cycle Exists

![w:600 center](images/bellman-ford-example-1-12.png)

---

## Example 2: Negative Cycle Exists

![w:450 center](images/bellman-ford-example-1-13.png)


---

## Correctness of Bellman-Ford

The Bellman-Ford algorithm guarantees one of two outcomes:

1. If there is a **negative cycle** reachable from the source → it reports “Negative Cycle”.
2. Otherwise, it computes the correct shortest path distances $d(s,v)$ for all $v \in V$.


---

### Claim 1: If there is a **negative cycle** reachable from the source, it reports “Negative Cycle”.


![w:850 center](images/negative_cycle3.png)

- Let the negative cycle be $C = (v_1, \dots, v_k)$ with
  $$
  \sum_{i=1}^k w(v_i, v_{i+1}) < 0, \quad v_{k+1} = v_1
  $$
- Since $C$ is reachable from $s$, there exists a path from $s$ to $v_1$ and to every node on $C$.
- For each node on $C$, there exists a simple path (no cycles) from $s$ with **at most $n-1$ edges**.



---

![w:900 center](images/negative_cycle3.png)

- For the sake of contradiction, suppose Bellman-Ford does not report “Negative Cycle,” even though $C$ exists.
  - After $n-1$ iterations, $d[v_i]$ will be some finite number $< \infty$ for $i=1,\dots,k$.
  - Since no negative cycle was reported (i.e., no edge can be released):
    $$
    d[v_{i+1}] \le d[v_i] + w(v_i, v_{i+1}), \quad i=1,\dots,k
    $$


---

![w:900 center](images/negative_cycle3.png)

- Since no cycle is reported, 
  $$
  d[v_{i+1}] \le d[v_i] + w(v_i, v_{i+1}), \quad i=1,\dots,k
  $$
- Summing over the cycle $C$, we obtain
  $$
  \sum_{i=1}^{k} d[v_{i+1}] \le \sum_{i=1}^{k} d[v_{i}] + \sum_{i=1}^{k} w(v_i, v_{i+1}) \quad \rightarrow \quad 0 \le \sum_{i=1}^{k} w(v_i, v_{i+1})
  $$
  **This contradicts that $C$ is a negative cycle.**
- Therefore, Bellman-Ford must report “Negative Cycle”! ✅


---

## Correctness of Bellman-Ford

The Bellman-Ford algorithm guarantees one of two outcomes:

1. If there is a negative cycle reachable from the source → it reports “Negative Cycle”. ✅
2. Otherwise, it computes the **correct shortest path distances** $d(s,v)$ for all $v \in V$. ⬅️


---

![center w:800](images/bellman-ford-example-3-1.png)

---

![center w:800](images/bellman-ford-example-3-2.png)

---

![center w:800](images/bellman-ford-example-3-3.png)

---

![center w:800](images/bellman-ford-example-3-4.png)

---

![center w:800](images/bellman-ford-example-3-5.png)

---

![center w:800](images/bellman-ford-example-3-6.png)

---

![center w:800](images/bellman-ford-example-4-1.png)

---

![center w:800](images/bellman-ford-example-4-2.png)

---

![center w:800](images/bellman-ford-example-4-3.png)

---

![center w:800](images/bellman-ford-example-4-4.png)

---

![center w:800](images/bellman-ford-example-4-5.png)

---

![center w:800](images/bellman-ford-example-4-6.png)

---

## Correctness Proof Sketch

Regardless of **the order of edge relaxations**, the Bellman–Ford algorithm is guaranteed to find all shortest paths after $|V| - 1$ iterations.

This is because:
- Any simple path in a graph has at most $|V| - 1$ edges.
- Each iteration allows paths that are one edge longer to be considered.
- After $|V| - 1$ iterations, all possible simple paths have been explored.


---

### Claim 2: If $G$ has no negative cycle reachacle from $s$, then Bellman-Ford returns the correct shortest path distances, i.e., $d[v] = d(s, v), \forall v \in V$

**Claim 2.1: Shortest paths have at most $n-1$ edges**
If there is a path from $s \to v$, then the shortest path from $s$ to $v$ must have $\le n-1$ edges.


---

**Claim 2.1: Shortest paths have at most $n-1$ edges**
If there is a path from $s \to v$, then the shortest path from $s$ to $v$ must have $\le n-1$ edges.

**Why?**:
If a shortest path contains a cycle:
- The cycle cannot be negative (otherwise Bellman-Ford detects it - **Claim 1**).
- If the cycle has positive weight, removing it gives a shorter path.
- If the cycle has zero weight, removing it does not change the distance.

Thus, shortest paths are always simple paths (**no cycle!** 🚫). A simple path never revisits a vertex. With $n$ vertices total, such a path can have $\le n-1$ edges. ✅


---

**Claim 2.1: Shortest paths have at most $n-1$ edges** (proved ✅)

Ok.🧐 Now let $d_k(v)$ = distance estimate after $k$ iterations. We claim:

**Claim 2.2: $d_k(v) \le$ the minimum distance of a path from $s$ to $v$ with $\le k$ edges.**

If this is true:
- After $n-1$ iterations: $d[v] = d_{n-1}(v)$
- By Claim 2.1, the shortest path from $s$ to $v$ should have $\le n-1$ edges.
- By Claim 2.2, $d_{n-1}(v) \le$ **the minimum distance of a path from $s$ to $v$ with $\le n-1$ edges**, i.e., $d(s, v)$.
- Combinining above, $d[v] = d_{n-1}(v) \le d(s, v)$.
- Since $d[v] \ge d(s, v)$ (same reasoning as Dijkstra), $d[v] = d(s, v)$ ✅


---

Let's prove **Claim 2.2**

> **$d_{k}(v)$ $\le$ the minimum distance of a path from $s$ to $v$ with $\le k$ edges**

**Base case ($k=0$):** $d_0(s) = 0$, $d_0(v) = \infty$ for $v \neq s$. Matches paths with $\le 0$ edges. ✅

<br>

![w:800 center](images/bellman-ford-example-4-2.png)


---

**Inductive hypothesis:** 
$d_{k-1}(v)$ is at most the minimum distance of a path from $s$ to $v$ with $\le k-1$ edges.

<div class="one-one-columns">
<div>

![w:450 center](images/bellman-ford-example-4-3.png)

</div>

<div>

![w:450 center](images/bellman-ford-example-4-4.png)

</div>
</div>

**Inductive step:**

Consider $v \ne s$. Let $P$ be a shortest path $P: s \to \dots \to u \to v$ with $\le k$ edges.

Let $Q$ be the sub-path of $P$ from $s$ to $u$. Then, $Q$ is a shortest path from $s$ to $u$ with $\le k-1$ edges. By the inductive hypothesis, $d_{k-1}(u) \le$ $Q$'s distance weight.

Relaxing $(u,v)$ in iteration $k$: $d_k(v) \le d_{k-1}(u) + w(u,v) \le \text{w}(Q) + w(u, v) = w(P)$.

Thus, $d_k(v)$ is at most the cost of a shortest path with $\le k$ edges. ✅


---

**Claim 2.1: Shortest paths have at most $n-1$ edges** ✅

Let $d_k(v)$ = distance estimate after $k$ iterations.

**Claim 2.2: $d_k(v) \le$ the minimum distance of a path from $s$ to $v$ with $\le k$ edges.** ✅

- The final distance estimate $d[v]$ is $d_{n-1}(v)$.
- By Claim 2.1, the shortest path from $s$ to $v$ should have $\le n-1$ edges.
- By Claim 2.2, $d_{n-1}(v) \le$ **the minimum distance of a path from $s$ to $v$ with $\le n-1$ edges** = **The shortest distance from $s$ to $v$**
- Combinining above, $d[v] = d_{n-1}(v) \le d(s, v)$.
- Since $d[v] \ge d(s, v)$ (same reasoning as Dijkstra), $d[v] = d(s, v)$ ✅


---

## Runtime of Bellman-Ford Algorithm

- The total runtime of the Bellman-Ford algorithm is $O(mn)$.
- In the first for loop, we repeatedly update the distance estimates $n − 1$ times on all $m$ edges in time $O(mn)$.
- In the second for loop, we go through all $m$ edges to check for negative cycles in time of $O(m)$.


---

# Summary: **Bellman-Ford Algorithm**

The algorithm calculates shortest paths in a bottom-up manner.
- It first calculates the shortest distances which have at-most one edge in the path.
- Then, it calculates the shortest paths with at-most 2 edges, and so on. 
- After the $i$-th iteration, the shortest paths with at-most $i$ edges are calculated.
- There can be maximum $|V| – 1$ edge in any simple path, that is why the loop runs $|V| – 1$ time. 


---

# The Bellman-Ford algorithm is a **Dynamic Programming** algorithm!


---

# **Dynamic Programming (DP)**

- A basic paradigm in algorithm design used to solve problems by relying on **intermediate solutions** to smaller subproblems.
- Core ingredients:
  - **Optimal Substructure**
  - **Overlapping Subproblems**

Used in many classic algorithms, e.g., shortest paths, sequence alignment, knapsack, etc.  


---

# Next Class

- We'll learn the concept of **Dynamic Programming (DP)** and understand why Bellman–Ford is actually a DP algorithm.

🧑‍🏫 So far, we solved the Single-Source Shortest Path (SSSP) problem.

🧐 But what if we want the **shortest paths between every pair of nodes**?

- This is the **All-Pairs Shortest Paths (APSP)** problem!
- We’ll learn the **Floyd–Warshall** algorithm, a dynamic programming approach to solving APSP.


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

  .one-one-columns {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 0.5rem;
  }

  .two-one-columns {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 0.5rem;
  }

  .three-one-columns {
    display: grid;
    grid-template-columns: 3fr 1fr;
    gap: 0.5rem;
  }

  .five-one-columns {
    display: grid;
    grid-template-columns: 5fr 1fr;
    gap: 0.5rem;
  }

</style>