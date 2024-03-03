# [Codeforces Round 921 (Div. 2)](https://codeforces.com/contest/1925)[A-D]

[Dashboard - Codeforces Round 921 (Div. 2) - Codeforces](https://codeforces.com/contest/1925)

阿三的场很难评哈哈

赛中出了D 没出C，鉴定为 不会思维

## A. We Got Everything Covered!

给定两个正整数 $n$ 和 $k$ 。

您的任务是找到一个字符串 $s$ ，使得使用前 $k$ 个小写英文字母组成的所有可能的长度为 $n$ 的字符串都作为 $s$ 的子序列出现。

如果有多个答案，则打印长度最小的一个。如果仍有多个答案，您可以打印其中任何一个。

**注意：** 如果可以通过从 $b$ 中删除一些（可能为零）字符而不改变剩余字符的顺序来获得 $a$ ，则字符串 $a$ 称为另一个字符串 $b$ 的子序列。

**那么我们想如果要构成一个字符串需要什么呢**

需要有所有的字母对吧(好像是废话)

那我们先保证有第一个字母

然后第一个字母后面有第二个字母 等等

以此类推

目标串的某一位 是前k 个字母

那就是 

$[1,k]$ $[1,k]$ $[1,k]$........ 一共$n$ 个

这样可以保证 每一个字母后面可以取任意的 字符串

**code**

https://codeforces.com/contest/1925/submission/243557501

```C++
// Codeforces - Codeforces Round 921 (Div. 2) A. We Got Everything Covered!
// https://codeforces.com/contest/1925/problem/0 2024-01-27 22:45:22

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, k;
void solve() {
  cin >> n >> k;
  string s;
  for (int j = 0; j < n; j++) {
    for (char a = 'a'; a < 'a' + k; a++) {
      s += a;
    }
  }
  cout << s << "\n";
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

## B. A Balanced Problemset?

杰伊成功地创造了一个难度为 $x$ 的问题，并决定将其作为 Codeforces 第 921 回的第二个问题。

但 Yash 担心这个问题会让比赛变得非常不平衡，协调员会拒绝接受它。因此，他决定把它分解成一个由 $n$ 个子问题组成的问题集，使得所有子问题的难度都是一个正整数，并且它们的总和等于 $x$。

协调人阿列克谢将问题集的平衡定义为问题集中所有子问题难度的 [GCD](https://en.wikipedia.org/wiki/Greatest_common_divisor)。

求如果 Yash 最佳地选择子问题的难度，他能达到的最大平衡。

**分析**

我们贪心的想一下，问题中答案肯定越大越好对吧

在尽量大的同时 使得所有子问题的 难度可以整除答案

那么这个答案就是 每个难度的因子

所以 也就是 $x$的因子

那么我们就枚举 $x$ 的因子

找出一个 最大而且能够分成$\ge n$ 的方案

那我们就枚举一下就可以解决了

分析一下时间复杂度

 $t$$ ( $$1\leq t\leq 10^3$ )表示测试用例的数量。

两个整数 $x$ ( $1\leq x\leq 10^8$ ) 和 $n$ ( $1\leq n\leq x$ )。

分解因子的复杂度 $O(\sqrt n)$

总时间复杂度为 $O(T \sqrt n)$ $\le$ $10^7$

可以通过

**code**

https://codeforces.com/contest/1925/submission/243574068

```C++
// Codeforces - Codeforces Round 921 (Div. 2) B. A Balanced Problemset?
// https://codeforces.com/contest/1925/problem/B 2024-01-27 22:49:07

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int x, n;
vector<int> divide2(int x) {
  vector<int> ans;
  for (int i = 2; i <= x / i; i++)
    if (x % i == 0) {
      ans.push_back(i);
      if (i * i != x) ans.push_back(x / i);
    }
  if (x > 1) ans.push_back(x);
  return ans;
}
void solve() {
  cin >> x >> n;
  auto it = divide2(x);
  sort(all(it));
  reverse(all(it));
  for (auto _x : it) {
    if (x / _x >= n) {
      cout << _x << "\n";
      return;
    }
  }
  cout << "1\n";
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

## C. Did We Get Everything Covered?

您将获得两个整数 $n$ 和 $k$ 以及一个字符串 $s$ 。

您的任务是检查使用前 $k$ 个小写英文字母组成的所有可能的长度为 $n$ 的字符串是否作为 $s$ 的子序列出现。如果答案是否定的，您还需要打印一个长度为 $n$ 的字符串，该字符串可以使用前 $k$ 个小写英文字母组成，该字母不会作为 $s$ 的子序列出现。

如果有多个答案，您可以打印其中任何一个。

**注意：** 如果可以通过从 $b$ 中删除一些(可能为零)字符而不改变剩余字符的顺序来获得 $a$ ，则字符串 $a$ 称为另一个字符串 $b$ 的子序列。



**分析**

这个题和 $A$ 题很像

> quote A:
>
> $[1,k]$ $[1,k]$ $[1,k]$........ 一共n 个
>
> 这样可以保证 每一个字母后面可以取任意的 字符串

然后我们分析一下 $C$ 给定的字符串

看看有没有 $n $个 含有字母 $[1,k]$的 区间

如果没有的话就是$NO$

构造答案时选取 最后一个填满 字母 $[1,k]$ 的字母

这样可以使得答案最优

**code**

[Submission #243739750 - Codeforces](https://codeforces.com/contest/1925/submission/243739750)

```C++
// Codeforces - Codeforces Round 921 (Div. 2) C. Did We Get Everything Covered?
// https://codeforces.com/contest/1925/problem/C 2024-01-28 17:05:28

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, k, m;
template <typename T>
T read() {
  T x;
  cin >> x;
  return x;
}
void solve() {
  cin >> n >> k >> m;
  string out, s = read<string>();
  set<char> now;
  int res = 0;
  for (auto it : s) {
    now.insert(it);
    if (now.size() == k) res++, now.clear(), out += it;
  }
  if (res < n) {
    cout << "NO\n";
    for (char a = 'a'; a < 'a' + k; a++)
      if (!now.count(a))
        return (void)(cout << out + string(n - res, a) << "\n");
  } else
    cout << "YES\n";
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

## D. Good Trip

一个班有 $n$ 个孩子，其中 $m$ 对是朋友。第 $i$ 对好友的友谊值为 $f_i$ 。

老师必须进行 $k$ 次短途旅行，并且对于每次短途旅行，她都会随机、等概率且独立地选择一对孩子。如果选择了一对作为朋友的孩子，则在随后的所有游览中，他们的友谊值都会增加 $1$ (老师可以多次选择一对孩子)。一对非朋友的友谊值被视为 $0$ ，并且对于后续的旅行不会改变。

求为远足选择的所有 $k$ 对(在选择时)的友谊值总和的预期值。可以证明，这个答案总是可以表示为分数 $\dfrac{p}{q}$ ，其中 $p$ 和 $q$ 是互质整数。计算 $p\cdot q^{-1} \bmod (10^9+7)$ 。



我们直接用期望来算

这样所有关系都是等价的

设$P$ 是老师 命中朋友的 概率

可以证明 老师命中的期望是固定的

> 我们设
>
> $cnt = n * (n - 1) / 2$
>
> $add = (k - 1) * k / 2$
>
> $p = m / cnt$
>
> 答案就是
>
> $(p * add + sum * k) / cnt $

**code**

[Submission #243644428 - Codeforces](https://codeforces.com/contest/1925/submission/243644428)

```C++
// Codeforces - Codeforces Round 921 (Div. 2) D. Good Trip
// https://codeforces.com/contest/1925/problem/D 2024-01-28 00:08:34

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m, k;
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
void solve() {
  cin >> n >> m >> k;
  MLong<0LL> sum = 0;
  for (int i = 1; i <= m; i++) {
    int _a, _b, _f;
    cin >> _a >> _b >> _f;
    sum += _f;
  }
  MLong<0LL> cnt = n * (n - 1LL) / 2LL;
  MLong<0LL> add = (k - 1) * k / 2;
  MLong<0LL> p = m / cnt;
  if (m)
    cout << (p * add + sum * k) / cnt << "\n";
  else
    cout << 0 << "\n";
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

