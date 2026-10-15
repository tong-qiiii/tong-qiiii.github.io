---
title: "CCPC 2023 深圳 D · Bot Brothers"
description: "用分层叶子集合划分与随机异或哈希，判断后手能否保证到达循环后继叶子。"
date: 2026-10-15T09:10:00+08:00
slug: ccpc-2023-shenzhen-d-bot-brothers
categories: [ICPC 题解]
tags: [CCPC, 博弈论, 树, 随机哈希]
math: true
toc: true
---

## 题意

给定一棵以节点 $n$ 为根的树，其中叶子恰好为编号 $1\sim m$ 的节点。

两名玩家都从根出发，先手 Doodle 和后手 Doddle 轮流行动。每次必须走向当前节点的一个孩子；如果已经到达叶子，则原地等待。

最终，设先手到达叶子 $x$，后手到达叶子 $y$：

- 若 $y=x\bmod m+1$，则后手获胜。
- 若 $x=y\bmod m+1$，则先手获胜。
- 否则平局。

双方采用最优策略，求游戏结果。

## 从直接相连的叶子出发

假设先手位于节点 $u$，下一步选择直接相连的叶子 $a$。此后先手只能等待，后手只要还能到达 $a\bmod m+1$，就能获胜。

因此，后手不一定要和先手同时到达叶子。真正重要的是：**后手是否仍然保留通向对应获胜叶子的路径。**

但当先手还没有选定终点时，后手也必须继续移动，可能因此放弃某些叶子。所以，仅考虑直接相连的叶子还不够，需要考虑整棵子树中的所有叶子。

## 后手必胜的必要条件

后手可以始终模仿先手，最终到达同一个叶子，从而保证平局。因此，答案只可能是 `Doddle` 或 `Tie`。

定义循环后继函数：

$$
\sigma(a)=a\bmod m+1.
$$

记：

$$
S[u]=\{u\text{ 的子树中所有叶子的编号}\},
$$

$$
T[u]=\{\sigma(a)\mid a\in S[u]\}.
$$

将双方各行动一次称为一轮。考虑某轮结束后，先手位于 $u$，后手位于 $v$。

如果 $T[u]\nsubseteq S[v]$，就存在一个叶子 $a\in S[u]$，使得 $\sigma(a)\notin S[v]$。

此时先手一直沿着路径走向 $a$，后手无论如何行动，都无法到达唯一能打败 $a$ 的叶子。

因此，后手想保证获胜，必须在每轮结束后维持：

$$
T[u]\subseteq S[v].
$$

注意，这只是当前状态的必要条件。后手还需要保证先手继续选择分支后，自己仍然有合法的应对。

例如：

```text
        7
       / \
      5   6
     / \ / \
    1  2 3  4
```

初始双方都在根，满足 $T[7]=S[7]$。但先手进入节点 $5$ 后：

- 若后手也进入 $5$，先手可以选择叶子 $2$，后手到不了 $3$。
- 若后手进入 $6$，先手可以选择叶子 $1$，后手到不了 $2$。

所以，只在初始状态满足包含关系，并不能保证后手获胜。

## 从包含关系到集合相等

为了统一处理提前到达叶子的情况，认为叶子在后续每轮仍代表同一个单元素集合。

经过固定的 $d$ 轮后，所有可能位置对应的叶子集合两两不交，并覆盖全部叶子，因此构成一个划分，记为 $\mathcal P_d$。

将其中所有叶子编号循环加一，得到另一个划分 $\sigma(\mathcal P_d)$。因为 $\sigma$ 是一个置换，这两个划分的块数相同。

如果后手存在必胜策略，那么对先手的每一种可能位置，都必须存在后手的应对位置，使集合包含关系成立。因此：

> $\sigma(\mathcal P_d)$ 中的每一块，都包含于 $\mathcal P_d$ 的某一块。

这说明前一个划分是后一个划分的细化。由于二者块数相同，且每一块都非空，所以两个划分只能完全相同：

$$
\sigma(\mathcal P_d)=\mathcal P_d.
$$

所以，后手必胜时，每轮的每一个叶子集合循环加一后，都必须能找到一个完全相同的对应集合。

## 充分性证明

假设每轮的划分都满足上述条件。

初始双方都位于根，显然有 $T[u]=S[v]$。

现在假设一轮结束后，先手位于 $u$，后手位于 $v$，并且保持着：

$$
T[u]=S[v].
$$

如果先手下一步进入孩子 $a$，下一轮的划分中一定存在对应位置 $b$，满足：

$$
S[b]=T[a].
$$

又因为：

$$
S[b]=T[a]\subseteq T[u]=S[v],
$$

所以 $b$ 必须属于 $v$ 的下一步状态：如果 $v$ 还不是叶子，$b$ 就是它的一个孩子；如果已经到达叶子，则原地等待。

原因是下一轮的划分是在当前轮的划分上继续细分得到的，其他分支的集合与 $S[v]$ 不相交。

