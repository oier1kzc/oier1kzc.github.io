---
title: "ABC283 EFH 个人题解"
published: 2023-01-25T03:49:27.000Z
updated: 2023-02-07T04:06:55.000Z
description: "E 不会做，没想到 DP。"
image: ""
tags: ["AtCoder","ABC","DP","万欧"]
category: "题解"
draft: false
---
[$\text{AtCoder Beginner Contest 283}$](https://atcoder.jp/contests/abc283)

E 不会做，没想到 DP。

**$\text{Date 2023/01/24}$**

# [E. Don’t Isolate Elements](https://atcoder.jp/contests/abc283/tasks/abc283_e)

## Statement

给定 $n, m \ (1 \leq n, m \leq 1000)$，和 $n$ 行 $m$ 列的矩阵 $A _ {n \times m} (0 \leq A _ {i, j } \leq 1)$。

每次操作选择一个 $i \ (1 \leq i \leq n)$，对于所有 $1 \leq j \leq m$，将 $A _ {i, j}$ 修改为 $1 - A _ {i, j}$。

$A _ {i, j}$ 是可爱的，当且仅当存在 $(x, y) \in \{(i - 1, j), (i, j - 1), (i, j + 1), (i + 1, j)\}$，满足 $1 \leq x \leq n, 1 \leq y \leq m$ 且 $A _ {i, j} = A _ {x, y}$。

## Solution

DP！！！！！怎么就不会 DP！！

设 $f _ {i, 0/1, 0/1}$ 表示考虑前 $i$ 行，第 $i - 1$ 行是否翻转，第 $i$ 行是否翻转。

或者 $f _ {i, 0 / 1}$ 表示考虑前 $i$ 行，第 $i$ 行相对于第 $i - 1$ 行是否翻转。

都可以在 $O(m)$ 内简单转移，时间复杂度 $O(nm)$。

## Reflection

怎么就没想到 DP 呢。

# [F. Permutation Distance](https://atcoder.jp/contests/abc283/tasks/abc283_f)

## Statement

给定 $(1, 2, \cdots, n)$ 的排列 $p = (p _ 1, p _ 2, \cdots, p _ n)$。

对于所有 $1 \leq i \leq n$，求出下式的值

- $D _ i = \min \limits _ {j \neq i} \{|p _ i - p _ j| + |i - j|\}$。

## Solution1

先拆第二个绝对值

- $D _ i = \min \left( \min \limits _ {j < i} \{|p _ i - p _ j| + i - j\}, \min \limits _ {i < j} \{|p _ i - p _ j| + j - i\} \right)$。

两部分对称，只需对所有 $i$，算 $\min \limits _ {j < i} \{|p _ i - p _ j| + i - j\}$ 即可。

再拆第一个绝对值，直接固定 $p _ i$ 和 $p _ j$ 的大小关系，过程和上面一样，数据结构维护即可。

时间复杂度 $O(n \log n)$。

## Solution2

考虑如下算法

```cpp
for (int i = 1; i <= n; ++i) {
  int d = n;
  for (int j = i - 1; j && i - j < d; --j) {
    d = min(d, abs(p[i] - p[j]) + i - j);
  }
  for (int j = i + 1; j <= n && j - i < d; ++j) {
    d = min(d, abs(p[i] - p[j]) + j - i);
  }
  printf("%d ", d);
}
```

这是一个优化的暴力，下面说明该算法时间复杂度为 $O(n \sqrt n)$。

对值域分块，设 $1 \leq B \leq n$。对于每个 $1 \leq k \leq \lfloor n / B \rfloor$，

- 设 $l _ k = (k - 1) B + 1, r _ k = k B$。
- 考虑所有满足 $l _ k \leq p _ i \leq r _ k$ 的所有位置 $i$，设这些位置是 $q _ 1 < q _ 2 < \cdots < q _ t$，其中 $t = r _ k - l _ k + 1$。
- 对于每个 $q _ u$，当 $i = q _ u$ 时，$j$ 往左枚举时，不会超过 $q _ {u - 1} - B$，否则有
  - $i - j \geq q _ u - q _ {u - 1} + B \geq q _ u - q _ {u - 1} + |p[q _ u] - p[q _ {u - 1}]| \geq d$，
- 同样地，$j$ 往右枚举，不会超过 $q _ {u + 1} + B$。
- 这里认为 $q _ 0 = 1, q _ {t + 1} = n$，边界也是合法的。
- 于是在 $i = q _ u$ 这里的总枚举次数不超过 $(q _ u - q _ {u - 1} + B) + (q _ {u + 1} - q _ u + B) = q _ {u + 1} - q _ {u - 1} + 2 B$。
- 对于所有 $1 \leq u \leq t$ 累加，
  - $\begin {aligned} \sum _ {u = 1} ^ t q _ {u + 1} - q _ {u - 1} + 2 B = q _ {t + 1} + q _ t - q _ 1 - q _ 0 + 2 t B \leq 2 n + 2 t B = 2 n + 2 (r _ k - l _ k + 1) B\end {aligned}$。

于是对于每个 $1 \leq k \leq \lfloor n / B \rfloor$，总执行次数不超过 $2 n + 2 (r _ k - l _ k + 1) B$。对 $k$ 求和，有

- $\begin {aligned} \sum _ {k = 1} ^ {\lfloor n / B \rfloor} 2 n + 2 (r _ k - l _ k + 1) B = 2 n \left \lfloor \frac n B \right \rfloor + 2 n B \end {aligned}$。

取 $B = \sqrt n + O(1)$，可知执行次数不超过 $4n \sqrt n + O(1)$，于是时间复杂度不超过 $O(n \sqrt n)$。

事实上，也可以卡到 $O(n \sqrt n)$。按照上文证明的方式进行构造，尽量卡满。一组构造如下

- $1, B + 1, 2 B + 1, \cdots, 2, B + 2, 2 B + 2, \cdots, 3, 3 B + 1, 3 B + 2, \cdots$。

其中 $B = \lfloor \sqrt n \rfloor + 1$，上述算法执行次数约为 $2 n \sqrt n$。

目前不确定是否有更紧的上界（严格小于 $4 n \sqrt n$）或更紧的下界（严格大于 $2 n \sqrt n$）。

至此，我们说明了该算法时间复杂度为 $O(n \sqrt n)$。

# [Ex. Popcount Sum](https://atcoder.jp/contests/abc283/tasks/abc283_h)

借这个题学一下万欧。以前不会。

## Statement

给定整数 $n, m, r \ (1 \leq m \leq n \leq 10 ^ 9, 0 \leq r < m)$。

对于所有 $1$ 到 $n$ 中且对 $m$ 取模余 $r$ 的整数，计算 $\text {popcount}$ 的和。

## Solution

拆每位贡献，考虑第 $k$ 位，要算

- $\displaystyle \sum _ {i = 0} ^ {\lfloor (n - r) / m \rfloor} \left( \left \lfloor \frac {m \times i + r} {2 ^ k} \right \rfloor \bmod 2 \right)$

设 $m _ 0 = \left \lfloor \dfrac {n - r} m \right \rfloor$。中间的 $\bmod 2$ 不好处理，用 $x \bmod 2 = x - 2 \left \lfloor \dfrac x 2 \right \rfloor$ 拆开

- $\displaystyle \sum _ {i = 0} ^ {m _ 0} \left( \left \lfloor \frac {m \times i + r} {2 ^ k} \right \rfloor - 2 \left \lfloor \frac {m \times i + r} {2 ^ {k + 1}} \right \rfloor \right) = \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m \times i + r} {2 ^ k} \right \rfloor - 2 \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m \times i + r} {2 ^ {k + 1}} \right \rfloor$

