#! https://zhuanlan.zhihu.com/p/680250527
#[AtCoder Beginner Contest 338[A-F]](https://atcoder.jp/contests/abc338) 

![114543235_p0.jpg](https://cdn.acwing.com/media/article/image/2024/01/28/215908_31aaaa1abd-114543235_p0.jpg) 


## **A - Capitalized?**

问题陈述

您将得到一个由大写和小写英文字母组成的非空字符串$S$。判断是否满足以下条件:

—“$S$”的第一个字符必须大写，其他字符必须小写。

**分析**

语法题

**code**

[Submission #49691742 - AtCoder Beginner Contest 338](https://atcoder.jp/contests/abc338/submissions/49691742)

```C++
// AtCoder - AtCoder Beginner Contest 338 A - Capitalized?
// https://atcoder.jp/contests/abc338/tasks/abc338_a 2024-01-27 20:00:18

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
bool is(char a) { return a >= 'a' and a <= 'z'; }
void solve() {
  string s;
  cin >> s;
  bool falg = true;
  falg &= (not is(s[0]));
  for (int i = 1; i < s.size(); i++) falg &= is(s[i]);
  if (falg)
    cout << "Yes\n";
  else
    cout << "No\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  solve();
  return 0;
}
```

## **B - Frequency**

问题陈述

你得到一个由小写英文字母组成的字符串$S$。找出在$S$中出现最频繁的字符。如果存在多个这样的字符，请按字母顺序报告出现最早的字符。

**分析**

语法题

**code**

[Submission #49698974 - AtCoder Beginner Contest 338](https://atcoder.jp/contests/abc338/submissions/49698974)

```C++
// AtCoder - AtCoder Beginner Contest 338 B - Frequency
// https://atcoder.jp/contests/abc338/tasks/abc338_b 2024-01-27 20:01:58

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
template <typename T>
T read() {
  T x;
  cin >> x;
  return x;
}
void solve() {
  map<char, int> mp;
  string s = read<string>();
  for (auto it : s) mp[it]++;
  int _max = 0;
  char ans = 'z';
  for (auto _a = 'a'; _a <= 'z'; _a++) 
    if (mp[_a] > _max) ans = _a, _max = mp[_a];
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

## **C - Leftover Recipes**

问题陈述

你的冰箱有$N$种食材。我们称它们为成分$1$，$\dots$，成分$N$。你有$Q_i$克成分$i$。

你可以做两种菜。做一份A菜，每种食材$i$ $(1 \leq i \leq N)$需要$A_i$克。要做一份B菜，每种配料$i$需要$B_i$克。每种菜你只能做整数份。

只用冰箱里的食材，你能做的菜的最大份数是多少?

Constraints

-   $1 \leq N \leq 10$
-   $1 \leq Q_i \leq 10^6$
-   $0 \leq A_i \leq 10^6$
-   There is an $i$ such that $A_i \geq 1$.
-   $0 \leq B_i \leq 10^6$
-   There is an $i$ such that $B_i \geq 1$.
-   All input values are integers.

**分析**

既然$N\le 10$

那么我们直接枚举 A 做几次 B 做几次

那么 可以 $O(N|值域|)$ 的处理

**code**

[Submission #49705799 - AtCoder Beginner Contest 338](https://atcoder.jp/contests/abc338/submissions/49705799)

```C++
// AtCoder - AtCoder Beginner Contest 338 C - Leftover Recipes
// https://atcoder.jp/contests/abc338/tasks/abc338_c 2024-01-27 20:05:48

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> Q(n + 1), A(n + 1), B(n + 1);
  int _max = INT_MAX;
  for (int i = 1; i <= n; i++) cin >> Q[i];
  for (int i = 1; i <= n; i++) cin >> A[i];
  for (int i = 1; i <= n; i++)
    if (A[i]) _max = min(_max, Q[i] / A[i]);
  for (int i = 1; i <= n; i++) cin >> B[i];
  int ans = 0;
  for (int i = 0; i <= _max; i++) {
    auto _B = Q;
    int _ansB = INT_MAX;
    for (int j = 1; j <= n; j++)
      if (B[j]) _B[j] -= A[j] * i, _ansB = min(_ansB, _B[j] / B[j]);
    ans = max(ans, _ansB + i);
  }
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

## **D - Island Tour**

AtCoder群岛由$N$个岛屿组成，由$N$座桥连接。岛屿编号从$1$到$N$，第$i$座桥($1\leq i\leq N-1$)将岛屿$i$和$i+1$双向连接，而第$N$座桥将岛屿$N$和$1$双向连接。除了过桥，在岛屿之间没有别的交通方式。

在岛上，定期进行从$X_1$岛出发，依次参观$X_2, X_3, \dots, X_M$岛的**游**。旅游可能会经过其他岛屿，而不是那些参观过的岛屿，在此期间桥梁的总次数

**分析**

我们可以发现这个题的操作是可以贪的

就是比如 你去掉这条路

那么只对走这条路 的情况 产生影响

那么我们分析每一次 移动

在这条路上所有的 路 都加上一个权重

最后答案加上 权重最小的 可以保证答案最优

**code**

[Submission #49725307 - AtCoder Beginner Contest 338](https://atcoder.jp/contests/abc338/submissions/49725307)

```C++
// AtCoder - AtCoder Beginner Contest 338 D - Island Tour
// https://atcoder.jp/contests/abc338/tasks/abc338_d 2024-01-27 20:12:22

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
template <typename... Args>
void debug(Args &&...args) {
  ((cerr << args << " "), ...), cerr << "\n";
}
void solve() {
  cin >> n >> m;
  vector<int> X(m + 1);
  vector<int> _sum(n + 2, 0);
  for (int i = 1; i <= m; i++) cin >> X[i];
  int _ans = 0;
  for (int i = 2; i <= m; i++) {
    int _x = abs(X[i] - X[i - 1]);
    int _y = n - abs(X[i] - X[i - 1]);
    _ans += min(_x, _y);
    int cost = abs(_x - _y);
    if (X[i - 1] < X[i]) {
      // 如果 目的地编号大于当前的
      if (_x < _y) {
        // 那么就顺时针去
        _sum[X[i - 1]] += cost;
        _sum[X[i]] -= cost;

      } else if (_y < _x) {
        // 那么就逆时针去
        _sum[X[i]] += cost;
        _sum[n + 1] -= cost;
        _sum[0] += cost;
        _sum[X[i - 1]] -= cost;
      }
    } else if (X[i - 1] > X[i]) {
      // 如果 目的地编号小于当前的
      if (_x < _y) {
        // 那么就逆时针去
        _sum[X[i]] += cost;
        _sum[X[i - 1]] -= cost;
      } else if (_y < _x) {
        // 那么就顺时针去
        _sum[X[i - 1]] += cost;
        _sum[n + 1] -= cost;
        _sum[0] += cost;
        _sum[X[i]] -= cost;
      }
    }
  }
  int _min = LLONG_MAX / 2;
  for (int i = 1; i <= n; i++)
    _sum[i] += _sum[i - 1], _min = min(_min, _sum[i]);
  cout << _ans + _min << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  solve();
  return 0;
}
```

## **E - Chords**

问题陈述

在一个圆上以相等的间隔放置$2N$个点，从某一点开始按顺时针方向编号为$1$到$2N$。

圆上也有$N$和弦，$i$\-弦连接点$A_i$和$B_i$。它保证所有的值$A_1,\dots,A_N,B_1,\dots,B_N$都是不同的。

确定和弦之间是否有交集。

![](https://img.atcoder.jp/abc338/de1d9dd6cf38caec1c69fe035bdba545.png) 

**分析**

我们观察一下 上面的 图

我们把 线 抽象成 线段

那就是 线相不相交 就是线段 相不相交

线段当 没有交点 的时候 和 完全包含的 时候是 满足条件的

那么我们就可以转换成 线段的 交

然后我们可以用逆序对 处理 线段

我这里用的树状数组处理的

**code**

[Submission #49728236 - AtCoder Beginner Contest 338](https://atcoder.jp/contests/abc338/submissions/49728236)

```C++
// AtCoder - AtCoder Beginner Contest 338 E - Chords
// https://atcoder.jp/contests/abc338/tasks/abc338_e 2024-01-27 20:59:44

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
struct szsz {
  int N;
  szsz() {}
  szsz(int u) : N(u) { val.resize(u + 1); }
  vector<int> val;
  int lowbit(int u) { return (u) & (-u); };
  void update(int idx, int x) {
    for (int i = idx; i <= N; i += lowbit(i)) {
      val[i] += x;
    }
  };
  int ask(int u) {
    int res = 0;
    for (int i = u; i; i -= lowbit(i)) {
      res += val[i];
    }
    return res;
  };
};
void solve() {
  cin >> n;
  vector<pii> a;
  szsz szsz(4e7 + 10);
  for (int i = 0; i < n; i++) {
    int x, y;
    cin >> x >> y;
    if (x > y) swap(x, y);
    a.emplace_back(x, y);
  }
  sort(all(a));
  for (auto [x, y] : a) {
    int _x = szsz.ask(x);
    int _y = szsz.ask(y);
    if (_x == _y) {
    } else {
      return (void)(cout << "Yes\n");
    }
    szsz.update(y, 1);
  }
  return (void)(cout << "No\n");
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << setprecision(15);
  solve();
  return 0;
}
```

## **F - Negative Traveling Salesman**

问题陈述

有一个带$N$顶点和$M$边的加权简单有向图。顶点编号为$1$到$N$，第$i$条边的权值为$W_i$，从顶点$U_i$延伸到顶点$V_i$。权值可以是负的，但是图中不包含负环。

确定是否存在至少访问每个顶点一次的遍历。如果存在这样的遍历，求遍历边的最小总权重。如果同一条边被遍历多次，则每次遍历都会增加该边的权重。

这里，“至少访问每个顶点的遍历”

### Constraints

-   $2\leq N \leq 20$
-   $1\leq M \leq N(N-1)$
-   $1\leq U_i,V_i \leq N$
-   $U_i \neq V_i$
-   $(U_i,V_i) \neq (U_j,V_j)$ for $i\neq j$
-   $-10^6\leq W_i \leq 10^6$
-   The given graph does not contain negative cycles.
-   All input values are integers.

我们可以发现 $N\le 20$

题目又要 表示 每个位置有没有 经过

那就是很经典的 状态压缩 DP 跑 图论

每一位上 是1 的表示 已经走过的

然后 用边 不断地更新 当前的二进制表示

最后求最小值就好

时间复杂度是$O(N^22^N)$的 $4*10^8$ 可以跑过

**code**

[Submission #49742933 - AtCoder Beginner Contest 338](https://atcoder.jp/contests/abc338/submissions/49742933)

```C++
// AtCoder - AtCoder Beginner Contest 338 F - Negative Traveling Salesman
// https://atcoder.jp/contests/abc338/tasks/abc338_f 2024-01-27 21:13:26

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
constexpr int N = 21;
int dp[1 << N][N], f[N][N];
void solve() {
  cin >> n >> m;
  for (int i = 0; i <= n; i++)
    for (int j = 0; j <= n; j++) f[i][j] = LLONG_MAX / 4;
  for (int j = 0; j <= n; j++) f[j][j] = 0;
  for (int i = 1; i <= m; i++) {
    int x, y, c;
    cin >> x >> y >> c;
    x--, y--;
    f[x][y] = c;
  }
  for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
      for (int k = 0; k < n; k++)
        if (f[j][i] != LLONG_MAX / 4 and f[i][k] != LLONG_MAX / 4)
          f[j][k] = min(f[j][i] + f[i][k], f[j][k]);

  for (int i = 0; i < 1 << n; i++)
    for (int j = 0; j < n; j++) dp[i][j] = LLONG_MAX / 4;
  for (int i = 0; i < n; i++) dp[1 << i][i] = 0;
  for (int i = 0; i < (1 << n); i++)
    for (int j = 0; j < n; j++)
      if (dp[i][j] == LLONG_MAX / 4)
        continue;
      else
        for (int k = 0; k < n; k++)
          if ((i >> k) & 1)
            continue;
          else if (f[j][k] != LLONG_MAX / 4)
            dp[i | (1 << k)][k] = min(dp[i | (1 << k)][k], dp[i][j] + f[j][k]);

  int ans = LLONG_MAX / 2;
  int res = (1 << n) - 1;
  for (int i = 0; i < n; i++) ans = min(ans, dp[res][i]);
  if (ans >= LLONG_MAX / 8)
    cout << "No\n";
  else
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