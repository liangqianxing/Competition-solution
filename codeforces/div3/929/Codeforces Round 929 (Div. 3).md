# E. Turtle vs. Rabbit Race: Optimal Trainings

艾萨克开始了他的训练。有 $n$ 条跑道可供使用， $i$ 条跑道（ $1 \le i \le n$ ）由 $a_i$ 个等长的部分组成。

给定一个整数 $u$ ( $1 \le u \le 10^9$ )，完成每一段都能使艾萨克的能力提高一个特定值，具体描述如下：

- 完成 $1$ （ $1$ \-st）部分会使艾萨克的成绩提高 $u$ 。
- 完成 $2$ （nd）部分会使艾萨克的能力提高 $u-1$ 。
- 完成 $3$ \-rd部分会使艾萨克的成绩提高 $u-2$ 。
- $\ldots$
- 完成 $k$ \-th部分（ $k \ge 1$ ）会使艾萨克的成绩提高 $u+1-k$ 。 $u+1-k$ 的值可以是负数，这意味着完成额外的部分会降低艾萨克的成绩）。

您还得到了一个整数 $l$ 。您必须选择一个整数 $r$ ，使 $l \le r \le n$ 和艾萨克都能完成赛道 $l, l + 1, \dots, r$ 的**个**段（即总共完成 $l \le r \le n$ 个段）。(即总共完成 $\sum_{i=l}^r a_i = a_l + a_{l+1} + \ldots + a_r$ 节）。

请回答下列问题：你所能选择的最佳 $r$ 是什么，能最大限度地提高艾萨克的成绩？

如果有多个 $r$ 可以最大限度地提高艾萨克的成绩，请选出**小的** $r$ 。

为了增加难度，你需要回答 $q$ 个不同值的 $l$ 和 $u$ 。

**分析**

给出两个整数 $l$ 和 $u$

设$F(u,x)=\sum \limits ^u _{u-x+1}$

要求出 $r$ 使得 $f(l,r)=\sum \limits_{i=l}^r a_i$

要求$F(u,f(l,r))$ 最大

我们先来观察一下 函数的性质

题目给定 $l,u$ 设这两位为常量

那么 $f(l,r)$ 随着 $r$ 的增加而增加

$f(l,r)$是关于$r$的增函数

因为 $F(u,x)=\sum \limits ^u _{u-x+1}$

当$u-x_0+1=0$ 时

$F(u,x_0)$ 为 $F$ 的最大值

当 $x<u$ 时 , $F$ 随着 $x$的增大而增大

当$x>u$ 时 ,  $F$ 随着 $x$的增大而减小

所以$F$是关于 $x$ 的一个凸函数

所以$F$是关于  $f(l,r)$ 的一个凸函数

因为$f(l,r)$是关于$r$的增函数

所以$F$是关于  $r$ 的一个凸函数

所以我们三分找最大值

二分找函数取最大值时变量的最小值

**code**

[Submission #248692936 - Codeforces](https://codeforces.com/contest/1933/submission/248692936)

```C++
// Codeforces - Codeforces Round 929 (Div. 3) E. Turtle vs. Rabbit Race: Optimal
// Trainings https://codeforces.com/contest/1933/problem/E 2024-02-28 12:41:52

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, q;

void solve() {
  cin >> n;
  vector<int> a(n + 1), s(n + 1);
  for (int i = 1; i <= n; i++) cin >> a[i], s[i] = a[i] + s[i - 1];
  cin >> q;
  while (q--) {
    int l, r, u;
    cin >> l >> u;
    int L = l, R = n;
    int ans = 0;
    auto check = [&](int r) -> int {
      int X = s[r] - s[l - 1];
      return (u + u - X + 1) * (X) / 2LL;
    };
    while (L < R) {
      int lmid = L + (R - L) / 3LL;
      int rmid = R - (R - L) / 3LL;
      if (check(lmid) <= check(rmid)) {
        ans = rmid;
        L = lmid + 1;
      } else {
        ans = lmid;
        R = rmid - 1;
      }
    }
    L = l;
    int _ans = R;
    while (L <= R) {
      int mid = L + R >> 1;
      if (check(mid) == check(ans)) {
        _ans = mid;
        R = mid - 1;
      } else {
        L = mid + 1;
      }
    }
    cout << _ans << " ";
  }
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

