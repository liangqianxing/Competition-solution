# 信息工程大学第五届超越杯程序设计竞赛

# 遗失的旋律

给一个n长度的01串，其中0和1字符分别表示为一种运算方法，一个数可以通过遍历一个运算区间得到一个运算结果。

用x来表示前面运算得出的结果，则有：

0字符所代表运算表示为：$x=x+1$  
1字符所代表运算表示为：$x = 2 \times x$

现在有m次询问,询问有如下两种情况：  
1.op=1时给定一个y，翻转y位置上的值(如0变1，1变0)。  
2.op=2时给定一个初始x和区间l,r，输出x在经历运算区间\[l,r\]之后的值，答案可能很大对998244353取模。

**t**

数据水了

**code**

```C++
// NowCoder 遗失的旋律
// https://ac.nowcoder.com/acm/contest/78567/A 2024-03-30 10:37:32

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
string s;
constexpr int mod = 998244353;
int query(int x, int l, int r) {
  int ans = x;
  for (int i = l; i <= r; i++) {
    if (s[i] == '0')
      ans = (ans + 1) % mod;
    else
      ans = (ans * 2) % mod;
  }
  return ans;
}
void solve() {
  cin >> n >> m;
  cin >> s;
  for (int i = 0; i < m; i++) {
    int op, x, l, r;
    cin >> op;
    if (op == 1) {
      cin >> x;
      x--;
      s[x] = '1' + '0' - s[x];
    } else {
      cin >> x >> l >> r;
      l--, r--;
      cout << query(x, l, r) << "\n";
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

# 时间的礼物

在一个普通的周末，萨姆发现了一只古老的怀表，它有着不可思议的力量——能够让时间暂停。起初，他用这个能力完成工作、读书，甚至避开尴尬的社交场合。但随着时间的流逝，他意识到真正的生活是与人共度时光，体验每一刻的喜悦与悲伤。最终，萨姆把怀表永久地停下，选择与家人和朋友们共享每一分每一秒。 

给定一个数n，你通过分解n得到一个m大小的数组（数组元素可以是0）。问一共有多少种分解方案。答案对P取模。

**THINK**

看了题面发现可以转成经典小球模型

**EX4**

n个小球放到m个盒子中,每个盒子不为空

求方案数

**隔板法**

$C_{n-1}^{m-1}$

**数学公式**
$$
x_1+x_2+\dots+x_m=n \quad(x_i\ge1)
$$

**EX5**

n个小球放到m个盒子中,每个盒子可以为空

求方案数

**数学公式**
$$
\begin{align}
x_1+x_2+\dots+x_m&=n \quad(x_i\ge0)\\
x_1+x_2+\dots+x_m+m&=n +m\quad(x_i\ge0)\\
(x_1+1)+(x_2+1)+\dots+(x_m+1)&=n+m\quad(x_i\ge0)\\
y_1+y_2+\dots+y_m&=n +m\quad(y_i\ge1)\\
\end{align}
$$
转换成了**EX4**的模型 $C^{m-1}_{n+m-1}$

问题转为求$C_{n+m-1}^{m-1} \pmod{P}$
$$
\begin{align}
1 \leq P \leq 1 \times 10^{9}\\
P不一定是质数
\end{align}
$$
所以我们对$P$ 进行 CRT分解

$P=P_1P_2\dots P_x$ 确保 $P_i$ 是质数

我们对每一个质数进行$lucas$ 求组合数

然后CRT 求出 对于$P$ 的答案

按理说这道题应该可以求出来的，但是我也不知道为什么

只有92分，剩下的部分我不知道了

**code** 

```C++
// NowCoder 时间的礼物
// https://ac.nowcoder.com/acm/contest/78567/B 2024-03-30 14:13:52

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, p;
constexpr int N = 2E6 + 10;
int jie[N], injie[N];
int qmi(int a, int b, int p) {
  int res = 1 % p;
  while (b) {
    if (b & 1) res = res * a % p;
    a = a * a % p;
    b >>= 1;
  }
  return res;
}
void init(int p) {
  jie[0] = 1;
  for (int i = 1; i <= min(p, n + m); i++) {
    jie[i] = (jie[i - 1] * i) % p;
  }
}
int C(int n, int r, int p) {
  if (n < r) return 0;
  return jie[n] * qmi(jie[r], p - 2, p) % p * qmi(jie[n - r], p - 2, p) % p;
}
int lucas(int n, int m, int p) {
  if (n < m) return 0;
  if (!n) return 1;
  if (n < p and m < p)
    return C(n % p, m % p, p) % p;
  else
    return lucas(n / p, m / p, p) * C(n % p, m % p, p) % p;
}
vector<int> get_fac(int x) {
  vector<int> res;
  for (int i = 2; i * i <= x; i++) {
    if (x % i == 0) {
      while (x % i == 0) x /= i;
      res.emplace_back(i);
    }
  }
  if (x != 1) res.emplace_back(x);
  return res;
}
int a[N];
using i64 = long long;
template <class T>
constexpr T power(T a, i64 b) {
  T res = 1;
  for (; b; b /= 2, a *= a) {
    if (b % 2) {
      res *= a;
    }
  }
  return res;
}

