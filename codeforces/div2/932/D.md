[TOC]

## **D. Exam in MAC**

硕士辅导中心已公布入学考试，考试内容如下。

给候选者一个大小为 $n$  的集合 $s$ 和一些奇怪的整数 $c$ 。对于这个集合，需要计算整数对 $(x, y)$ 的数量，使得 $0 \leq x \leq y \leq c$ 、 $x + y$ **不**包含在集合 $s$ 中，并且 $y - x$ **也不包含在集合 $s$ 中** 包含在集合 $s$ 中。

您的朋友想要进入中心。帮助他通过考试！

**THINK**

先求出 总共的 对数

一共有 $c+1$ 个数
$$
\begin {align}
\forall x<y  \qquad&(x,y)共有 \frac {c(c+1)}2 对\\
\forall x=y \qquad &(x,y)共有 c+1 对\\
\forall x\le y \qquad &(x,y)共有\frac {(c+1)(c+2)} 2对
\end {align}
$$

$$
\begin {align}
&我们发现 对于 y+z=x\\&y\in[0,\lfloor\frac x 2\rfloor] \qquad z\in [\lceil\frac x 2\rceil,x]\\
&共有\lceil \frac x 2 \rceil对
\end {align}
$$

$$
\begin {align}
&我们发现 对于 y-z=x\\  &z\in [0,c-x]\\
&共有c-x+1对
\end {align}
$$

$$
\begin{align}
我们发现一对最多被重复计算两次\\
因为最多是和计算一次,差计算一次\\
所以我们要将重复减去的贡献加回来\\
观察发现相加是奇数的对,相减也是奇数\\
相加是偶数的对,相减也是偶数\\
因为相加和相减的取值是[0,c]\\
所以一定会重复计算所有可以重复的情况\\
对于奇数和偶数都是\frac {cnt(cnt+1)} 2 次
由此便可以解决
\end{align}
$$

**code**

https://codeforces.com/contest/1935/submission/249889750

```C++
// Codeforces - Codeforces Round 932 (Div. 2) D. Exam in MAC
// https://codeforces.com/contest/1935/problem/D 2024-03-06 13:14:29

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, c;
void solve() {
  cin >> n >> c;
  int ans = (c + 1) * (c + 2) / 2;
  vector<int> s(n);
  for (auto &i : s) cin >> i;
  for (auto it : s) {
    ans -= c - it + 1;
    ans -= it / 2 + 1;
  }
  array<int, 2> mod = {};
  for (auto it : s) mod[it % 2]++;
  for (int i = 0; i < 2; i++) {
    ans += mod[i] * (mod[i] + 1) / 2;
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