看起来很差分，先对 $k$ 求和，化简一下

- $\displaystyle \sum _ {k \geq 0} \left( \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m \times i + r} {2 ^ k} \right \rfloor - 2 \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m \times i + r} {2 ^ {k + 1}} \right \rfloor \right) = \sum _ {i = 0} ^ {m _ 0} (m \times i + r) - \sum _ {k \geq 1} \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m \times i + r} {2 ^ k} \right \rfloor$

前者是 $(m _ 0 + 1) r + \dfrac {m _ 0 (m _ 0 + 1)} 2 m$。剩下要计算形如下的和式。

- $\displaystyle \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m i + r} q \right \rfloor$

设 $\displaystyle f (m _ 0, m, q, r) = \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m i + r} q \right \rfloor$。

若 $r \geq q$，设 $r = u q + v$，其中 $0 \leq v < q$，则

$$
\begin {aligned}
f(m _ 0, m, q, r) = & \ \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m i + u q + v} q \right \rfloor \\
= & \ \sum _ {i = 0} ^ {m _ 0} \left( u + \left \lfloor \frac {m i + v} q \right \rfloor \right) \\
= & \ (m _ 0 + 1) u + f(m _ 0, m, q, v)
\end {aligned}
$$

转为 $0 \leq r < q$。

