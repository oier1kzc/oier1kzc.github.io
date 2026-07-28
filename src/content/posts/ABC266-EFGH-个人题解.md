---
title: "ABC266 EFGH 个人题解"
published: 2023-01-31T03:49:37.000Z
updated: 2023-02-10T15:12:40.000Z
description: "E 对我来说还是很有难度。"
image: ""
tags: ["AtCoder","ABC","DP","期望","计数","分治"]
category: "题解"
draft: false
---
[$\text{AtCoder Beginner Contest 266}$](https://atcoder.jp/contests/abc266)

E 对我来说还是很有难度。

### $\text{Date 2022/08/29}$

# [E. Throwing the Die](https://atcoder.jp/contests/abc266/tasks/abc266_e)

## Statement

有一个 $m$ 面的骰子（本题中 $m = 6$）。

你需要玩一个游戏。

这个游戏最多有 $n$ 轮，对于每一轮

- 扔出骰子，得到一个在 $[1, m]$ 中均匀随机的整数。设该数为 $x$。
- 如果这是第 $n$ 轮，那么游戏结束，你的得分是 $x$。
- 否则，你可以选择是否结束游戏。
- 若结束游戏，那么你的得分是 $x$。

求你在最优策略下的得分期望。

## Solution

执行 $k$ 轮最优的期望得分为 $f _ k$。

转移即考虑**在前面**加一轮，枚举该轮得分，和 $f _ k$ 取 $\max$，转移到 $f _ {k + 1}$。

有 $\displaystyle f _ {k + 1} = \frac 1 m \sum _ {i = 1} ^ m \max(i, f _ k)$。

时间复杂度 $O(nm)$。

# [F. Well-defined Path Queries on a Namori](https://atcoder.jp/contests/abc266/tasks/abc266_f)

## Statement

给定一个 $n$ 个点 $n$ 条边的简单连通无向图。

有 $Q$ 次询问，每次给出 $x, y$，问 $x$ 到 $y$ 的简单路径是否唯一。

简单路径的定义是不经过重复点的路径。

## Solution

图是基环树。

将所有度数为 $1$ 的点拿出来拓扑排序，可以得到最后的环。

将这个环拆掉，每次询问的两个点路径唯一当且仅当在一个连通块里。

# [G. Yet Another RGB Sequence](https://atcoder.jp/contests/abc266/tasks/abc266_g)

## Statement

给定非负整数 $r, g, b, k \ ( k \leq \min(r, g))$，求有多少由 `R`、`G`、`B` 构成的字符串 $s$ 满足如下条件

- `R` 的出现次数为 $r$，`G` 的出现次数为 $g$，`B` 的出现次数为 $b$。
- 连续子串 `RG` 出现 $k$ 次。

答案对 $998244353$ 取模。

## Solution

考虑将连续子串 `RG` 打包起来，看成新的字符 `A`。

对原问题进行转化

- 给定非负整数 $r, g, b, k$，求有多少由 `R`、`G`、`B`、`A` 组成的字符串 $s$ 满足如下条件
  - `R` 的出现次数为 $r - k$。
  - `G` 的出现次数为 $g - k$。
  - `B` 的出现次数为 $b$。
  - `A` 的出现次数为 $k$。
  - $s$ 中不包含连续子串 `RG`。

将额外出现的 `RG` 再打包看成字符 `C`，容斥即可。

# [H. Snuke Panic (2D)](https://atcoder.jp/contests/abc266/tasks/abc266_h)

## Statement

有 $n$ 个物品要掉落到一个二维平面上。

第 $i$ 个物品将在第 $t _ i$ 秒掉在 $(x _ i, y _ i)$ 的位置，其价值为 $a _ i$。

你的初始坐标为 $(0, 0)$，时间为第 $0$ 秒。

每秒你可以往上、左、右移动一个单位，也可以不动。

具体地，如果第 $t$ 秒你处于 $(x, y)$，则在第 $t + 1$ 秒，你可以挪动到 $(x, y + 1), (x - 1, y), (x + 1, y), (x, y)$ 这四个中的一个位置。

如果你在 $t _ i$ 秒恰好出现在 $(x _ i, y _ i)$ 这个位置，你就可以拾得第 $i$ 个物品。

给出 $t _ i, x _ i, y _ i, a _ i \ (1 \leq i \leq n)$，求你能拾得的物品价值和的最大值。

$0 \leq t _ i, |x _ i|, y _ i, a _ i \leq 10 ^ 9$。

## Solution

设 $f[t][x][y]$ 表示你走了 $t$ 秒，到达了 $(x, y)$ 的位置，拾得物品价值和的最大值。

考虑**枚举前面所有状态**进行转移

- $f[t][x][y] = \max \bigg \{f[t ^ \prime] [x ^ \prime] [y ^ \prime] {\ \bigg |} \left \{ \begin {aligned} & y ^ \prime \leq y \\ & |x - x ^ \prime| + y - y ^ \prime \leq t - t ^ \prime \end {aligned} \right. \bigg \} + a(t, x, y)$
- 其中 $a(t, x, y)$ 表示在第 $t$ 秒落到 $(x, y)$ 的所有物品价值和。

中间的转移条件太复杂，导致转移过程难以优化。因此考虑简化转移条件

- $\left \{ \begin {aligned} & y ^ \prime \leq y \\ & |x - x ^ \prime| + y - y ^ \prime \leq t - t ^ \prime \end {aligned} \right.$
- $\Rightarrow \left \{ \begin {aligned} & y ^ \prime \leq y \\ & x - x ^ \prime + y - y ^ \prime \leq t - t ^ \prime \\ & x ^ \prime - x + y - y ^ \prime \leq t - t ^ \prime \end {aligned} \right.$
- $\Rightarrow \left \{ \begin {aligned} & y ^ \prime \leq y \\ & t ^ \prime - x ^ \prime - y ^ \prime \leq t - x - y \\ & t ^ \prime + x ^ \prime - y ^ \prime \leq t + x - y \end {aligned} \right.$

令 $b _ i = t _i - x _ i - y _ i, \ c _ i = t _ i + x _ i - y _ i$，令 $g[y][t - x - y][t + x - y] = f[t][x][y]$ 转移变为

- $g[y][b][c] = \max \bigg \{g[y ^ \prime] [b ^ \prime] [c ^ \prime] {\ \bigg |} \left \{ \begin {aligned} & y ^ \prime \leq y \\ & b ^ \prime \leq b \\ & c ^ \prime \leq c \ \ \end {aligned} \right. \bigg \} + a(y, b, c)$

只需计算出在所有 $a(y, b, c)$ 有值的位置的 $g$ 值。

该过程是一个三维数点，可以用二维树状数组、二维线段树、树状数组套线段树、分治等算法在 $O(n \log ^ 2 n)$ 的时间内计算。
