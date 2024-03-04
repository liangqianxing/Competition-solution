[toc]

# [G-小红的子序列权值和](https://ac.nowcoder.com/acm/contest/76133/G)

小红定义一个数组的权值为：数组所有元素乘积的因子数量。例如，\[1,2,3\]的权值为 4。

现在小红拿到了一个数组，她想求出数组中所有非空子序列的权值之和。你能帮帮她吗？由于答案过大，请对$10^9+7$取模。  
定义数组的子序列为：从左到右选择若干个元素（可以不连续）组成的数组，例如\[1,2,3,2\]的子序列有\[2,2\]等。因此，一个大小为$n$的数组有恰好$2^n-1$个子序列。

第一行输入一个正整数$n$，代表数组大小。  
第二行输入$n$个正整数$a_i$，代表数组的元素。  
$1\leq n \leq 10^5$  
$1\leq a_i \leq 3$

**think**

任意数组的乘积 $X$ 可以表示为 
$$
X=1^\alpha 2^\beta 3^\gamma
$$
设 $f(\beta,\gamma)$ 是 $x$ 的权值,根据 加法原理, 选择每一个因子的情况有 $cnt+1$种

根据乘法原理 总的方案数就是 全都乘起来

那么
$$
f(\beta,\gamma)=(\beta+1)(\gamma+1)
$$
另外我们可以发现,选择1 不会对答案造成更改,但是会让答案的次数加一

对于一个数组有 $\beta$ 个$2$ ,有 $\gamma$ 个$3$  

总共的答案就是
$$
2^\alpha\times\sum \limits ^\beta _{j=0} \sum\limits ^\gamma _{k=0}\binom{\beta}{j}\binom{\gamma}{k}f(j,k)
$$

$$
2^\alpha\times\sum \limits ^\beta _{j=0} \sum\limits ^\gamma _{k=0}\binom{\beta}{j}\binom{\gamma}{k}(j+1)(k+1)
$$

$$
2^\alpha\times{\sum \limits ^\beta _{j=0} \sum\limits ^\gamma _{k=0}\binom{\beta}{j}(j+1)\times\binom{\gamma}{k}(k+1)}
$$

$$
2^\alpha\times{\sum \limits ^\beta _{j=0} \binom{\beta}{j}(j+1)\times\sum\limits ^\gamma _{k=0}\binom{\gamma}{k}(k+1)}
$$

由此可$O(n)$通过

**code**

[代码查看 ](https://ac.nowcoder.com/acm/contest/view-submission?submissionId=67760440)

```C++
// NowCoder 小红的子序列权值和（hard）
// https://ac.nowcoder.com/acm/contest/76133/G 2024-03-04 10:27:34

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
struct CNM {
  int N = 100005, mod;
  CNM(int n = 1e5 + 10, int p = 1e9 + 7) : N(n), mod(p) {
    jie.resize(n + 1);
    inv.resize(n + 1);
    invjie.resize(n + 1);
    main_init();
  }
  int get_inv(int a, int b, int p) {
    int res = 1 % p;
    while (b) {
      if (b & 1) res = res * a % p;
      a = a * a % p;
      b >>= 1;
    }
    return res;
  }
  vector<int> jie, invjie, inv;
  void main_init() {
    int n = N;
    jie[0] = inv[1] = 1;
    for (int i = 1; i <= n; i++) jie[i] = jie[i - 1] * i % mod;
    invjie[n] = get_inv(jie[n], mod - 2, mod);
    for (int i = n - 1; ~i; i--) invjie[i] = invjie[i + 1] * (i + 1) % mod;
    for (int i = 2; i <= n; i++) inv[i] = (mod - mod / i) * inv[mod % i] % mod;
  }
  int C(int n, int m) {
    return n >= m && m >= 0ll ? jie[n] * invjie[m] % mod * invjie[n - m] % mod
                              : 0ll;
  }
} CNM;
// 快速幂
int qmi(int a, int b, int p = 1e9 + 7) {
  int res = 1 % p;
  while (b) {
    if (b & 1) res = res * a % p;
    a = a * a % p;
    b >>= 1;
  }
  return res;
}
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
i64 MLong<0LL>::Mod = 1e9 + 7;
using mint = MLong<0LL>;
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  array<int, 4> cnt;
  for (int i = 0; i < 4; i++) cnt[i] = 0;
  for (auto it : a) cnt[it]++;
  mint ans = 1;
  int beta = 0, gamma = 0;
  for (int i = 0; i <= cnt[2]; i++) beta += CNM.C(cnt[2], i) * (i + 1);
  for (int i = 0; i <= cnt[3]; i++) gamma += CNM.C(cnt[3], i) * (i + 1);
  if (cnt[2]) ans *= beta;
  if (cnt[3]) ans *= gamma;
  ans *= qmi(2, cnt[1]);
  ans -= 1;
  cout << ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