若 $m \geq q$，设 $m = u q + v$，其中 $0 \leq v < q$，则

$$
\begin {aligned}
f(m _ 0, m, q, r) = & \ \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {u q i + v i + r} q \right \rfloor \\
= & \ \sum _ {i = 0} ^ {m _ 0} \left( u i + \left \lfloor \frac {v i + r} q \right \rfloor \right) \\
= & \ \frac {m _ 0 (m _ 0 + 1)} 2 u + f(m _ 0, v, q, r)
\end {aligned}
$$

转为 $0 \leq m < q$。

若 $0 \leq m < q$，

$$
\begin {aligned}
f(m _ 0, m, q, r) = \sum _ {i = 0} ^ {m _ 0} \left \lfloor \frac {m i + r} {q} \right \rfloor = \sum _ {k = 1} ^ {\lfloor (m m _ 0 + r) / q \rfloor} \sum _ {i = 0} ^ {m _ 0} \left[ \left \lfloor \frac {m i + r} q \right \rfloor \geq k \right]
\end {aligned}
$$

其中

$$
\begin {aligned}
& \ \left \lfloor \frac {m i + r} q \right \rfloor \geq k \\
\Leftrightarrow & \ \frac {m i + r} q \geq k \\
\Leftrightarrow & \ i \geq \frac {k q - r} m \\
\Leftrightarrow & \ i \geq \left \lceil \frac {k q - r} m \right \rceil = \left \lfloor \frac {k q - r - 1} m \right \rfloor + 1 \\
\end {aligned}
$$

设 $m _ 1 = \left \lfloor \dfrac {m m _ 0 + r} q \right \rfloor$ 于是

$$
\begin {aligned}
f(m _ 0, m, q, r) = & \ \sum _ {k = 1} ^ {m _ 1} \sum _ {i = 0} ^ {m _ 0} \left[ i \geq \left \lfloor \frac {k q - r - 1} m \right \rfloor + 1 \right] \\
= & \ \sum _ {k = 1} ^ {m _ 1} \left( m _ 0 - \left \lfloor \frac {k q - r - 1} m \right \rfloor \right) \\
= & \ m _ 0 m _ 1 - \sum _ {k = 0} ^ {m _ 1 - 1} \left \lfloor \frac {k q + q - r - 1} m \right \rfloor \\
= & \ m _ 0 m _ 1 - f \left( m _ 1 - 1, q, m, q - r - 1 \right)
\end {aligned}
$$

这样就交换了 $q$ 和 $m$。回到 $m \geq q$ 递归执行。

递归过程与 欧几里得算法 相同，因此时间复杂度为 $O(\log m + \log q)$。

结合枚举位，总时间复杂度为 $O(\log ^ 2 m)$。
