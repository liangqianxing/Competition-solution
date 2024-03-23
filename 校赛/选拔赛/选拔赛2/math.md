# XJU-GPLT2-L3B

曾经发明了脑洞治疗仪&超能粒子炮的发明家SHTSC又公开了他的新发明：超能粒子炮·改--一种可以发射威力更加 强大的粒子流的神秘装置。超能粒子炮·改相比超能粒子炮，在威力上有了本质的提升。它有三个参数n，k。它会 向编号为0到k的位置发射威力为C(n,k) mod 2333的粒子流。现在SHTSC给出了他的超能粒子炮·改的参数，让你求 其发射的粒子流的威力之和模2333。

$t = 100000,n,k \le 10^{18}$

**think**

求
$$
\begin{align}
&\sum\limits_{i=0}^k\binom{n}{i}\pmod{p}\\
我们设f(n,k)=&\sum\limits_{i=0}^k\binom{n}{i}\pmod{p}\\
等价于f(n,k)=&\sum\limits_{i=0}^k\binom{n\%p}{i\%p}\binom{n/p}{i/p}\\
我们观察\binom{n/p}{i/p}，&发现这是一个很整除分块的式子\\
选中p作为块长\\
对于 n\ge p \and k\ge p的情况&分析 i\in [0,\lfloor \frac k p\rfloor\times p)\\
[0,p-1] 可以取p次\\
f(n,k)=&\binom{n/p}{0}\sum\limits_{i=0}^{p-1}\binom{n\%p}{i\%p}+\binom{n/p}{1}\sum\limits_{i=0}^{p-1}\binom{n\%p}{i\%p}+\dots+\binom{n/p}{k/p}\sum\limits_{i=0}^{k\%p}\binom{n\%p}{i\%p}\\
注意到上方的i都在[0,p-1]内,于是等同于\\
&f(n,k)=\binom{n/p}{0}\sum\limits_{i=0}^{p-1}\binom{n\%p}{i}+\binom{n/p}{1}\sum\limits_{i=0}^{p-1}\binom{n\%p}{i}+\dots+\binom{n/p}{k/p}\sum\limits_{i=0}^{k\%p}\binom{n\%p}{i}\\
&f(n,k)=\sum\limits_{j=0}^{k/p-1}\binom{n/p}{j}\sum\limits_{i=0}^{p-1}\binom{n\%p}{i}+\binom{n/p}{k/p}\sum\limits_{i=0}^{k\%p}\binom{n\%p}{i}\\
&f(n,k)=f(n/p,k/p-1)\times f(n\%p,p-1)+f(n\%p,k\%p)\\
第一部分我们用lucas求, 后两部分我们预处理O(n^2)\\
O (plog_pn)+O(n^2)+O(n^2)\\
由此我们解决了这道题
\end {align}
$$
**code**

```C++
// NowCoder L3-2(300pts)
// https://ac.nowcoder.com/acm/contest/77713/N 2024-03-24 02:34:38

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, k;
constexpr int mod = 2333, N = 2333, P = 2333;
int c[N + 5][N + 5], f[N + 5][N + 5];
void init() {
  f[0][0] = c[0][0] = 1;
  for (int i = 1; i <= N; i++) c[i][i] = c[i][0] = 1;
  for (int i = 1; i <= N; i++)
    for (int j = 1; j < i; j++) c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % P;
  for (int i = 1; i <= N; i++) f[i][0] = 1;
  for (int i = 0; i <= N; i++)
    for (int j = 1; j <= N; j++) f[i][j] = (c[i][j] + f[i][j - 1]) % P;
}
int C(int n, int m) { return c[n][m]; }
int lucas(int n, int m) {
  if (!m) return 1;
  if (n == m) return 1;
  if (n < m) return 0;
  return (c[n % P][m % P] * lucas(n / P, m / P)) % P;
}
int F(int n, int k) {
  if (k < 0) return 0;
  if (not n) return 1;
  if (not k) return 1;
  if (n < P && k < P) return f[n][k];
  return (F(n / P, k / P - 1) * f[n % P][P - 1] % P +
          lucas(n / P, k / P) * f[n % P][k % P] % P) %
         P;
}
void solve() {
  cin >> n >> k;
  cout << F(n, k) << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  init();
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

