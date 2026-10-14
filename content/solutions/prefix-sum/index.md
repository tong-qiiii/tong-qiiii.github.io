---
title: "示例题解 · 前缀和与区间求和"
description: 用一道基础示例展示题意、推导、正确性证明与 C++17 实现的写法。
date: 2026-10-14T10:00:00+08:00
slug: prefix-sum
categories: [ICPC 题解]
tags: [前缀和, C++, 示例]
math: true
toc: true
---

> 这是一篇用于展示博客格式的示例题解，题目为自拟基础练习，不对应某场 ICPC 比赛。

## 题意

给定长度为 $n$ 的整数数组 $a$，回答 $q$ 次询问。每次给定 $l,r$，求闭区间 $[l,r]$ 的元素和。

约束：$1 \le n,q \le 2 \times 10^5$，$|a_i| \le 10^9$，$1 \le l \le r \le n$。

输入第一行为 $n,q$，第二行为 $n$ 个整数，接下来 $q$ 行每行两个整数 $l,r$。对每次询问输出一行答案。

## 解题思路

如果每次直接遍历区间，总时间最坏达到 $O(nq)$。

定义前缀和 $s_i$ 为前 $i$ 个元素的总和，并令 $s_0=0$：

$$
s_i = s_{i-1} + a_i
$$

两个前缀相减，就能得到区间和：

$$
\sum_{i=l}^{r} a_i = s_r - s_{l-1}
$$

例如数组为 `[2, 3, -1, 4]`，前缀和为 `[0, 2, 5, 4, 8]`。区间 $[2,4]$ 的和为 $s_4-s_1=8-2=6$。

## 正确性证明

由定义，$s_r$ 包含位置 $1$ 到 $r$ 的所有元素，$s_{l-1}$ 包含位置 $1$ 到 $l-1$ 的所有元素。相减后，位置 $1$ 到 $l-1$ 的项恰好抵消，剩下的正是位置 $l$ 到 $r$ 的元素。因此每次询问的结果正确。

## 复杂度分析

- 预处理：$O(n)$。
- 单次询问：$O(1)$。
- 总时间：$O(n+q)$。
- 空间：$O(n)$。

## C++17 实现

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, q;
    cin >> n >> q;
    vector<long long> prefix(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        long long x;
        cin >> x;
        prefix[i] = prefix[i - 1] + x;
    }
    while (q--) {
        int l, r;
        cin >> l >> r;
        cout << prefix[r] - prefix[l - 1] << '\n';
    }
    return 0;
}
```

## 样例

输入：

```text
4 3
2 3 -1 4
2 4
1 1
1 4
```

输出：

```text
6
2
8
```

## 易错点

1. 当 $l=1$ 时，需要访问 $s_0$，因此数组长度应为 $n+1$。
2. 区间和可能超过 32 位整数范围，应使用 `long long`。
3. 前缀和适合数组不变的区间查询；涉及修改时，需要考虑树状数组或线段树。