constexpr i64 mul(i64 a, i64 b, i64 p) {
  i64 res = a * b - i64(1.L * a * b / p) * p;
  res %= p;
  if (res < 0) {
    res += p;
  }
  return res;
}
template <i64 P>
struct MLong {
  i64 x;
  constexpr MLong() : x{} {}
  constexpr MLong(i64 x) : x{norm(x % getMod())} {}

  static i64 Mod;
  constexpr static i64 getMod() {
    if (P > 0) {
      return P;
    } else {
      return Mod;
    }
  }
  constexpr static void setMod(i64 Mod_) { Mod = Mod_; }
  constexpr i64 norm(i64 x) const {
    if (x < 0) {
      x += getMod();
    }
    if (x >= getMod()) {
      x -= getMod();
    }
    return x;
  }
  constexpr i64 val() const { return x; }
  explicit constexpr operator i64() const { return x; }
  constexpr MLong operator-() const {
    MLong res;
    res.x = norm(getMod() - x);
    return res;
  }
  constexpr MLong inv() const {
    assert(x != 0);
    return power(*this, getMod() - 2);
  }
  constexpr MLong &operator*=(MLong rhs) & {
    x = mul(x, rhs.x, getMod());
    return *this;
  }
  constexpr MLong &operator+=(MLong rhs) & {
    x = norm(x + rhs.x);
    return *this;
  }
  constexpr MLong &operator-=(MLong rhs) & {
    x = norm(x - rhs.x);
    return *this;
  }
  constexpr MLong &operator/=(MLong rhs) & { return *this *= rhs.inv(); }
  friend constexpr MLong operator*(MLong lhs, MLong rhs) {
    MLong res = lhs;
    res *= rhs;
    return res;
  }
  friend constexpr MLong operator+(MLong lhs, MLong rhs) {
    MLong res = lhs;
    res += rhs;
    return res;
  }
  friend constexpr MLong operator-(MLong lhs, MLong rhs) {
    MLong res = lhs;
    res -= rhs;
    return res;
  }
  friend constexpr MLong operator/(MLong lhs, MLong rhs) {
    MLong res = lhs;
    res /= rhs;
    return res;
  }
  friend constexpr std::istream &operator>>(std::istream &is, MLong &a) {
    i64 v;
    is >> v;
    a = MLong(v);
    return is;
  }
  friend constexpr std::ostream &operator<<(std::ostream &os, const MLong &a) {
    return os << a.val();
  }
  friend constexpr bool operator==(MLong lhs, MLong rhs) {
    return lhs.val() == rhs.val();
  }
  friend constexpr bool operator!=(MLong lhs, MLong rhs) {
    return lhs.val() != rhs.val();
  }
};

