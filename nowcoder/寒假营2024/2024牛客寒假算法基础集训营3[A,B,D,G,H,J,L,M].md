#! https://zhuanlan.zhihu.com/p/681808847
[toc]

# [2024牛客寒假算法基础集训营3[A,B,D,G,H,J,L,M]](https://ac.nowcoder.com/acm/contest/67743#question)

## [A-智乃与瞩目狸猫、幸运水母、月宫龙虾_](https://ac.nowcoder.com/acm/contest/67743/A)

词性的前提下，只要求两个单词的**首字母**忽略大小写相同时就认为它们可能是一组ubuntu代号，请你编写程序判断给定的两个单词是否可能是一个ubuntu代号。

**分析**

语法题

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67180414)

```C++
// NowCoder 智乃与瞩目狸猫、幸运水母、月宫龙虾
// https://ac.nowcoder.com/acm/contest/67743/A 2024-02-07 13:02:29

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  string a, b;
  cin >> a >> b;
  if (a[0] >= 'A' and a[0] <= 'Z') a[0] += ('a' - 'A');
  if (b[0] >= 'A' and b[0] <= 'Z') b[0] += ('a' - 'A');
  if (a[0] == b[0])
    cout << "Yes\n";
  else
    cout << "No\n";
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

## [B-智乃的数字手串](https://ac.nowcoder.com/acm/contest/67743/B)

智乃有一个数字手串，数字手串上有$N$个数字，第$N$个数字与第$1$个数字相邻。智乃决定和清楚姐姐玩一个游戏。  

两个人轮流从数字手串上进行取数和交换的操作，当且仅当某两个相邻数字的和为偶数时，可以拿走这两个相邻数字中的任意一个，取数后将剩下的数字按照它们之前的相对顺序重新紧凑排列并选择两个数字交换他们的值，例如一开始的数字环为$[1,3,2,5,4,7]$，取走数字$3$后，数字手串变为$[1,2,5,4,7]$，接下来还可以选择两个数字交换他们的值，例如交换$1$和$2$变为$[2,1,5,4,7]$。（玩家在交换这个环节也可以选择跳过不进行任何交换操作）。  

若数字手串的尺寸为$1$，则玩家可以直接取走最后一个数字并获得游戏的胜利。  

若玩家不能进行取数操作，则该玩家失败输掉游戏。  

现在清楚姐姐先手取数，若两个人都采取最优策略，谁将获胜？

**分析**

我们倒着分析

 $n=1$ 是必胜态

$n=2$ 是 如果可以取就 转换到了另一方的必胜态

如果不能取就是 这一方的 必败态

所以 $n=2$ 必败

$n=3$ 一定可以拿

不存在 

$a+b $奇数

$b+c$奇数

$a+c$奇数

所以 $n=3$是 必胜态

$n=4$ 没有办法转移到 必胜态，所以$n=4 $是必败态

$n=5$没有办法转移到必败态，所以$n=5$是必胜态

所以 奇数是必胜态，偶数是 必败态

由此便能解决

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67188631)

```C++
// NowCoder 智乃的数字手串
// https://ac.nowcoder.com/acm/contest/67743/B 2024-02-07 13:08:04

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  if (n & 1 ^ 1)
    cout << "zn\n";
  else
    cout << "qcjj\n";
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

## [D-chino's bubble sort and maximum subarray sum(easy version)](https://ac.nowcoder.com/acm/contest/67743/D)

智乃最近学习了冒泡排序和最长子段和，所以她现在想把它们合并成一个新的算法。


众所周知，冒泡排序是一种通过交换相邻元素实现排序的算法，最长子段和是指从一个数组$a$中取出一段连续的**非空**数组区间$[l,r]$，最大化数组区间的和$\sum_{i=l}^{r}a_{i}$。  

现在智乃有一个长度大小为$N$的数组，数组中元素的值有正有负。她想要先**进行恰好**$K$**次相邻元素的交换操作，再求整个数组的最大子段和**。她想要让最后求出的最大子段和尽可能的大，请你帮助智乃算出她最终可能的最大子段和有多大。

第一行输入两个正整数$N,K(2\leq N \leq 10^3,0 \leq K \leq 1)$表示数组的长度，交换的次数。  

接下来一行$N$个整数，输入数组元素$a_i(-10^{9} \leq a_{i} \leq 10^{9})$表示数组元素的值。

**分析**

数据范围很小

可以跑个暴力$n^2$的做法

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67185266)

