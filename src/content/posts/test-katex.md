---
title: "test"
published: 2023-01-12T13:52:54.000Z
updated: 2023-01-31T08:36:37.000Z
description: ""
image: ""
tags: ["test"]
category: "test"
draft: false
---
测试 katex 与 mathjax。

## 组合数

$$
\binom n m = \cfrac {n!} {m! (n - m)!} = \cfrac {n ^ {\underline m}} {m!}
$$

递推

$$
\binom {n + 1} {m + 1} = \binom n {m + 1} + \binom n m
$$

将 $\displaystyle \binom n {m + 1}$ 按上述方式使劲展开，

$$
\binom {n + 1} {m + 1} = \sum _ {k \leq n} \binom k m
$$

乘 $(m + 1)!$，

$$
(n + 1) ^ {\underline {m + 1}} = (m + 1) \sum _ {k = 0} ^ n k ^ {\underline {m}}
$$

由阶乘

$$
\binom n m = \binom {n - 1} {m - 1} \cfrac n m
$$

转化

$$
x y = \cfrac {(x + y) ^ 2 - x ^ 2 - y ^ 2} 2 = \binom {x + y} 2 - \binom x 2 - \binom y 2
$$

部分值

$$
\begin {array} {c:c:c:c:c:c:c}
& 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 \cr \hdashline
0 & 1 & & & & & & & & \cr \hdashline
1 & 1 & 1 & & & & & & & \cr \hdashline
2 & 1 & 2 & 1 & & & & & & \cr \hdashline
3 & 1 & 3 & 3 & 1 & & & & & \cr \hdashline
4 & 1 & 4 & 6 & 4 & 1 & & & & \cr \hdashline
5 & 1 & 5 & 10 & 10 & 5 & 1 & & & \cr \hdashline
6 & 1 & 6 & 15 & 20 & 15 & 6 & 1 & & \cr \hdashline
7 & 1 & 7 & 21 & 35 & 35 & 21 & 7 & 1 & \cr \hdashline
8 & 1 & 8 & 28 & 56 & 70 & 56 & 28 & 8 & 1
\end {array}
$$

## 第一类斯特林数

$$
{n \brack m} = {n - 1 \brack m - 1} + (n - 1) {n - 1 \brack m}
$$

懂得不多，不写了。

## 第二类斯特林数

递推

$$
{n \brace m} = {n - 1 \brace m - 1} + m {n - 1 \brace m}
$$

通项公式

$$
{n \brace m} = \cfrac 1 {m !} \sum _ k \binom m k (-1) ^ k (m - k) ^ n
$$

方幂转下降幂

$$
m ^ n = \sum _ k {n \brace k} m ^ {\underline k}
$$
