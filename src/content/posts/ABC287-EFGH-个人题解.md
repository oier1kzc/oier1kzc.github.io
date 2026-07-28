---
title: "ABC287 EFGH 个人题解"
published: 2023-02-07T03:50:17.000Z
description: "不会 Floyd。"
image: ""
tags: ["AtCoder","ABC","计数","数学","组合","Burnside 引理"]
category: "题解"
draft: false
---
[$\text{AtCoder Beginner Contest 287}$](https://atcoder.jp/contests/abc287)

不会 Floyd。

**$\text{Date 2023/02/07}$**

# [E. Karuta](https://atcoder.jp/contests/abc287/tasks/abc287_e)

## Statement

给定正整数 $n \ (2 \leq n \leq 5 \cdot 10 ^ 5)$。

给定 $n$ 个字符串 $s _ 1, s _ 2, \cdots, s _ n (\Sigma = \{\texttt a, \cdots, \texttt z\}, \sum |s _ i| \leq 5 \cdot 10 ^ 5)$。

对于 $i = 1, 2, \cdots, n$，求出

- $\max _ {j \neq i} \text{LCP} (s _ i, s _ j)$

其中 $\text{LCP}(x, y)$ 的定义是最大的整数 $n$，满足

- $x$ 和 $y$ 的长度都至少为 $n$。
- 对于所有 $i = 1, 2, \cdots, n$，$x$ 和 $y$ 的第 $i$ 个字符相同。

## Solution

考虑字符串 $s _ 1, \cdots, s _ n (n \geq 2)$，假设它们前 $k$ 个字符都是相同的，可以根据第 $k + 1$ 个字符将它们分为 $|\Sigma|$ 类。在分类时，如果某个集合只含一个字符串，那么就知道了它的答案。否则对于剩下部分，递归。

时间复杂度 $O((n + \sum |s _ i|) |\Sigma|)$。

## Reflection

也可以用字符串哈希等算法。

# [F. Components](https://atcoder.jp/contests/abc287/tasks/abc287_f)

## Statement

给定一个 $n \ (1 \leq n \leq 5000)$ 个点的树。

对于 $x = 1, 2, \cdots, n$，求出

- 对于点集 $V$ 的 $2 ^ n - 1$ 个非空子集，求导出子图有恰好 $x$ 个连通块的子集数量，答案对 $998244353$ 取模。

点集 $S$ 的导出子图定义是 $G = (V ^ \prime, E ^ \prime)$，其中 $V ^ \prime = S$，$E ^ \prime = \{(x, y) \mid (x, y) \in E; x, y \in S\}$。

## Solution

树形 DP，设 $f _ {u, k}$ 表示考虑点 $u$ 的子树，构成 $k$ 个连通块，且包含 $u$ 的子集数量；$g _ {u, k}$ 表示考虑点 $u$ 的子树，构成 $k$ 个连通块，且不含 $u$ 的子集数量。

转移即以下三种。

- $g ^ \prime _ {u, i + j} \gets g _ {u, i} (f _ {v, j} + g _ {v, j})$；
- $f ^ \prime _ {u, i + j} \gets f _ {u, i} g _ {v, j}$；
- $f ^ \prime _ {u, i + j - 1} \gets f _ {u, i} f _ {v, j}$。

每次合并时间复杂度是两个子树 $sz$ 乘积，这是 $O(n ^ 2)$ 的，考虑每两个点在 $\text{LCA}$ 算一次贡献。

# [G. Balance Update Quer](https://atcoder.jp/contests/abc287/tasks/abc287_g)

## Statement

给定整数 $n, m \ (1 \leq n, m \leq 2 \cdot 10 ^ 5)$。

给定长为 $n$ 的数组 $a _ 1, \cdots, a _ n (0 \leq a _ i \leq 10 ^ 9)$ 和 $b _ 1, \cdots, b _ n (0 \leq b _ i \leq 10 ^ 4)$

有 $n$ 种卡片，第 $i$ 种卡片分数为 $a _ i$，数量为 $b _ i$。

给定 $m$ 次操作，每次操作为以下三种

- `1 x y`，表示将 $a _ x$ 改为 $y$。保证 $1 \leq x \leq n$ 且 $0 \leq y \leq 10 ^ 9$。
- `2 x y`，表示将 $b _ x$ 改为 $y$。保证 $1 \leq x \leq n$ 且 $0 \leq y \leq 10 ^ 4$。
- `3 x`，表示一次查询。从所有卡片中选出 $x$ 张卡片，求所有卡片分数总和的最大值。若卡片总数不够 $x$，输出 `-1`。保证 $1 \leq x \leq 10 ^ 9$。

## Solution

每次查询即进行体积为 $1$ 的多重背包。

按照 $a$ 为顺序，用平衡树维护，每个点上维护子树内 $a \times b$ 的和。

每次操作将前 $x$ 个卡片拆出来。查根。

时间复杂度 $O(n \log n)$。

另，在离散化后可以用树状数组做到 $O(n \log n)$。

# [Ex. Directed Graph and Query](https://atcoder.jp/contests/abc287/tasks/abc287_h)

## Statement

给定 $n \ (2 \leq n \leq 2000)$ 个点 $m \ (0 \leq m \leq n (n - 1))$ 条边的有向图。

一条路径的代价定义为，其中所有点的标号的最大值，包含起点和终点。

给定 $Q \ (1 \leq Q \leq 10 ^ 4)$ 次询问，第 $i$ 次询问给定 $s _ i, t _ i$，求 $s _ i$ 到 $t _ i$ 的所有路径的最小代价。

## Solution

考虑 Floyd！！！第 $k$ 次循环可以求只经过前 $k$ 个点的信息！！！！！！！！！

考虑 Floyd，维护数组 $g _ {u, v}$ 表示是否存在从 $u$ 到 $v$ 的路径。

每次内部循环结束后，枚举所有询问 $(s _ i, t _ i)$，若 $s _ i, t _ i \leq k$ 且 $g[s _ i][t _ i]$ 为 $1$，则其答案不超过 $k$。

在第一次枚举到时记录即可。

Floyd 中 $g$ 的更新可以用 `bitset` 优化。

时间复杂度 $O \left( \cfrac {n ^ 3} {\omega} \right)$。

## Reflection

Floyd 懂了但没完全懂。

不要一见到编号 $\leq k$ 这种，就考虑分治，或者动态加点然后用数据结构维护。

思维不够灵活。

固定性的思维在某些题上可能有优势，但是不能被限制。

一种思路做不出来，要学会放弃。
