#! https://zhuanlan.zhihu.com/p/681547583
[TOC]

# [2024牛客寒假算法基础集训营2](https://ac.nowcoder.com/acm/contest/67742)[A,B,D,E,F,I,J,K]

## [A-Tokitsukaze and Bracelet](https://ac.nowcoder.com/acm/contest/67742/A)

手环有 $3$ 种属性：普通攻击百分比加成，体力，精神。每次炼成手环时，会对手环的每个属性都随机赋予强化等级，每个属性的强化等级可能为$+0$, $+1$, $+2$。强化等级对应的属性值如下：  

-    对于普通攻击百分比加成来说：$+0$ 为 $100\%$，$+1$ 为 $150\%$，$+2$ 为 $200\%$
-    对于体力和精神来说：$+0$ 会在 $\{29,30,31,32\}$ 里随机选择，$+1$ 会在 $\{34,36,38,40\}$ 里随机选择， $+2$ 固定为 $45$。

例如，一个普通攻击百分比加成 $100\%$，体力 $45$，精神 $40$ 的手环的强化等级为 $+3$。其中普通攻击力百分比提供了 $+0$，体力提供了 $+2$，精神提供了 $+1$。  

现在 Tokitsukaze 炼成了 $n$ 个手环，她只知道每个手环的属性，请你告诉她每个手环的强化等级是多少。

**分析**

语法题

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67104279)

```C++
// NowCoder Tokitsukaze and Bracelet
// https://ac.nowcoder.com/acm/contest/67742/A 2024-02-05 13:06:29

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  int a, b, c;
  cin >> a >> b >> c;
  int ans = 0;
  ans += (a - 100) / 50;
  if (b == 45)
    ans += 2;
  else if (b <= 40 and b >= 34)
    ans++;
  if (c == 45)
    ans += 2;
  else if (c <= 40 and c >= 34)
    ans++;
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [B-Tokitsukaze and Cats_](https://ac.nowcoder.com/acm/contest/67742/B)

现在把 Tokitsukaze 的家看作是一个 $n \times m$ 的网格，第 $i$ 只猫的位置在 $(x_i,y_i)$。TomiokapEace 想用若干片防猫网来限制猫的移动，她将在一只猫所在格子的四周各放上一片防猫网。  

一片防猫板是位于两个相邻格子间隔，长度为1个单位的障碍物，可以阻止猫移动。具体来讲，当猫位于 $(x,y)$ 时，若$(x-1,y)$, $(x,y-1)$, $(x+1,y)$, $(x,y+1)$ 中的任意一格和 $(x,y)$ 之间不存在防猫板，则猫可以向相邻格子移动。  

TomiokapEace 想知道至少需要购买多少片防猫网才能使所有猫无法移动。  

**分析**

一个猫要用四片 围起来

如果俩猫相邻 那就节省一片

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67106560)

```C++
// NowCoder Tokitsukaze and Cats
// https://ac.nowcoder.com/acm/contest/67742/B 2024-02-05 13:09:13

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, k;
constexpr int N = 301;
int st[N][N];
int st2[N][N];
void solve() {
  cin >> n >> m >> k;
  for (int i = 1; i <= k; i++) {
    int x, y;
    cin >> x >> y;
    st[x][y] = 1;
  }
  int ans = 0;
  int dx[] = {0, 1, 0, -1};
  int dy[] = {1, 0, -1, 0};
  for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
      if (st[i][j] == 1)
        for (int k = 0; k < 4; k++) {
          int x = i + dx[k];
          int y = j + dy[k];
          if (st[x][y] == 1) ans--;
        }

  ans /= 2;
  ans += k * 4;
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  solve();
  return 0;
}
```

## [D-Tokitsukaze and Slash Draw](https://ac.nowcoder.com/acm/contest/67742/D)

Tokitsukaze 的卡组有 $n$ 张卡片，她已经知道「一击必杀！居合抽卡」这张卡片是在卡组中从最下面往上数的第 $k$ 张卡片。她挑选了 $m$ 种能够调整卡组中卡片顺序的卡片，第 $i$ 种类型的卡片效果是：把卡组最上方的 $a_i$ 张卡片拿出来按顺序放到卡组最下方。这个效果你可以理解为：首先将一张卡片拿出来，然后把这张卡片放到卡组最下方，这个动作重复做 $a_i$ 次。而发动第 $i$ 种类型的卡片效果，需要支付 $b_i$ 的''cost''。  

Tokitsukaze 可以通过一些操作使每种类型的卡片可以发动无限次，她是否能够让「一击必杀！居合抽卡」这张卡片出现在卡组的最上方？如果能，请输出最少需要支付的''cost''，如果不能，请输出 ''\-1''（不带引号）。

**分析**

我们把整个问题看成一张图

那么问题就是求从 起点到终点的 最短路

然后我们建边跑最短路就好

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67129986)

```c++
// NowCoder Tokitsukaze and Slash Draw
// https://ac.nowcoder.com/acm/contest/67742/D 2024-02-05 14:29:40

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, k;
using pii = pair<int, int>;
void solve() {
  cin >> n >> m >> k;
  k--;
  vector<int> a(m), b(m);
  vector<int> dp(n, INT_MAX);
  dp[k] = 0;
  for (int i = 0; i < m; i++) cin >> a[i] >> b[i];
  int ed = n - k - 1;
  map<pii, bool> st;
  vector<int> dist(n, LLONG_MAX / 2);
  vector<vector<pii>> G(n);
  vector<bool> stt(n);
  for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
      G[j].emplace_back(b[i], (j + a[i]) % n);
    }
  }
  priority_queue<pii, vector<pii>, greater<>> q;
  q.emplace(0, 0);
  dist[0] = 0;
  while (q.size()) {
    auto [a, b] = q.top();
    q.pop();
    for (auto [v, w] : G[b]) {
      if (dist[b] + v < dist[w]) {
        dist[w] = dist[b] + v;
        q.emplace(dist[w], w);
      }
    }
  }
  if (dist[ed] >= LLONG_MAX / 8) dist[ed] = -1;
  cout << dist[ed] << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [E-Tokitsukaze and Eliminate (easy)_](https://ac.nowcoder.com/acm/contest/67742/E)

**easy 与 hard 的唯一区别是** $col_i$ **的范围。**  

Tokitsukaze 正在玩一个消除游戏。  

初始有 $n$ 个宝石从左到右排成一排，第 $i$ 个宝石的颜色为 $col_i$。Tokitsukaze 可以进行若干次以下操作：  

-    任选一种颜色 $x$，将颜色为 $x$ 的最右边那颗宝石、以及该宝石右边的所有宝石全部消除。

Tokitsukaze 想知道至少需要几次操作才能把 $n$ 个宝石全部消除。

**分析**

用两个优先队列动态的维护就好

$O(nlogn)$ 可以解决

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67103203)

