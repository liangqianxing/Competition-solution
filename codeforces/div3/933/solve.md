#! https://zhuanlan.zhihu.com/p/686567701
[toc]

# Div3 933

## **[A. Rudolf and the Ticket](https://codeforces.com/contest/1941/problem/A)**

鲁道夫要去拜访伯纳德，他决定乘坐地铁去见他。可以在只接受 2 个硬币的机器上购买门票，硬币的总和不超过 $k$ 。

鲁道夫有两个装有硬币的口袋。左边口袋里有 $n$ 个面额为 $b_1, b_2, \dots, b_n$ 的硬币。右侧口袋里有 $m$ 个面额为 $c_1, c_2, \dots, c_m$ 的硬币。他想从左口袋中恰好选择一枚硬币，并从右口袋中恰好选择一枚硬币(总共两枚硬币)。

帮助 Rudolf 确定有多少种方法可以选择索引 $f$ 和 $s$ 使得 $b_f + c_s \le k$  。

( $1 \le n, m \le 100, 1 \le k \le 2000$ )

**think**

观察数据范围以后我们发现可以跑$O(n^2)$的暴力

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, k;
void solve() {
  cin >> n >> m >> k;
  vector<int> b(n), c(m);
  for (auto &i : b) cin >> i;
  for (auto &i : c) cin >> i;
  sort(all(b)), sort(all(c));
  int ans = 0;
  for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++) {
      if (b[i] + c[j] <= k) ans++;
    }
  cout << ans << "\n";
}
signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [**B. Rudolf and 121**](https://codeforces.com/contest/1941/problem/B)

Rudolf 有一个包含 $n$ 个整数的数组 $a$ ，元素编号从 $1$ 到 $n$ 。

在一项操作中，他可以选择索引 $i$ ( $2 \le i \le n - 1$ ) 并分配：

- $a_{i - 1} = a_{i - 1} - 1$ 
- $a_i = a_i - 2$ 
- $a_{i + 1} = a_{i + 1} - 1$ 

鲁道夫可以多次应用此操作。任何索引 $i$ 都可以使用零次或多次。

他能用这个操作使数组的所有元素都等于0吗？

**think**
$$
\begin {align}
\forall i \in [1,n] \quad 让a_i=0 有唯一策略\\

\end {align}
$$
然后我们顺序执行并判断

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  for (int i = 0; i < n; i++) {
    if (a[i] > 0) {
      if (i + 2 < n) {
        int cnt = a[i];
        a[i] -= cnt;
        a[i + 1] -= cnt + cnt;
        a[i + 2] -= cnt;
      } else
        return (void)(cout << "NO\n");
    } else if (a[i] == 0)
      continue;
    else
      return (void)(cout << "NO\n");
  }
  cout << "YES\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [**C. Rudolf and the Ugly String**](https://codeforces.com/contest/1941/problem/C)

Rudolf 有一个长度为 $n$ 的字符串 $s$ 。如果字符串 $s$ 包含子字符串“pie”或子字符串“map”，鲁道夫认为字符串 $s$ 是丑陋的，否则字符串 $s$ 将被认为是漂亮的。

例如，“ppiee”、“mmap”、“dfpiefghmap”是丑陋的字符串，而“mathp”、“ppiiee”是漂亮的字符串。

Rudolf 希望通过删除一些字符来缩短字符串 $s$ 以使其美观。

主角不喜欢紧张，所以他要求你通过删除最少数量的字符来使字符串变得漂亮。他可以从字符串中的**任何**位置删除字符(不仅仅是从字符串的开头或结尾)。

$^\dagger$  如果字符串 $b$ 中存在等于 $a$ 的**连续**字符段，则字符串 $a$ 是 $b$ 的子字符串。

**think**

我们发现对于`map ` 和 `pie` 我们每次删除中间的字母就可以了

所以 操作次数 $= \sum _{i=1} ^n (s[i:i+3]=[map|pie])$

但是 这个是不对的

原因在于形如`mapie`  只需要删除 `p` 即可

所以我们需要容斥一下

操作次数 $= \sum _{i=1} ^n (s[i:i+3]=[map|pie])-\sum _{i=1} ^n (s[i:i+5]=[mapie])$

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  string s;
  cin >> s;
  string a = "map", b = "pie";
  bitset<1000100> st;
  int ans = 0;
  for (int i = 0; i + 2 < n; i++) {
    if (not st[i] and (string(begin(s) + i, begin(s) + i + 3) == a or
        string(begin(s) + i, begin(s) + i + 3) == b)) {
      st[i] = true;
      st[i + 1] = true;
      st[i + 2] = true;
      ans++;
    }
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [D. Rudolf and the Ball Game](https://codeforces.com/contest/1941/problem/D)

鲁道夫和伯纳德决定和他们的朋友玩一个游戏。 $n$ 人站成一圈，开始互相扔球。它们按顺时针顺序从 $1$ 到 $n$ 编号。

我们将球从一名球员传给他的邻居的运动称为转换。可以顺时针或逆时针进行过渡。

我们将从玩家 $y_1$ 到玩家 $y_2$ 的顺时针(逆时针)距离称为从玩家 $y_1$ 移动到玩家 $y_2$ 所需的顺时针(逆时针)转换次数。例如，如果为 $n=7$ ，则从 $2$ 到 $5$ 的顺时针距离为 $3$ ，从 $2$ 到 $5$ 的逆时针距离为 $4$ 。

最初，球的编号为 $x$ (玩家按顺时针方向编号)。在第 $i$ 次移动时，持球者将球顺时针或逆时针扔到距离 $r_i$ ( $1 \le r_i \le n - 1$ ) 的地方。例如，如果有 $7$ 个玩家，第 $2$ 个玩家接球后将球扔出 $5$ 的距离，则球将被第 $7$ 个玩家接住(顺时针投掷) ) 或第 $4$ 个玩家(逆时针投掷)。该示例的图示如下所示。

![](https://espresso.codeforces.com/9b74f6d4c5991dacc37be972d6589d05a2c4111d.png)

由于意外下雨，比赛在 $m$ 次投掷后中断。雨停了，大家又聚在一起继续前行。然而，没有人记得谁拿球。事实证明，伯纳德记住了每次投掷的距离以及**一些**投掷的方向(顺时针或逆时针)。

鲁道夫请你帮助他，并根据伯纳德提供的信息，计算出 $m$ 次投掷后可以拿球的球员人数。

**think**

可达性DP 模板

开个滚动数组记录一下 跑$O(n^2)$ 就好

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, x;
void solve() {
  cin >> n >> m >> x;
  x--;
  vector<int> st(n);
  st[x] = 1;
  for (int i = 0, r; i < m; i++) {
    char c;
    cin >> r >> c;
    vector<int> now(n);
    if (c == '?') {
      for (int i = 0; i < n; i++) now[(i + r) % n] |= st[i];
      for (int i = 0; i < n; i++) now[(i - r + n) % n] |= st[i];
    } else if (c == '0') {
      for (int i = 0; i < n; i++) now[(i + r) % n] |= st[i];
    } else {
      for (int i = 0; i < n; i++) now[(i - r + n) % n] |= st[i];
    }
    swap(now, st);
  }
  vector<int> idx;
  for (int i = 0; i < n; i++)
    if (st[i]) idx.emplace_back(i + 1);
  cout << idx.size() << "\n";
  for (auto it : idx) cout << it << " ";
  cout << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [E. Rudolf and k Bridges](https://codeforces.com/contest/1941/problem/E)

伯纳德喜欢拜访鲁道夫，但他总是迟到。问题是伯纳德必须乘坐渡轮过河。鲁道夫决定帮助他的朋友解决这个问题。

河流是一个由 $n$ 行和 $m$ 列组成的网格。第 $i$ 行和第 $j$ 列的交集包含数字 $a_{i,j}$ - 相应单元格中的深度。 **first**和**last** 列中的所有单元格都对应于河岸，因此它们的深度为 $0$ 。

![](https://espresso.codeforces.com/f21f7d8bd805233bd6369b6f61033c76a2e8fae6.png) 河流可能看起来像这样。

鲁道夫可以选择行 $(i,1), (i,2), \ldots, (i,m)$ 并在其上建造一座桥。在该行的每个单元格中，他都可以为桥安装一个支架。在单元 $(i,j)$ 中安装支撑的成本为 $a_{i,j}+1$ 。安装支撑件必须满足以下条件：

1、 $(i,1)$ 单元必须安装支架；
2、 $(i,m)$ 单元必须安装支架；

3. 任何一对相邻支撑之间的距离必须**不超过** $d$ 。

只建造一座桥是很无聊的。因此，鲁道夫决定在河流的**连续**行上建造 $k$ 座桥梁，即选择一些 $i$ ( $1 \le i \le n - k + 1$ )并在每行 $i, i + 1, \ldots, i + k - 1$ 上独立建造一座桥梁。帮助鲁道夫**最小化**安装支撑的总成本。

**think**

任何一对相邻支撑之间的距离必须**不超过** $d$ , 看到这个条件那就是 

单调队列优化$DP$

我们 对每一列跑 一遍

然后 最后再取 最优的 $k$ 个连续值

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, k, d;
using pii = pair<int, int>;
void solve() {
  cin >> n >> m >> k >> d;
  vector<vector<int>> a(n + 1, vector<int>(m + 1)),
      dp(n + 1, vector<int>(m + 1, LLONG_MAX / 4));
  for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++) cin >> a[i][j];
  for (int i = 1; i <= n; i++) dp[i][0] = 0;
  for (int i = 1; i <= n; i++) {
    priority_queue<pii, vector<pii>, greater<>> q;
    q.emplace(1, 1);
    dp[i][1] = 1;
    for (int j = 2; j <= m; j++) {
      while (q.top().second + d + 1 < j) q.pop();
      dp[i][j] = min(dp[i][j], a[i][j] + 1 + q.top().first);
      q.emplace(dp[i][j], j);
    }
  }
  vector<int> ans;
  for (int i = 1; i <= n; i++) ans.emplace_back(dp[i][m]);
  int cur = 0;
  for (int i = 0; i < k; i++) cur += ans[i];
  int __ans = cur;
  for (int i = 0, j = k; j < n; j++, i++) {
    cur -= ans[i], cur += ans[j];
    __ans = min(__ans, cur);
  }
  cout << __ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [F. Rudolf and Imbalance](https://codeforces.com/contest/1941/problem/F)

鲁道夫准备了一组 $n$ 复杂的问题 $a_1<a_2<a_3<\dots<a_n$ 。他对平衡性并不完全满意，所以他想添加**最多一个**问题来修复它。 

为此，鲁道夫提出了 $m$ 个问题模型和 $k$ 个函数。第 $i$ 个模型的复杂度为 $d_i$ ，第 $j$ 个函数的复杂度为 $f_j$ 。为了创建一个问题，他选择值 $i$ 和 $j$ ( $1 \le i \le m$ , $1 \le j \le k$ )，并将第 $i$ 个模型与第 $j$ 个函数相结合，得到一个新问题复杂程度为 $d_i + f_j$ 。 

为了确定该集合的不平衡性，Rudolf 按升序对问题的复杂性进行排序，并找到 $a_i - a_{i - 1}$ ( $i>1$  ) 的最大值。

鲁道夫通过添加最多一个问题(根据所描述的规则创建)可以实现的最小不平衡值是多少？ 

**think**

就是找到 $b_i\times c_j$ 插入到 $a$ 数组中， 使得 新数组的 最大间隔值 最小

然后我们就 找到最大的 间隔和 第二大的 间隔

二分答案

$O(nlog|V|)$ 进行`check`

总的复杂度是$O(nlog^2|V|)$

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
using ll = long long;
int n, m, k;
void solve() {
  cin >> n >> m >> k;
  vector<int> a(n), b(m), c(k);
  for (auto &i : a) cin >> i;
  for (auto &i : b) cin >> i;
  for (auto &i : c) cin >> i;
  int mx1 = -1, mx2 = -1, x = -1, y = -1;
  sort(all(a)), sort(all(c));
  for (int i = 1; i < n; i++)
    if (a[i] - a[i - 1] > mx1) {
      x = a[i - 1], y = a[i];
      mx2 = mx1;
      mx1 = a[i] - a[i - 1];
    } else if (a[i] - a[i - 1] > mx2)
      mx2 = a[i] - a[i - 1];
  ll l = max(mx2, (mx1 + 1) / 2), r = mx1;
  auto check = [&](ll mid) -> bool {
    ll vl = y - mid, vh = x + mid;
    if (vl > vh)
      return false;
    else {
      for (auto it : b) {
        auto I = lower_bound(all(c), vl - it);
        if (I != end(c) and *I + it <= vh) return true;
      }
    }
    return false;
  };
  ll ans = r;
  while (l <= r) {
    ll mid = l + r >> 1;
    if (check(mid)) {
      ans = mid;
      r = mid - 1;
    } else
      l = mid + 1;
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [G. Rudolf and Subway](https://codeforces.com/contest/1941/problem/G)

搭建桥梁对伯纳德没有帮助，他仍然到处迟到。然后鲁道夫决定教他如何使用地铁。

鲁道夫将地铁地图描述为一个无向连通图，没有自环，其中顶点代表车站。任意一对顶点之间至多有一条边。

如果两个顶点可以直接在相应的站点之间绕过其他站点，则通过边连接。鲁道夫和伯纳德居住的城市的地铁有颜色符号。这意味着站点之间的任何边缘都有特定的颜色。特定颜色的边缘一起形成一条地铁线。地铁线路**不能**包含不连通的边，并形成给定地铁图的连通子图。

图中显示了地铁地图的示例。

![](https://espresso.codeforces.com/31e30f806576f7b01bfd85b0add099f0d5011898.png)

鲁道夫声称，如果经过最少数量的地铁线路，该路线就是最佳路线。

帮助伯纳德确定给定出发站和目的地站的最小数量。

**think**

这道题我们用类似拆点的思想做

剩下的 放学回来更新

**code**

```C++
#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n >> m;
  vector<map<int, vector<int>>> adj(n);
  for (int i = 0; i < m; i++) {
    int u, v, c;
    cin >> u >> v >> c;
    u--, v--;
    adj[u][c].emplace_back(v);
    adj[v][c].emplace_back(u);
  }
  map<pair<int, int>, int> dis;
  deque<tuple<int, int, int>> q;
  int b, e;
  cin >> b >> e;
  b--, e--;
  q.emplace_back(0, b, 0);
  while (!q.empty()) {
    auto [d, x, a] = q.front();
    q.pop_front();
    if (dis.count({x, a})) continue;
    dis[{x, a}] = d;
    if (a) {
      q.emplace_front(d, x, 0);
      for (auto y : adj[x][a]) q.emplace_front(d, y, a);
    } else
      for (auto &[a, _] : adj[x]) q.emplace_back(d + 1, x, a);
  }
  cout << dis[{e, 0}] << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

