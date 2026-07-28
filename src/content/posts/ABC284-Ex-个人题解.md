---
title: "ABC284 Ex 个人题解"
published: 2023-02-03T03:49:42.000Z
updated: 2023-02-07T04:07:53.000Z
description: "有色图再放送。"
image: ""
tags: ["AtCoder","ABC","计数","数学","组合","Burnside 引理"]
category: "题解"
draft: false
---
[$\text{AtCoder Beginner Contest 284}$](https://atcoder.jp/contests/abc284)

有色图再放送。

**$\text{Date 2023/02/03}$**

# [Ex. Count Unlabeled Graphs](https://atcoder.jp/contests/abc284/tasks/abc284_h)

有色图再放送。

## Statement

给定正整数 $n, m, p\ (1 \leq m \leq n\leq 30, 10 ^ 8 \leq p \leq 10 ^ 9, p \text{ is a prime})$。

考虑由如下过程生成一张图

- 生成任意一张 $n$ 个点的无标号简单无向图。
- 对于图的每个顶点，填上 $1$ 到 $m$ 的数字之一。
- 需要保证对于 $1$ 到 $m$ 的每个数字，都至少填在了一个顶点上。

求可以生成出的不同的图个数，对 $p$ 取模。

两张图被认为是不同的，当且仅当存在一组标号，分别将两张图的顶点标为 $1, 2, \cdots, n$，满足

- 对于任意 $x$ 满足 $1 \leq x \leq n$，两张图上点 $x$ 的标号相同。
- 对于任意 $x, y$ 满足 $1 \leq x < y \leq n$，边 $(x, y)$ 在一张图上存在，当且仅当它也在另一张图上存在。

## Solution

根据 $\text{Burnside}$ 引理，答案是

$$
\frac 1 {|G|} \sum _ {g \in G} \chi (g)
$$

其中 $|G|$ 表示所有置换数量，是 $n!$。$\chi (g)$ 表示置换 $g$ 的不动点数量。

考虑枚举置换 $g$，算不动点。考虑将置换 $g$ 拆成若干环的乘积。

因为置换下不动，所以对于一个环，它上面的所有颜色都是相同的。

要将 $m$ 种颜色填入这 $t$ 个环，且保证每种颜色至少出现一次，算方案数。$\text{Editorial}$ 中用容斥解决，其实不用，可以直接算，是 $\displaystyle m! {t \brace m}$。这两种方式的桥是 $\displaystyle {t \brace m} = \frac 1 {m!} \sum _ i \binom m i (-1) ^ {m - i} i ^ t$。

剩下的就只需考虑连边了。将边分成两类，在同一环上的，和在两个不同环之间的。

计算在同一个环上的边。为了方便描述，设环上有 $s$ 个点，依次标号为 $0, 1, \cdots, s - 1$。如果有 $x \leftrightarrow (x + r) \bmod s$ 的边，由于置换后相同，对于所有点都需要连这样的边，所以只需要考虑其中某个点 $x$ 的连边方式，就可以确定其它所有点。并且，如果有 $x \leftrightarrow (x + r) \bmod s$ 的边，那么也有 $(x - r) \bmod s \leftrightarrow x$ 的边，所以对于 $a + b = s$ 的正整数 $a, b$，当 $r = a$ 和 $r = b$ 时连边情况是相同的。所以可以限制 $0 < r \leq \left \lfloor \dfrac s 2 \right \rfloor$。于是，可以得到这部分方案数是 $2 ^ {\left \lfloor s / 2 \right \rfloor}$。

计算在不同环上的边。为了方便描述，设两个环分别为 $A, B$，$A$ 上有 $s$ 个点，标号为 $0, 1, \cdots, (s - 1)$，$B$ 上有 $t$ 个点，标号为 $0, 1, \cdots, (t - 1)$，记 $x \leftrightarrow y$ 表示连接 $A$ 上的点 $x$ 和 $B$ 上的点 $y$ 的边。如果有 $x \leftrightarrow y$ 的边，置换后可以得到，对于任意的 $r$，有 $(x + r) \bmod s \leftrightarrow (y + r) \bmod t$ 的边。于是对于任意 $k \geq 0$，都有 $x \leftrightarrow (y + k \gcd(s, t)) \bmod t$ 的边，一共是 $\dfrac t {\gcd(s, t)}$ 条，所以边的等价类个数是 $\gcd(s, t)$。于是，这部分方案数是 $2 ^ {\gcd(s, t)}$。

将这些方案数乘起来就是置换 $g$ 的不动点数，即 $\displaystyle m! {t \brace m} \cdot \left( \prod _ s 2 ^ {\left \lfloor s / 2 \right\rfloor} \right) \cdot \left( \prod _ {s, t} 2 ^ {\gcd(s, t)} \right)$。

这只与环的个数和每个环的大小有关，也就是如何将 $n$ 个点拆分成若干环上。考虑枚举拆分 $v _ 0, v _ 1, \cdots, v _ c$，满足 $v _ 0 \geq v _ 1 \geq \cdots \geq v _ c$ 且 $\displaystyle \sum v _ i = n$，再计算它对应了多少置换。

先将 $v$ 看成有序的，拆分方案数是 $\displaystyle \binom n {v _ 0, v _ 1, \cdots, v _ c}$。而对于每个环，其内部又有 $v _ i - 1)!$ 种排列方式。两个一乘，是 $\dfrac {n!} {\prod v _ i}$。相同的 $v$ 会有影响，导致算重，要再去掉 $v$ 的顺序。设与 $x$ 相等的 $v$ 有 $\texttt \# _ x$ 个，会算 $\texttt \# _ x!$ 次，所以最后除去 $\displaystyle \prod \texttt \# _ x!$ 就可以了。于是，对于固定的拆分 $v$，其对应的方案数是 $\dfrac {n!} {\prod v _ i \prod \texttt \# _ x!}$。