template <>
i64 MLong<0LL>::Mod = i64(1E18) + 9;
void solve() {
  cin >> n >> m >> p;
  if (n == m) {
    if (p == 1)
      cout << "0\n";
    else
      cout << "1\n";
    return;
  }
  auto it = get_fac(p);
  int _n = it.size();
  for (int i = 0; i < _n; i++) {
    int mn = it[i];
    init(mn);
    a[i] = lucas(n + m - 1, m - 1, it[i]);
  }
  vector<int> m(_n), inm(_n), c(_n);
  for (int i = 0; i < _n; i++) {
    m[i] = p / it[i];
  }
  for (int i = 0; i < _n; i++) {
    inm[i] = qmi(m[i], it[i] - 2, it[i]);
  }
  for (int i = 0; i < _n; i++) {
    c[i] = m[i] * inm[i];
  }
  MLong<0LL> ans = 0;
  ans.setMod(p);
  for (int i = 0; i < _n; i++) {
    ans += a[i] * c[i];
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

# 财政大臣

宝乐是王国的财政大臣，每年年底都要统计王国每个城市的财政收入。王国可以简略用一棵树来表示，每一个节点都代表一个城市，王国的首都一号城市视为树的根。现在只知道一年初始时每个城市的财政本金以及一年内财政收入发生的变化。请聪明的你帮助宝乐统计一下今年年底时每个城市的财政收入吧。  

财政收入变化有两种可能：

1.   $1$ $u$ $x$，以 $u$ 为根的子树中每个城市都增加 $x$ 收入。  
2.   $2$ $u$ $x$，以 $u$ 为根的子树中每个城市都减少 $x$ 收入。

**think**

观察以后发现是树链刨分板子

所以我们树刨以后扔线段树上

整体进行维护

**code**

```C++
#include <bits/stdc++.h>
template <typename... Args>
void debug(Args&&... args) {
  ((std::cerr << args << " "), ...);
  std::cerr << "\n";
}
template <typename T>
void debug(const std::string& var_name, const T& var_value) {
  std::cerr << var_name << "=" << var_value << "\n";
}
#define all(a) begin(a), end(a)
#define int long long
using namespace std;
constexpr int N = 2e5 + 10;
int n, m, r, mod = 1E18;
struct point {
  int l, r;
  int val, add;
  point(int l = 0, int r = 0, int val = 0, int add = 0)
      : l(l), r(r), val(val), add(add){};
  point operator+(const point& t) const {
    return point(l, t.r, (val + t.val) % mod);
  }
};
using p = point;
p tr[N * 4];
int a[N];
void pushup(int u) {
  auto& l = tr[u << 1];
  auto& r = tr[u << 1 | 1];
  tr[u] = tr[u << 1] + tr[u << 1 | 1];
}
void build(int u, int l, int r) {
  if (l == r) {
    tr[u] = {l, r, a[l], 0ll};
  } else {
    int mid = (l + r) >> 1;
    build(u << 1, l, mid);
    build(u << 1 | 1, mid + 1, r);
    pushup(u);
  }
}
void pushdown(int u) {
  auto& rt = tr[u];
  auto& l = tr[u << 1];
  auto& r = tr[u << 1 | 1];
  l.val += rt.add * (l.r - l.l + 1) % mod;
  l.val %= mod;
  l.add += rt.add;
  l.add %= mod;
  r.val += rt.add * (r.r - r.l + 1) % mod;
  r.val %= mod;
  r.add += rt.add;
  r.add %= mod;
  rt.add = 0;
}
void modi(int u, int l, int r, int v) {
  // debug(l, r, l + r >> 1, v, tr[u].l, tr[u].r, "lru");
  if (tr[u].l > r or tr[u].r < l) return;
  if (tr[u].l >= l and tr[u].r <= r) {
    tr[u].add += v;
    tr[u].val += v * (tr[u].r - tr[u].l + 1);
    tr[u].add %= mod;
    tr[u].val %= mod;
  } else {
    pushdown(u);
    auto& L = tr[u << 1];
    auto& R = tr[u << 1 | 1];
    int mid = (l + r) >> 1;
    modi(u << 1, l, r, v);
    modi(u << 1 | 1, l, r, v);
    pushup(u);
  }
}
int que(int u, int l, int r) {
  pushdown(u);
  auto& rt = tr[u];
  auto& L = tr[u << 1];
  auto& R = tr[u << 1 | 1];
  if (rt.l >= l and rt.r <= r) {
    return rt.val;
  } else {
    int ans = 0;
    if (l <= L.r) {
      ans += que(u << 1, l, r);
      ans %= mod;
    }
    if (r >= R.l) {
      ans += que(u << 1 | 1, l, r);
      ans %= mod;
    }
    pushup(u);
    return ans;
  }
}
struct graph {
  int n, cnt;
  vector<vector<int>> G;
  vector<int> in, fa, dep, siz, son, top, id, rk;
  vector<int> f;
  graph(int n) : n(n) {
    cnt = 0;
    G.resize(n + 1);
    in.resize(n + 1);
    fa.resize(n + 1);
    dep.resize(n + 1);
    siz.resize(n + 1);
    son.resize(n + 1);
    top.resize(n + 1);
    id.resize(n + 1);
    rk.resize(n + 1);
    f.resize(n + 1);
  };
  void add(int a, int b) {
    G[a].emplace_back(b);
    in[b]++;
  }
  void dfs1(int u, int fath, int depth) {
    fa[u] = fath, dep[u] = depth, siz[u] = 1;
    for (auto it : G[u]) {
      if (it == fath)
        continue;
      else {
        dfs1(it, u, depth + 1);
        siz[u] += siz[it];
        if (siz[it] > siz[son[u]]) son[u] = it;
      }
    }
  }
  void dfs2(int u, int t) {
    top[u] = t;
    id[u] = ++cnt;
    rk[cnt] = u;
    if (!son[u])
      return;
    else
      dfs2(son[u], t);
    for (auto it : G[u]) {
      if (it != son[u] and it != fa[u]) dfs2(it, it);
    }
  }
  int sum(int x, int y) {
    int rt = 1;
    int ans = 0, fx = top[x], fy = top[y];
    while (fx != fy) {
      if (dep[fx] >= dep[fy]) {
        ans += que(1, id[fx], id[x]);
        x = f[fx], fx = top[x];
      } else {
        ans += que(1, id[fy], id[y]);
        y = f[fy], fy = top[y];
      }
    }
      return ans;
  }
};
void solve() {
  cin >> n >> m;
  r = 1;
  graph G(n);
  for (int i = 1; i < n; i++) {
    int a, b;
    cin >> a >> b;
    G.add(a, b);
    G.add(b, a);
  }
  for (int i = 1; i <= n; i++) {
    int x;
    cin >> x;
    G.f[i] = x;
  }
  G.dfs1(r, 0, 1);
  G.dfs2(r, r);
  for (int i = 1; i <= n; i++) {
    a[i] = G.f[G.rk[i]];
  }
  build(1, 1, n);
  for (int i = 1; i <= m; i++) {
    int op, x;
    cin >> op >> x;
    int z;
    cin >> z;
    if (op == 1) {
    } else
      z = -z;
    modi(1, G.id[x], G.id[x] + G.siz[x] - 1, z);
  }
  for (int i = 1; i <= n; i++) cout << que(1, G.id[i], G.id[i]) << " ";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), std::cout.tie(0);
  cout << std::fixed << std::setprecision(15);
  cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  solve();
  return 0;
}
```

# 实验室有多少人

boss统计出了每个人在实验室的时间表，希望你能根据统计表计算出实验室最多有多少人。 

时间表有n行，每行有两个数，分别表示从哪天开始就一直在实验室，以及从哪天开始离开实验室。

**THINK**

求最大重叠线段数

**code**

```C++
// NowCoder 实验室有多少人
// https://ac.nowcoder.com/acm/contest/78567/D 2024-03-30 11:17:20

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
// #define int long long
int n, m;
using pii = pair<int, int>;
int f(vector<vector<int>>& a) {
  priority_queue<int, vector<int>, greater<int>> q;
  int ans = 0;
  int n = a.size();
  sort(a.begin(), a.end(), [](const vector<int>& x1, const vector<int>& x2) {
    return x1[0] < x2[0];
  });
  for (int i = 0; i < n; i++) {
    while (!q.empty() and q.top() < a[i][0]) q.pop();
    q.push(a[i][1]);
    ans = max(ans, (int)q.size());
  }
  return ans;
}

signed main() {
  int n;
  cin >> n;
  vector<vector<int>> a(n, vector<int>(2));
  for (int i = 0; i < n; i++) cin >> a[i][0] >> a[i][1], a[i][1]--;
  int ans = f(a);
  cout << ans << "\n";
  return 0;
}

```

# 在雾中寻宁静

给定一颗树，然后有q次操作，每一次操作给你一个结点和一种颜色，将这个结点的子树颜色全部染成该颜色，每次染色会覆盖，问最终染色完之后有每个结点颜色情况。 1为根节点，所有结点颜色默认一开始是0。

**THINK**

手玩一下可以发现只在根节点上打标记

遍历的时候再判断时间戳 传不传下去

时间是$O(n)$

**code**

```C++
// NowCoder 在雾中寻宁静
// https://ac.nowcoder.com/acm/contest/78567/E 2024-03-30 10:59:03

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
constexpr int N = 2E5 + 10;
int n, m;
struct p {
  int col, id;
} q[N];
void solve() {
  cin >> n;
  vector<int> fa(n + 1);
  vector<vector<int>> G(n + 1);
  for (int i = 2; i <= n; i++) {
    cin >> fa[i];
    G[fa[i]].emplace_back(i);
  }
  cin >> m;
  for (int i = 1; i <= m; i++) {
    int v, w;
    cin >> v >> w;
    q[v] = {w, i};
  }
  function<void(int, int, p)> dfs = [&](int u, int fa, p t) -> void {
    if (q[u].id != 0) {
      if (q[u].id > t.id) {
        for (auto it : G[u]) {
          dfs(it, u, q[u]);
        }
      } else {
        q[u] = t;
        for (auto it : G[u]) {
          dfs(it, u, q[u]);
        }
      }
    } else {
      q[u] = t;
      for (auto it : G[u]) {
        dfs(it, u, q[u]);
      }
    }
  };
  dfs(1, 0, q[1]);
  for (int i = 1; i <= n; i++) cout << q[i].col << " ";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

# 不规则的轮回

给定 n 个数对，对于每个数对 (x,y) 会持续执行以下操作直到 x=y：  
\- 当 x > y 时，x = x - y  
\- 当 x < y 时，y = y - x  
同时有 q 次询问，每次给定一个数对 $x_q,y_q$, 问上述给定的 n 个数对的执行过程中出现了 $x_q,y_q$ 的次数。

**THINK**

暴力即可，类似`gcd`

时间复杂度上界是$O(n)$

不会超时

**code**

```C++
// NowCoder 不规则的轮回
// https://ac.nowcoder.com/acm/contest/78567/F 2024-03-30 10:55:58

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
using pii = pair<int, int>;
map<pii, int> mp;
void f(int a, int b) {
  mp[{a, b}]++;
  if (a > b)
    f(a - b, b);
  else if (a < b)
    f(a, b - a);
}
void solve() {
  cin >> n;
  vector<int> a(n), b(n);
  for (int i = 0; i < n; i++) {
    cin >> a[i] >> b[i];
    f(a[i], b[i]);
  }
  cin >> m;
  for (int i = 0; i < m; i++) {
    int l, r;
    cin >> l >> r;
    cout << mp[{l, r}] << "\n";
  }
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

# 完美数字

宝乐很喜欢末尾数位上是0的数字，如果一个数末尾至少有k个0我们就称其为完美数。现在有一个长度为n的数组，如果一个子区间的所有数字之积是完美数，那么我们就称该区间为完美区间。宝乐想知道该数组有多少完美区间，聪明的你帮忙解决这个问题吧。 

本题子区间的定义：原始序列或集合中的一段连续的子集，其中元素之间的顺序保持不变。

**THINK**

双指针问题

**code**

```C++
// NowCoder 完美数字
// https://ac.nowcoder.com/acm/contest/78567/G 2024-03-30 12:03:39

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, k;
using pii = pair<int, int>;
template <class T, class U>
ostream &operator<<(ostream &os, const std::pair<T, U> &p) {
  os << "" << p.first << " " << p.second << endl;
  return os;
}
pii divi(int x) {
  int a = 0;
  int b = 0;
  while (x % 2 == 0 and x) x /= 2, a++;
  while (x % 5 == 0 and x) x /= 5, b++;
  return {a, b};
}
void solve() {
  cin >> n >> k;
  vector<int> a(n + 1);
  vector<pii> b(n + 1);
  for (int i = 1; i <= n; i++) {
    cin >> a[i];
    b[i] = divi(a[i]);
  }
  int x = 0, y = 0;
  int l = 1, ans = 0;
  for (int r = 1; r <= n; r++) {
    x += b[r].first;
    y += b[r].second;
    while (min(x, y) >= k and l <= r) {
      x -= b[l].first;
      y -= b[l++].second;
    }
    if (min(x + b[l - 1].first, y + b[l - 1].second) >= k) ans += l - 1;
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

# 春天与花儿

给定一个包含 n 个自然数的序列 A，你可以对序列进行以下操作：  
\- 选择一个数 $a_i$，使其数值变成 $a_i + 1$  
现在，给出一个数字 k，询问至少执行多少次操作可以使得序列 A 的乘积是 k 的倍数。

**think**

分类讨论

**code**

```C++
// NowCoder 春天与花儿
// https://ac.nowcoder.com/acm/contest/78567/H 2024-03-30 12:47:13

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, k;
constexpr int N = 1E6 + 10;
int a[N];
map<int, int> mp;
void solve() {
  cin >> n >> k;
  for (int i = 1; i <= n; i++) {
    cin >> a[i];
  }
  if (k == 1) {
    cout << 0 << "\n";
  } else if (k == 2) {
    for (int i = 1; i <= n; i++) {
      mp[a[i] % 2]++;
    }
    if (mp[0])
      cout << "0\n";
    else
      cout << "1\n";
  } else if (k == 3) {
    for (int i = 1; i <= n; i++) {
      mp[a[i] % 3]++;
    }
    if (mp[0])
      cout << "0\n";
    else if (mp[2])
      cout << "1\n";
    else
      cout << "2\n";
  } else if (k == 5) {
    for (int i = 1; i <= n; i++) {
      mp[a[i] % 5]++;
    }
    if (mp[0])
      cout << "0\n";
    else
      for (int i = 4; i >= 1; i--) {
        if (mp[i]) {
          cout << 5 - i << "\n";
          break;
        }
      }
  } else if (k == 4) {
    for (int i = 1; i <= n; i++) {
      mp[a[i] % 4]++;
    }
    if (mp[0])
      cout << "0\n";
    else {
      int ans = 4;
      for (int i = 3; i >= 1; i--) {
        if (mp[i]) {
          ans = min(ans, 4 - i);
        }
      }
      if (n >= 2) {
        if (mp[2] >= 2)
          ans = min(ans, 0LL);
        else {
          ans = min(ans, 2 - mp[2]);
        }
      }
      cout << ans << "\n";
    }
  } else if (k == 6) {
    for (int i = 1; i <= n; i++) {
      mp[a[i] % 6]++;
    }
    if (mp[0])
      cout << "0\n";
    else {
      int ans = 6;
      for (int i = 5; i >= 1; i--) {
        if (mp[i]) {
          ans = min(ans, 6 - i);
        }
      }
      if (n >= 2) {
        if ((mp[2] or mp[4]) and mp[3])
          ans = min(ans, 0LL);
        else {
          if ((mp[2] or mp[4])) {
            if (mp[2] >= 2) {
              ans = min(ans, 1LL);
            } else if (mp[4] and mp[2]) {
              ans = min(ans, 1LL);
            } else {
              if (mp[1]) {
                ans = min(ans, 2LL);
              }
            }
          } else if (mp[3]) {
            if (mp[1]) {
              ans = min(ans, 1LL);
            }
          } else {
            if (mp[1] >= 2) ans = min(ans, 3LL);
          }
        }
      }
      cout << ans << "\n";
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

# 孤独与追求

有一个长度为n的字符串S，字符串中只包含26个小写字母，并且字符都具有一定的价值，不同种类的字符价值不同。 对于S的每个子串，只有该子串为回文串的时候才具有价值，且价值为子串的字母之和，不然价值就是0。 请找出S中价值最大的子串，并输出他的价值。 

注意字符串中任意个连续的字符组成的子序列称为该串的子串。

**THINK**

跑一遍马拉车求出每个位置的最长回文子串长度

双指针暴力维护每个区间的答案

最劣情况下`n+2*n/2+4*n/4`最多`log(n)`段

所以可以在 $O(nlogn)$的 时间内解决

**code**

```C++
// NowCoder 孤独与追求
// https://ac.nowcoder.com/acm/contest/78567/I 2024-03-30 14:52:44

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
map<int, int> val;
vector<int> manacher(std::string s) {
  auto t = s;
  int n = t.size();
  std::vector<int> r(n);
  for (int i = 0, j = 0; i < n; i++) {
    if (2 * j - i >= 0 && j + r[j] > i) {
      r[i] = std::min(r[2 * j - i], j + r[j] - i);
    }
    while (i - r[i] >= 0 && i + r[i] < n && t[i - r[i]] == t[i + r[i]]) {
      r[i] += 1;
    }
    if (i + r[i] > j + r[j]) {
      j = i;
    }
  }
  return r;
}

void solve() {
  cin >> n;
  for (int i = 0; i < 26; i++) {
    cin >> val[i];
  }
  val['#' - 'a'] = 0;
  val['$' - 'a'] = 0;
  string s;
  cin >> s;
  string t = "#";
  for (auto c : s) {
    t += c;
    t += '#';
  }
  int n = t.size();
  auto it = manacher(t);
  vector<int> sum(n);
  for (int i = 0; i < n; i++) {
    sum[i] = val[t[i] - 'a'];
    if (i) sum[i] += sum[i - 1];
  }
  // for (int i = 0; i < n; i++) {
  // it[i]--;
  // }
  int ans = 0;
  for (int i = 1; i < n; i++) {
    // if ((i & 1) == 0) continue;
    for (int l = i, r = i; l >= i - it[i] + 1; l -= 2, r += 2) {
      int ssum = 0;
      if (l)
        ssum = sum[r] - sum[l - 1];
      else
        ssum = sum[r];
      ans = max(ans, ssum);
    }
    // cout << ssum << "\n";
  }
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

# 天使的拼图

在“天使的拼图”中，挑战者需要使用Tenshi图块完美覆盖一个巨大的n\*m的图形，完美覆盖要求图块之间不能重叠，且不能覆盖到矩形外，覆盖矩形不能有空隙。 

Tenshi图块由6个单位的正方形构成，如下图所示： 

![](https://uploadfiles.nowcoder.com/images/20240323/0_1711185830892/699882CF8ED4C3DEE4ECE1AD63033BF6)  

在覆盖矩形时，可以任意旋转或翻转Tenshi图块。 

有一些给定的n\*m矩形可能是无法完成的

请你找到哪些矩形是无法完成覆盖的。

**THINK**

手玩可以发现，所能拼成的矩形是3*4的

所以问题就变成了3\*4的矩形能不能填满n\*m的矩形

当n m 分别是 3 4 的倍数是 显然可以

还有一种情况

我们想象一列都是3\*4 一列都是4\*3

这样的情况下，要想使高度相同，那么高度就是12的倍数

宽度是3 4所能表示出的集合

对于$3,4$ 不能表示出的最大的数是$3\times4-3-4=5$

以及小于3的也不能表示出

其余情况都能表示

**code**

```C++
// NowCoder 天使的拼图
// https://ac.nowcoder.com/acm/contest/78567/K 2024-03-30 17:49:02

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n >> m;
  if (n % 3 == 0 and m % 4 == 0)
    cout << "Yes\n";
  else if (n % 4 == 0 and m % 3 == 0)
    cout << "Yes\n";
  else {
    if (n % 12 == 0 and m >= 3 and m != 5)
      cout << "Yes\n";
    else if (m % 12 == 0 and n >= 3 and n != 5)
      cout << "Yes\n";
    else
      cout << "No\n";
  }
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

# Monika's game

一个数n，初始得分为0。

每一次可以将一个数拆成两个正整数，要求这两个正整数之和等于被拆数，

获得这两个数乘积的积分。注意拆除后原来的数不再存在，分为被拆分的两个数。

可以进行该操作一直到无法再拆分为止，最后的得分为所有积分之和。

对于每一个n输出一行整数表示答案。

**THINK**

打表发现是

$\frac{n(n-1)}{2}$

**code**

```C++
// NowCoder Monika's game
// https://ac.nowcoder.com/acm/contest/78567/M 2024-03-30 10:31:10

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  cout << (n) * (n -1) / 2 << "\n";
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