因此，后手总能合法地保持集合相等。一直进行到双方都到达叶子，就得到后手的编号是先手编号的循环后继，后手获胜。

结合必要性，得到：

$$
\boxed{\text{后手必胜}\iff\text{每轮的叶子集合划分在循环加一后保持不变。}}
$$

## 为什么跳过只有一个叶子的子树

设 $cnt[u]=|S[u]|$。

当 $cnt[u]=1$ 时，节点 $u$ 以下是一条只通向某个确定叶子的末端链。后续已经没有选择，链长只影响到达时间。

例如：

```text
      5
    / | \
   1  2  4
         |
         3
```

先手进入 $4$ 后，最终一定到达 $3$。后手可以直接选择 $1$ 并等待。因此，不能要求这条末端链上的每个节点，都必须在同一深度找到对应的非叶节点。

实现时，不需要真的给提前到达的叶子补出后续状态，只检查大小大于 $1$ 的集合即可。

证明如下：在某一轮的划分中，若所有大小大于 $1$ 的集合循环加一后，都能匹配另一个大小大于 $1$ 的集合，那么这些集合就被 $\sigma$ 互相置换，因此它们的并集保持不变。剩余元素的集合也保持不变，而剩余的块全部是单元素集合，所以它们也自然满足匹配条件。

于是，只需要检查原树中：

$$
\boxed{
\forall u\text{ 满足 }cnt[u]>1,\quad
\exists v:\ dep[v]=dep[u],\ S[v]=T[u].
}
$$

这里跳过的是**只有一个叶子的子树**，不是所有只有一个孩子的节点。只有一个孩子的节点，其子树仍可能包含多个叶子，后续仍存在选择，不能直接跳过。

## 随机哈希维护集合

给每个叶子 $i$ 分配随机的 64 位权值 $h[i]$，维护：

$$
xr[u]=\bigoplus_{a\in S[u]}h[a],
$$

$$
shift[u]=\bigoplus_{a\in S[u]}h[\sigma(a)].
$$

对于叶子 $u$：

$$
cnt[u]=1,\qquad xr[u]=h[u],\qquad shift[u]=h[\sigma(u)].
$$

对于内部节点，将所有孩子的信息合并：

$$
cnt[u]=\sum_{v\text{ 是 }u\text{ 的孩子}}cnt[v],
$$

$$
xr[u]=\bigoplus_{v\text{ 是 }u\text{ 的孩子}}xr[v],\qquad
shift[u]=\bigoplus_{v\text{ 是 }u\text{ 的孩子}}shift[v].
$$

不同孩子的子树叶子集合互不相交，因此可以直接异或合并。

将所有 $cnt[u]>1$ 的节点对应的 $(dep[u],xr[u])$ 放入 `set`，然后检查每个这样的节点是否存在 $(dep[u],shift[u])$。全部存在则输出 `Doddle`，否则输出 `Tie`。

不能只用哈希值映射到一个深度，因为同一个叶子集合可能出现在一条链上的多个深度。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

using ull = unsigned long long;

mt19937_64 rng(
    chrono::steady_clock::now().time_since_epoch().count()
);

void solve()
{
    int n, m;
    cin >> n >> m;

    vector<ull> h(m + 1);
    for (int i = 1; i <= m; i++)
        h[i] = rng();

    vector<vector<int>> g(n + 1);
    for (int i = 1; i < n; i++)
    {
        int u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }

    vector<int> dep(n + 1), cnt(n + 1);
    vector<ull> xr(n + 1), shift(n + 1);
    set<pair<int, ull>> has;

    auto dfs = [&](auto &&self, int u, int fa) -> void
    {
        dep[u] = dep[fa] + 1;

        if (u <= m)
        {
            cnt[u] = 1;
            xr[u] = h[u];
            shift[u] = h[u % m + 1];
            return;
        }

        for (int v : g[u])
        {
            if (v == fa)
                continue;

            self(self, v, u);

            cnt[u] += cnt[v];
            xr[u] ^= xr[v];
            shift[u] ^= shift[v];
        }

        if (cnt[u] > 1)
            has.insert({dep[u], xr[u]});
    };

    dfs(dfs, n, 0);

    for (int u = m + 1; u <= n; u++)
    {
        if (cnt[u] <= 1)
            continue;

        if (!has.count({dep[u], shift[u]}))
        {
            cout << "Tie\n";
            return;
        }
    }

    cout << "Doddle\n";
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;
    while (t--)
        solve();

    return 0;
}
```

## 复杂度与实现注意

- 时间复杂度：$O(n\log n)$。
- 空间复杂度：$O(n)$。
- 集合相等使用随机哈希判断，存在极低概率的碰撞。
- lambda 递归最坏深度为 $O(n)$，需要注意运行环境的栈空间限制。

## 参考资料

- [原题：Bot Brothers](https://qoj.ac/download.php?id=8331&type=statement)
- [官方题解：CCPC 深圳 2023，D 题部分](https://assets.ucup.ac/tutorials/tutorials-2-25.pdf)