```C++
// NowCoder chino's bubble sort and maximum subarray sum(easy version)
// https://ac.nowcoder.com/acm/contest/67743/D 2024-02-07 13:07:55

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, k;
int maxSum(vector<int> a, int n) {
  int sum = a[0];
  int b = a[0];

  for (int i = 1; i < n; i++) {
    if (b > 0)
      b += a[i];
    else
      b = a[i];
    if (b > sum) sum = b;
  }
  return sum;
}
void solve() {
  cin >> n >> k;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  int ans = LLONG_MIN / 2;
  if (k == 1) {
    for (int i = 1; i < n; i++) {
      swap(a[i], a[i - 1]);
      ans = max(ans, maxSum(a, n));
      swap(a[i], a[i - 1]);
    }
  } else {
    ans = max(ans, maxSum(a, n));
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

## [GH-智乃的比较函数](https://ac.nowcoder.com/acm/contest/67743/G)

在c++标准库中，存在一个叫做std::sort的函数，它可以默认按照升序的方式排序，或者按照一种给定的自定义方式排序。


新手在使用自定义比较函数进行sort时，经常会因为没有遵守sort传入比较函数的约定而导致代码崩溃。  

具体来讲，使用sort时需要定义一个比较函数$cmp(x,y)$他表示比较在排序的过程中$x$的顺序是否严格小于$y$的顺序，如果$x$的顺序严格小于$y$的顺序，则$cmp(x,y)=1$，反之$cmp(x,y)=0$。  

新手在编写$cmp$函数时的一个易错点是在$x$和$y$的值相等时令$cmp(x,y)=1$，例如降序排序时将$x>y$写成了$x \geq y$。  


抛开c++语言的底层实现，这样其实已经产生了逻辑矛盾。因为当$x$和$y$的值相等时$cmp(x,y)=cmp(y,x)=1$，在调用约定中，它表示在排序中$x$的顺序严格小于$y$且$y$的顺序严格小于$x$，显然这里产生了逻辑矛盾。所以此时无论c++库函数执行出任何的结果都是可能的，一般来讲这将导致运行错误。

 

所以在编写$cmp(x,y)$时，一定要注意，当$x$和$y$的值相等时，应该令$cmp(x,y)=cmp(y,x)=0$，这表示通知排序函数，不能确定在排序中$x$的顺序严格小于$y$，同时也不能确定$y$的顺序严格小于$x$，当然，从逻辑上讲，这只有一种情况，就是$x=y$，并未产生任何逻辑矛盾。


现在有三个整形变量$a_{1},a_{2},a_{3}$（你可以认为这三个变量的值是int范围内任意的整数），告诉你$N$组$cmp(a_x,a_y)$的值，问是否产生了逻辑矛盾。

**分析**

通过观察题目可以发现到这是 一个良定的代数系统

然后我们判断一下 给定的条件是不是 良定的

我们可以跑两个闭包 然后判断冲不冲突

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67207282)

```C++
// NowCoder 智乃的比较函数(normal version)
// https://ac.nowcoder.com/acm/contest/67743/H 2024-02-07 14:57:16

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  std::cin >> n;
  std::bitset<4> st[4] = {};
  std::bitset<4> st2[4] = {};
  for (int i = 0; i < n; i++) {
    int x, y, z;
    cin >> x >> y >> z;
    if (z == 0) {
      st[x][y] = 1;
    } else
      st2[x][y] = 1;
  }
  for (int j = 1; j <= 3; j++) {
    for (int i = 1; i <= 3; i++) {
      if (st[i][j]) {
        st[i] |= st[j];
      }
    }
  }
  for (int j = 1; j <= 3; j++) {
    for (int i = 1; i <= 3; i++) {
      if (st2[i][j]) {
        st2[i] |= st2[j];
      }
    }
  }
  for (int i = 1; i <= 3; i++) {
    if (st2[i][i]) {
      cout << "No\n";
      return;
    }
  }
  for (int i = 1; i <= 3; i++)
    for (int j = 1; j <= 3; j++) {
      if (st[i][j] == 1 and st2[i][j] == 1) {
        cout << "No\n";
        return;
      }
    }
  cout<<"Yes\n";
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

## [J-智乃的相亲活动](https://ac.nowcoder.com/acm/contest/67743/J)

智乃举办了一个相亲活动，她邀请了$N$位男嘉宾和$M$位女嘉宾，现在男女嘉宾之间存在$K$对双向的好感关系。  

活动的第一步是选出若干位心动男/女嘉宾，按照如下的方式进行。  

每个人都首先自己各自独立的从自己有好感的异性中选出一个人，若某个人至少被一位异性选中，则ta成为心动男/女嘉宾。  

现在已知这$K$对双向的好感关系，若每个人选择异性的方式都为从自己有好感的异性中等概率随机选择，则心动男/女嘉宾数目的期望分别是多少？

**分析**

我们要算 期望

那么 一个女生选中的概率就是 $1- 所有男生都不选她$

同样 一个男生选中的概率就是 $1- 所有女生都不选他$



然后我们对应每一条边 进行对应的处理就好

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67203648)