```C++
// NowCoder Tokitsukaze and Eliminate (easy)
// https://ac.nowcoder.com/acm/contest/67742/E 2024-02-05 13:01:03

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  priority_queue<int> q[2];
  for (int i = 1; i <= n; i++) {
    int x;
    cin >> x;
    x--;
    q[x].emplace(i);
  }
  int ans = 0;
  int now = n + 1;
  while (q[0].size() and q[1].size()) {
    if (q[0].top() < q[1].top()) {
      auto it = q[0].top();
      q[0].pop();
      while (q[1].size() and q[1].top() > it) q[1].pop();
      ans++;
    } else {
      auto it = q[1].top();
      q[1].pop();
      while (q[0].size() and q[0].top() > it) q[0].pop();
      ans++;
    }
  }
  while (q[0].size()) {
    ans++;
    q[0].pop();
  }
  while (q[1].size()) {
    ans++;
    q[1].pop();
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [F-Tokitsukaze and Eliminate (hard)](https://ac.nowcoder.com/acm/contest/67742/F)

**easy 与 hard 的唯一区别是** $col_i$ **的范围。**  

Tokitsukaze 正在玩一个消除游戏。  

初始有 $n$ 个宝石从左到右排成一排，第 $i$ 个宝石的颜色为 $col_i$。Tokitsukaze 可以进行若干次以下操作：

-    任选一种颜色 $x$，将颜色为 $x$ 的最右边那颗宝石、以及该宝石右边的所有宝石全部消除。

Tokitsukaze 想知道至少需要几次操作才能把 $n$ 个宝石全部消除。

**分析**

我们维护一下当前最优的颜色 并把后面的 下标全删了就行

删除最多 $n$ 次，可以暴力

维护最优 用 `set` 可以 $log$ 的维护

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67122244)

```C++
// NowCoder Tokitsukaze and Eliminate (hard)
// https://ac.nowcoder.com/acm/contest/67742/F 2024-02-05 13:25:03

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
using pii = pair<int, int>;
template <class T, class U>
ostream &operator<<(ostream &os, const std::pair<T, U> &p) {
  os << "" << p.first << " " << p.second << endl;
  return os;
}
void solve() {
  cin >> n;
  int ans = 0;
  vector<int> a(n + 1);
  vector<vector<int>> col(n + 1);
  set<pii> S;
  set<int> S2;
  for (int i = 1; i <= n; i++) {
    cin >> a[i];
    col[a[i]].emplace_back(i);
    S2.emplace(a[i]);
  }
  for (auto it : S2) {
    S.emplace(col[it].back(), it);
  }
  auto _n = n;
  while (_n) {
    int _mn = INT_MAX;
    for (auto i : S2) _mn = min(_mn, col[i].back());
    vector<int> t;
    for (auto i : S2) {
      while (col[i].size() and col[i].back() >= _mn)
        col[i].pop_back(), _n--;
      if (!col[i].size())
        t.emplace_back(i);
      else
        S.emplace(col[i].back(), i);
    }
    for (auto i : t) S2.erase(S2.find(i));
    t.clear();
    ans++;
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [I-Tokitsukaze and Short Path (plus)](https://ac.nowcoder.com/acm/contest/67742/I)

**plus 与 minus 的唯一区别是 边权的计算方式。**  

Tokitsukaze 有一张 $n$ 个顶点的完全图 $G$, 顶点编号是 $1$ 到 $n$，编号为 $i$ 的顶点的值是 $a_i$。  

完全图指的是每对顶点之间都恰好有一条无向边的图。对于顶点 $u$ 和顶点 $v$ 之间的无向边，边权计算方式如下：  

$w_{u,v}=\begin{cases} 0, & u = v \\ |a_u+a_v|+|a_u-a_v|, & u \ne v \end{cases}$  

定义 $dist(i,j)$ 表示顶点 $i$ 为起点，顶点 $j$ 为终点的最短路。求 $\sum_{i=1}^n \sum_{j=1}^n dist(i,j)$  

关于最短路的定义：  

定义一条从 $s$ 到 $t$ 的路径为若干条首尾相接的边形成的序列且该序列的第一条边的起点为 $s$，最后一条边的终点为 $t$，特别的，当 $s=t$ 时该序列可以为空。  

定义一条从 $s$ 到 $t$ 的路径长度为该路径中边权的总和。  

定义 $s$ 到 $t$ 的最短路为 $s$ 到 $t$ 所有路径长度中的最小值。

**分析**

可以发现 边权就是 最大值的二倍

两者之间的转移 要不然是 直接转移

要不然是 通过最大值转移

遍历一遍就好

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67116521)

```C++
// NowCoder Tokitsukaze and Short Path (minus)
// https://ac.nowcoder.com/acm/contest/67742/J 2024-02-05 13:48:38

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  sort(all(a));
  int ans = 0;
  int mm = a[n - 1] * 4;
  for (int i = 0; i < n; i++) {
    int col = i;
    ans += min(mm, a[i] * 2) * col * 2;
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [J-Tokitsukaze and Short Path (minus)](https://ac.nowcoder.com/acm/contest/67742/J)

**plus 与 minus 的唯一区别是 边权的计算方式。**  

Tokitsukaze 有一张 $n$ 个顶点的完全图 $G$, 顶点编号是 $1$ 到 $n$，编号为 $i$ 的顶点的值是 $a_i$。  

完全图指的是每对顶点之间都恰好有一条无向边的图。对于顶点 $u$ 和顶点 $v$ 之间的无向边，边权计算方式如下：  

$w_{u,v}=\begin{cases} 0, & u = v \\ |a_u+a_v|-|a_u-a_v|, & u \ne v \end{cases}$  

定义 $dist(i,j)$ 表示顶点 $i$ 为起点，顶点 $j$ 为终点的最短路。求 $\sum_{i=1}^n \sum_{j=1}^n dist(i,j)$  

关于最短路的定义：  

定义一条从 $s$ 到 $t$ 的路径为若干条首尾相接的边形成的序列且该序列的第一条边的起点为 $s$，最后一条边的终点为 $t$，特别的，当 $s=t$ 时该序列可以为空。  

定义一条从 $s$ 到 $t$ 的路径长度为该路径中边权的总和。  

定义 $s$ 到 $t$ 的最短路为 $s$ 到 $t$ 所有路径长度中的最小值。

**分析**

和上面的题几乎一模一样

把符号变一下就好

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67116089)

```C++
// NowCoder Tokitsukaze and Short Path (minus)
// https://ac.nowcoder.com/acm/contest/67742/J 2024-02-05 13:48:38

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  sort(all(a));
  int ans = 0;
  int mm = a[0] * 4;
  for (int i = 0; i < n; i++) {
    int col = n - i - 1;
    ans += min(mm, a[i] * 2) * col * 2;
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

## [K-Tokitsukaze and Password (easy)](https://ac.nowcoder.com/acm/contest/67742/K)

**easy 与 hard 的唯一区别是** $T$**,** $n$**, 以及字符集的范围。**

**easy, hard 是 EX 的子问题，若能通过 EX，在做简单修改后，必能通过 easy, hard。  
**


Tokitsukaze 在古老的遗迹中找到一张破旧的卷轴，上面写着的一串长度为 $n$ 的数字正是打开遗迹深处宝箱的密码。虽然卷轴上的一些数字已经模糊不清，但 Tokitsukaze 可以根据从世界各地打听到关于密码的情报来还原出真正的密码。令密码为整数 $x$，情报如下：  

-    情报1. $x$ 没有前导 $0$。
-    情报2. $x$ 是 $8$ 的倍数。
-    情报3. Tokitsukaze 获得了另一个长度为 $n$ 的整数 $y$，密码 $x$ 满足 $x \leq y$。
-    情报4. 在模糊不清的数字中，Tokitsukaze 知道其中一些数位上的数字是相同的，她会将这些数位使用小写字母标记出来，**标记着相同字母的数位上的数字一定相同，标记着不同字母的数位上的数字一定互不相同**；而另一些数字没有任何情报，它可能是 '0' 到 '9' 中的任意一个，这种数字 Tokitsukaze 会将它们用下划线 '\_' 标记出来。


下面是几个关于情报4的例子：  

-    ''1aa'' 它可以是 ''100'', ''111'',...,''199''，但它不能是 ''123''，因为标记着相同字母的数位上的数字一定相同。
-    ''2abc'' 它不能是 ''2119'', ''2191'', ''2222''。因为标记着不同字母的数位上的数字一定互不相同。
-    ''9b\_b\_'' 它可以是 ''91010'', ''95355'', ''95555'' 等，用 '\_' 标记的数位上的数字没有任何限制。


现在 Tokitsukaze 想要根据已有情报暴力破解密码，她想尝试所有可能的密码。她告诉你标记后的字符串 $s$，以及整数 $y$，请你告诉她有多少种不同的密码。  

不同的密码数量可能特别多，请输出对 $10^9+7$ 取模后的结果。

**分析**

$n$ ($1 \leq n \leq 9$)

直接 暴搜就好

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67137094)

```C++
// NowCoder Tokitsukaze and Password (easy)
// https://ac.nowcoder.com/acm/contest/67742/K 2024-02-05 15:23:01

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
string x, y;
int n, m;
int ans;
map<char, bool> mp;
map<string, bool> ttt;
void dfs(string s, int u, map<char, bool> mp) {
  if (u == n) {
    int val = atoi(s.c_str());
    int tt = atoi(y.c_str());
    if (s[0] >= '1' and val <= tt and ((val % 8) == 0)) {
      ans++;
    } else if (n == 1) {
      if (val <= tt and ((val % 8) == 0)) {
        ans++;
      }
    }
  } else {
    if (s[u] == '_') {
      string ss = s;
      for (char a = '0'; a <= '9'; a++) {
        ss[u] = a;
        dfs(ss, u + 1, mp);
      }
    } else if (s[u] <= 'z' and s[u] >= 'a') {
      for (char a = '0'; a <= '9'; a++) {
        string ss = s;
        if (mp[a] == true)
          continue;
        else {
          for (int i = 0; i < n; i++) {
            if (ss[i] == s[u]) ss[i] = a;
          }
          mp[a] = true;
          dfs(ss, u + 1, mp);
          mp[a] = false;
        }
      }
    } else
      dfs(s, u + 1, mp);
  }
}
void solve() {
  ans = 0;
  cin >> n;
  cin >> x >> y;
  dfs(x, 0, mp);
  ans %= (int)(1e9 + 7);
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

