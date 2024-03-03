#! https://zhuanlan.zhihu.com/p/682032856
# [ABC340](https://atcoder.jp/contests/abc340) [A-F]

## [A - Arithmetic Progression ](https://atcoder.jp/contests/abc340/tasks/abc340_a)

打印一个首项为 $A$，尾项为 $B$，公差为 $D$ 的算术数列。

您只需输入存在这样一个算术序列的输入值。

**分析**

语法题

**code**

[Submission #50138520](https://atcoder.jp/contests/abc340/submissions/50138520)

```C++
// AtCoder - KAJIMA CORPORATION CONTEST 2024（AtCoder Beginner Contest 340） A -
// Arithmetic Progression https://atcoder.jp/contests/abc340/tasks/abc340_a
// 2024-02-10 20:00:13

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  int a, b, d;
  cin >> a >> b >> d;
  for (int i = a; i <= b; i += d) cout << i << " ";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [B - Append](https://atcoder.jp/contests/abc340/tasks/abc340_B)



您有一个空序列 $A$。给出了 $Q$ 个查询，你需要按照给出的顺序处理它们。  
查询有以下两种类型：

- `1 x`:将 $x$ 追加到 $A$ 的末尾。
- `2 k`:从 $A$ 的末尾查找 $k$ 的值。当给出这个查询时，可以保证$A$的长度至少为$k$。

**分析**

语法题

**code**

[Submission #50142678](https://atcoder.jp/contests/abc340/submissions/50142678)

```C++
// AtCoder - KAJIMA CORPORATION CONTEST 2024（AtCoder Beginner Contest 340） B -
// Append https://atcoder.jp/contests/abc340/tasks/abc340_b 2024-02-10 20:01:40

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a;
  for (int i = 1; i <= n; i++) {
    int op;
    cin >> op;
    int x;
    cin >> x;
    if (op == 1) {
      a.emplace_back(x);
    } else {
      cout << *(a.end() - x) << "\n";
    }
  }
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [C - Divide and Divide ](https://atcoder.jp/contests/abc340/tasks/abc340_C)



黑板上写着一个整数 $N$。  
高桥将重复下面的一系列操作，直到所有不小于$2$的整数都从黑板上移除：

- 选择写在黑板上的一个不小于$2$的整数$x$。
- 擦去黑板上出现的一个$x$。然后，在黑板上写下两个新的整数 $\left \lfloor \dfrac{x}{2} \right\rfloor$ 和 $\left\lceil \dfrac{x}{2} \right\rceil$。
- 高桥必须支付 $x$ 日元才能完成这一系列操作。

这里，$\lfloor a \rfloor$表示不大于$a$的最大整数，$\lceil a \rceil$表示不小于$a$的最小整数。

当不能再进行操作时，高桥支付的总金额是多少？  
可以证明，无论操作的顺序如何，他支付的总金额是不变的。

**分析**

暴力搜索一下就好了

记得记忆化

不记忆化会退化成 $O(n)$

记忆化可以 $O(log^2n)$

**code**

未记忆化

[Submission #50147597](https://atcoder.jp/contests/abc340/submissions/50147597)

记忆化

[Submission #50149614](https://atcoder.jp/contests/abc340/submissions/50149614)

```C++
// AtCoder - KAJIMA CORPORATION CONTEST 2024（AtCoder Beginner Contest 340） C -
// Divide and Divide https://atcoder.jp/contests/abc340/tasks/abc340_c
// 2024-02-10 20:03:36

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
map<int, int> mp;
int dfs(int n) {
  if (mp.count(n)) return mp[n];
  int ans = 0;
  if (n < 2)
    return mp[n] = ans;
  else if (n & 1) {
    return n + (mp[n / 2] = dfs(n / 2)) + (mp[n / 2 + 1] = dfs(n / 2 + 1));
  } else
    return n + 2 * (mp[n / 2] = dfs(n / 2));
}
void solve() {
  cin >> n;
  cout << dfs(n);
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [D - Super Takahashi Bros. ](https://atcoder.jp/contests/abc340/tasks/abc340_D)

高桥正在玩一个游戏。

游戏由编号为 $1,2,\ldots,N$ 的 $N$ 个阶段组成。最初，只有阶段 $1$ 可以玩。

对于每个可以下棋的阶段 $i$ ( $1\leq i \leq N-1$ )，你都可以在阶段 $i$ 执行以下两个操作中的一个：

- 花费$A_i$秒清除阶段$i$。这样就可以进入$i+1$阶段。
- 花费$B_i$秒清除阶段$i$。这样就可以进入$X_i$阶段。

忽略通关时间以外的其他时间，至少需要多少秒才能通关$N$？

**分析**

这个转移是 有环的 ，DP 比较难想

所以我们 用 最短路处理环

**code**

[Submission #50156551](https://atcoder.jp/contests/abc340/submissions/50156551)

```C++
// AtCoder - KAJIMA CORPORATION CONTEST 2024（AtCoder Beginner Contest 340） D -
// Super Takahashi Bros. https://atcoder.jp/contests/abc340/tasks/abc340_d
// 2024-02-10 20:07:52

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
constexpr int N = 2e5 + 10;
int dp[N];
int a[N], b[N], x[N];
int dfs(int u) {}
using pii = pair<int, int>;
template <class T, class U>
ostream &operator<<(ostream &os, const std::pair<T, U> &p) {
  os << "" << p.first << " " << p.second << endl;
  return os;
}
void solve() {
  cin >> n;
  for (int i = 1; i <= n; i++) cin >> a[i] >> b[i] >> x[i];
  vector<int> dist(n + 2, LLONG_MAX / 2);
  dist[1] = 0;
  vector<vector<pii>> G;
  G.resize(n + 1);
  for (int i = 1; i <= n; i++) {
    if (i != n) G[i].emplace_back(i + 1, a[i]);
    G[i].emplace_back(x[i], b[i]);
  }
  priority_queue<pii, vector<pii>, greater<>> q;
  q.emplace(0, 1);
  while (q.size()) {
    auto [v, w] = q.top();
    q.pop();
    for (auto [to, val] : G[w]) {
      if (dist[to] > val + v) {
        dist[to] = val + v;
        q.emplace(dist[to], to);
      }
    }
  }
  cout << dist[n] << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [E - Mancala 2 ](https://atcoder.jp/contests/abc340/tasks/abc340_E)



有 $N$ 个编号为 $0$ 至 $N-1$ 的盒子。最初，$i$盒里有$A_i$个球。

高桥将依次对$i=1,2,\ldots,M$进行以下操作：

- 将变量 $C$ 设为 $0$。
- 从盒子 $B_i$ 中取出所有的球并握在手中。
- 在手拿至少一个球的同时，重复下面的过程：
    - 将 $C$ 的值增加 $1$。
    - 将手中的一个球放入盒子 $(B_i+C) \bmod N$。

完成所有操作后，确定每个盒子中的球数。

**分析**

比较典型的区间修改 单点查询

套个线段树的 板子就好

**code**

[Submission #50164846 ](https://atcoder.jp/contests/abc340/submissions/50164846)

```C++
// AtCoder - KAJIMA CORPORATION CONTEST 2024（AtCoder Beginner Contest 340） E -
// Mancala 2 https://atcoder.jp/contests/abc340/tasks/abc340_e 2024-02-10
// 20:18:36

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
template <class Info, class Tag>
struct LazySegmentTree {
  int n;
  std::vector<Info> info;
  std::vector<Tag> tag;
  LazySegmentTree() : n(0) {}
  LazySegmentTree(int n_, Info v_ = Info()) { init(n_, v_); }
  template <class T>
  LazySegmentTree(std::vector<T> init_) {
    init(init_);
  }
  void init(int n_, Info v_ = Info()) { init(std::vector<Info>(n_, v_)); }
  template <class T>
  void init(std::vector<T> init_) {
    n = init_.size();
    info.assign(4 << std::__lg(n), Info());
    tag.assign(4 << std::__lg(n), Tag());
    std::function<void(int, int, int)> build = [&](int p, int l, int r) {
      if (r - l == 1) {
        info[p] = init_[l];
        return;
      }
      int m = (l + r) / 2;
      build(2 * p, l, m);
      build(2 * p + 1, m, r);
      pull(p);
    };
    build(1, 0, n);
  }
  void pull(int p) { info[p] = info[2 * p] + info[2 * p + 1]; }
  void apply(int p, const Tag &v) {
    info[p].apply(v);
    tag[p].apply(v);
  }
  void push(int p) {
    apply(2 * p, tag[p]);
    apply(2 * p + 1, tag[p]);
    tag[p] = Tag();
  }
  void modify(int p, int l, int r, int x, const Info &v) {
    if (r - l == 1) {
      info[p] = v;
      return;
    }
    int m = (l + r) / 2;
    push(p);
    if (x < m) {
      modify(2 * p, l, m, x, v);
    } else {
      modify(2 * p + 1, m, r, x, v);
    }
    pull(p);
  }
  void modify(int p, const Info &v) { modify(1, 0, n, p, v); }
  Info rangeQuery(int p, int l, int r, int x, int y) {
    if (l >= y || r <= x) {
      return Info();
    }
    if (l >= x && r <= y) {
      return info[p];
    }
    int m = (l + r) / 2;
    push(p);
    return rangeQuery(2 * p, l, m, x, y) + rangeQuery(2 * p + 1, m, r, x, y);
  }
  Info rangeQuery(int l, int r) { return rangeQuery(1, 0, n, l, r); }
  void rangeApply(int p, int l, int r, int x, int y, const Tag &v) {
    if (l >= y || r <= x) {
      return;
    }
    if (l >= x && r <= y) {
      apply(p, v);
      return;
    }
    int m = (l + r) / 2;
    push(p);
    rangeApply(2 * p, l, m, x, y, v);
    rangeApply(2 * p + 1, m, r, x, y, v);
    pull(p);
  }
  void rangeApply(int l, int r, const Tag &v) {
    return rangeApply(1, 0, n, l, r, v);
  }
  template <class F>
  int findFirst(int p, int l, int r, int x, int y, F pred) {
    if (l >= y || r <= x || !pred(info[p])) {
      return -1;
    }
    if (r - l == 1) {
      return l;
    }
    int m = (l + r) / 2;
    push(p);
    int res = findFirst(2 * p, l, m, x, y, pred);
    if (res == -1) {
      res = findFirst(2 * p + 1, m, r, x, y, pred);
    }
    return res;
  }
  template <class F>
  int findFirst(int l, int r, F pred) {
    return findFirst(1, 0, n, l, r, pred);
  }
  template <class F>
  int findLast(int p, int l, int r, int x, int y, F pred) {
    if (l >= y || r <= x || !pred(info[p])) {
      return -1;
    }
    if (r - l == 1) {
      return l;
    }
    int m = (l + r) / 2;
    push(p);
    int res = findLast(2 * p + 1, m, r, x, y, pred);
    if (res == -1) {
      res = findLast(2 * p, l, m, x, y, pred);
    }
    return res;
  }
  template <class F>
  int findLast(int l, int r, F pred) {
    return findLast(1, 0, n, l, r, pred);
  }
};  // 懒标记线段树
struct Tag {
  int add;
  void apply(Tag t) { this->add += t.add; }
};

struct Info {
  int sum;
  void apply(Tag t) { this->sum += t.add; }
};

Info operator+(const Info &a, const Info &b) {
  // 合并两个信息节点
  Info c;
  c.sum = a.sum + b.sum;
  return c;
}

void solve() {
  cin >> n >> m;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  vector<int> b(m);
  for (auto &i : b) cin >> i;
  vector<Info> _a;
  for (auto i : a) {
    _a.emplace_back(i);
  }
  LazySegmentTree<Info, Tag> T(_a);
  for (auto it : b) {
    auto res = T.rangeQuery(it, it + 1).sum;
    if (res > 0) {
      T.rangeApply(it, it + 1, (Tag){-res});
      if (res + it < n) {
        T.rangeApply(it + 1, it + 1 + res, (Tag){1});
      } else {
        int ran = res - (n - it);
        int cnt = ran / n;
        int mod = ran % n;
        T.rangeApply(it + 1, n, (Tag){1});
        T.rangeApply(0, n, (Tag){cnt});
        T.rangeApply(0, mod + 1, (Tag){1});
      }
    } else {
      continue;
    }
  }
  for (int i = 0; i < n; i++) cout << T.rangeQuery(i, i + 1).sum << " ";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [F - S = 1 (atcoder.jp)](https://atcoder.jp/contests/abc340/tasks/abc340_F)

给你整数 $X$ 和 $Y$，它们至少满足 $X \neq 0$ 和 $Y \neq 0$ 中的一个。  
请找出一对满足以下所有条件的整数 $(A, B)$。如果不存在这样的一对，请报告。

- $-10^{18} \leq A, B \leq 10^{18}$
- 顶点位于 $xy$ 平面上点 $(0, 0), (X, Y), (A, B)$ 的三角形的面积为 $1$.

**分析**

我们用 叉积想一下

要求的就是 平面上点 $(0, 0), (X, Y), (A, B)$ 的 平行四边形 面积是 $2$

那就是 $XB-YA=|2|$

我们用 拓展欧几里得求一下就好

**code**

[Submission #50178505 ](https://atcoder.jp/contests/abc340/submissions/50178505)

```C++
// AtCoder - KAJIMA CORPORATION CONTEST 2024（AtCoder Beginner Contest 340） F -
// S = 1 https://atcoder.jp/contests/abc340/tasks/abc340_f 2024-02-10 20:41:16

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
// 拓展欧几里得算法
int exgcd(int a, int b, int &x, int &y) {  // 返回gcd(a,b) 并求出解(引用带回)
  if (b == 0LL) {
    x = 1LL;
    y = 0LL;
    return a;
  }
  int x1, y1, gcd;
  gcd = exgcd(b, a % b, x1, y1);
  x = y1, y = x1 - a / b * y1;
  return gcd;
}
void solve() {
  cin >> n >> m;
  int x, y;
  int gcd = exgcd(m, -n, x, y);
  gcd = abs(gcd);
  if ((not gcd) or (2LL % gcd)) {
    cout << "-1" << '\n';
  } else if (gcd) {
    if (x == 0 and n == 0) swap(x, y);
    if (y == 0 and m == 0) swap(x, y);
    cout << 2LL / gcd * x << " " << 2LL / gcd * y << "\n";
  }
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