```C++
// NowCoder 智乃的相亲活动
// https://ac.nowcoder.com/acm/contest/67743/J 2024-02-07 14:32:07

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
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
i64 MLong<0LL>::Mod = i64(1E9) + 7;
int n, m, k;
using mint = MLong<0LL>;
constexpr int N = 2e5 + 10;
mint out[N], in[N];
mint ans[N], ans1[N];
using pii = pair<int, int>;
template <class T, class U>
ostream &operator<<(ostream &os, const std::pair<T, U> &p) {
  os << "" << p.first << " " << p.second << endl;
  return os;
}

void solve() {
  vector<pii> op;
  cin >> n >> m >> k;

  for (int i = 1; i <= k; i++) {
    int v, w;
    cin >> v >> w;
    out[v] += 1;
    in[w] += 1;
    op.emplace_back(v, w);
  }
  for (int i = 1; i <= max(n, m); i++) {
    ans[i] = 1LL;
    ans1[i] = 1LL;
  }
  for (int i = 0; i < k; i++) {
    auto [v, w] = op[i];
    ans[w] *= (out[v] - 1LL);
    ans[w] /= (out[v]);
    ans1[v] *= (in[w] - 1LL);
    ans1[v] /= in[w];
  }
  mint an = m;
  for (int i = 1; i <= m; i++) {
    an -= ans[i];
  }
  mint ann = n;
  for (int i = 1; i <= n; i++) {
    ann -= ans1[i];
  }
  cout << "modint\n";
  cout << ann << " " << an << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [L-智乃的36倍数(easy version)_2024牛客寒假算法基础集训营3 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/67743/L)

智乃定义了一种运算$f$，它表示将两个正整数按照字面值从左到右拼接。例如$f(1,1)=11$，$f(114,514)=114514$。

现在智乃有一个大小为$N$的正整数数组$a$，第$i$个元素为$a_{i}$，现在他从中想选出两个正整数进行前后拼接，使得它们拼接后是一个$36$的倍数，问智乃有多少种可行的方案。

具体来讲，她想要知道有多少对有序数对$i,j(i\neq j)$满足$f(a_{i},a_{j})$是一个$36$的倍数。

第一行输入一个正整数$N(1\leq N \leq 1000)$数组的大小  

接下来输入一行$N$个正整数$a_{i}(1\leq a_{i}\leq 10)$，表示$a_{i}$的值。

**分析**

既然数据范围很小 ，那我们跑个$n^2$ 绰绰有余

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67179493)

```C++
// NowCoder 智乃的36倍数(easy version)
// https://ac.nowcoder.com/acm/contest/67743/L 2024-02-07 13:00:31

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n + 1);
  for (int i = 1; i <= n; i++) cin >> a[i];
  int ans = 0;
  for (int i = 1; i <= n; i++)
    for (int j = 1; j <= n; j++) ans += ((a[i] * 10 + a[j]) % 36 == 0);
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

## [M-智乃的36倍数(normal version)](https://ac.nowcoder.com/acm/contest/67743/M)

智乃定义了一种运算$f$，它表示将两个正整数按照字面值从左到右拼接。例如$f(1,1)=11$，$f(114,514)=114514$。

现在智乃有一个大小为$N$的正整数数组$a$，第$i$个元素为$a_{i}$，现在他从中想选出两个正整数进行前后拼接，使得它们拼接后是一个$36$的倍数，问智乃有多少种可行的方案。

具体来讲，她想要知道有多少对有序数对$i,j(i\neq j)$满足$f(a_{i},a_{j})$是一个$36$的倍数。

第一行输入一个正整数$N(1\leq N \leq 10^5)$数组的大小  

接下来输入一行$N$个正整数$a_{i}(1\leq a_{i}\leq 10^{18})$，表示$a_{i}$的值。

**分析**

注意到 $a_i$最多有 18 位

$a_i≡_{36}X ∈[0,35]$

那我们枚举 数的长度和 模数

可以 $O(18*18*36*36)$的解决

**code**

[代码查看 (nowcoder.com)](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67191247)

```C++
// NowCoder 智乃的36倍数(normal version)
// https://ac.nowcoder.com/acm/contest/67743/M 2024-02-07 13:25:24

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
int dp[19][36];
// 快速幂
int qmi(int a, int b, int p) {
  int res = 1 % p;
  while (b) {
    if (b & 1) res = res * a % p;
    a = a * a % p;
    b >>= 1;
  }
  return res;
}
template <typename... Args>
void debug(Args &&...args) {
  ((cerr << args << " "), ...), cerr << "\n";
}
void solve() {
  // 思路 位数和 模数
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  auto get_len = [&](int a) -> int {
    int res = 0;
    while (a) {
      res++;
      a /= 10;
    }
    return res;
  };
  for (int i = 0; i < n; i++) {
    int len = get_len(a[i]);
    dp[len][a[i] % 36]++;
  }
  int ans = 0;
  for (int i = 1; i <= 18; i++) {
    for (int j = 1; j <= 18; j++) {
      for (int _i = 0; _i < 36; _i++) {
        for (int _j = 0; _j < 36; _j++) {
          if ((_j + _i * qmi(10, j, 36)) % 36 == 0) {
            ans += dp[i][_i] * dp[j][_j];
          }
        }
      }
    }
  }
  for (int i = 0; i < n; i++) {
    int len = get_len(a[i]);
    if (((a[i] % 36) + qmi(10, len, 36) % 36 * (a[i] % 36)) % 36 == 0) ans--;
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