枚举拆分，分别算对应这些东西，乘起来即可。

## Reflection

时间限制很宽松，不卡常可以 $1s$ 跑 $n \leq 60$。

做完有色图，这个题场上还是不会。遇到两次，算是有缘分了，希望再见时能做出来。

## Code

```cpp
#include <stdio.h>
#include <algorithm>

#define LOG(FMT...) fprintf(stderr, FMT)

using namespace std;

typedef long long LL;

const int N = 32;

int n, m, res, mod;
int v[N], szv;
int fact[N], infact[N], rec[N], wt2[N];
int st2[N][N], gcd[N][N];

void Add(int &x, int y) { if ((x += y) >= mod) x -= mod; }
void Sub(int &x, int y) { if ((x -= y) < 0) x += mod; }
int inv(int x, int k = mod - 2) {
	int r = 1;
	while (k) {
		if (k & 1) r = x * (LL)r % mod;
		x = x * (LL)x % mod;
		k >>= 1;
	}
	return r;
}

void dfs(int s, int last) {
	if (s == 0 && szv >= m) { // 环数 < m 时颜色填不完
		int prod = 1;
		for (int i = 0, j; (j = i) < szv; i = j) {
			while (j < szv && v[i] == v[j]) {
				prod = prod * (LL)rec[v[i]] % mod * wt2[v[i] >> 1] % mod; // 前者是去除拆分贡献，后者是同一环内连边方案数
				++j;
			}
			prod = prod * (LL)infact[j - i] % mod; // 去掉相同值得顺序
		}
    // 根据拆分方案数贡献，这里应该需要乘 n!，这和 Burnside 中除去的 |G| 抵消了
		prod = prod * (LL)st2[szv][m] % mod; // 环上涂色的方案数，m! 被扔到最后乘了
		for (int i = 0; i < szv; ++i) {
			for (int j = 0; j < i; ++j) {
				prod = prod * (LL)wt2[gcd[v[i]][v[j]]] % mod; // 两个环之间连边方案数
			}
		}
		Add(res, prod);
	}
	for (int k = min(last, s); k; --k) {
		v[szv++] = k;
		dfs(s - k, k);
		--szv;
	}
}

int main() {
	scanf("%d%d%d", &n, &m, &mod);
	*wt2 = 1, *fact = 1, *infact = 1;
	for (int i = 1; i <= n; ++i) {
		rec[i] = inv(i);
		fact[i] = fact[i - 1] * (LL)i % mod;
		infact[i] = infact[i - 1] * (LL)rec[i] % mod;
		wt2[i] = wt2[i - 1] * 2ll % mod;
	}
	st2[0][0] = 1;
	for (int i = 1; i <= n; ++i)
		for (int j = 1; j <= i; ++j)
			st2[i][j] = (st2[i - 1][j - 1] + j * (LL)st2[i - 1][j]) % mod;
	for (int j = 0; j <= n; ++j) gcd[0][j] = j;
	for (int i = 1; i <= n; ++i)
		for (int j = 0; j <= n; ++j)
			gcd[i][j] = (j >= i ? gcd[i][j - i] : gcd[j][i]);
	dfs(n, n);
	res = res * (LL)fact[m] % mod;
	printf("%d\n", res);
	return 0;
}
```
