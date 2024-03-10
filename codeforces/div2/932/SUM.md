#! https://zhuanlan.zhihu.com/p/685565142
## **C.** **Messenger in MAC**

在为硕士援助中心 Keftemerum 的学生提供的新信使中，计划进行更新，开发人员希望优化向用户显示的消息集。总共有 $n$ 条消息。每条消息都由两个整数 $a_i$ 和 $b_i$ 表征。读取编号为 $p_1, p_2, \ldots, p_k$ （ $1 \le p_i \le n$ ，所有 $p_i$ 都是 **不同**）的消息集所花费的时间由以下公式计算：

$$
\Large \sum_{i=1}^{k} a_{p_i} + \sum_{i=1}^{k - 1} |b_{p_i} - b_{p_{i+1}}|
$$

请注意，读取由编号为 $p_1$ 的 **一条** 消息组成的一组消息的时间等于 $a_{p_1}$ 。此外，读取空消息集的时间被视为 $0$ 。

用户可以确定他愿意在Messenger上花费的时间 $l$ 。信使必须告知用户该组消息的最大可能大小，该组消息的阅读时间不超过 $l$ 。请注意，消息集的最大大小可以等于 $0$ 。

流行的Messenger的开发人员未能实现此功能，因此他们要求您解决此问题。

**think**

转换以下就是选中 $k $ 个位置

权重等于
$$
\sum\limits _{i=1}^k a_{p_i}+{b_{p_1}}^{max}-{b_{p_2}}^{min}
$$
$1≤n≤2000$

一个朴素的想法，枚举第二维左右端点

在范围内 维护$\sum\limits_{i=1}^{k} a$ 最小值 

当时一度想到了是单调栈或者单调队列，但是我赛中完全没想到 单调队列怎么维护 前 $k$ 大

一度以为是 二维偏序 可持久化权值线段树 又想成 $n^2log^2$的二分套二分

完全想不明白

赛后才知道正解就是反悔贪心



关于单调队列怎么维护前 $k$ 大, 就是在进队的时候 将所有值都加和

然后在 队列大小大于 $k$ 的时候 弹出队首元素

这样就能维护前 $k$ 大了

**code**

https://codeforces.com/contest/1935/submission/249888227

```C++
// Codeforces - Codeforces Round 932 (Div. 2) C. Messenger in MAC
// https://codeforces.com/contest/1935/problem/C 2024-03-06 12:59:12

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
using pii = pair<int, int>;
void solve() {
  cin >> n >> m;
  vector<pii> a;
  for (int i = 0; i < n; i++) {
    int x, y;
    cin >> x >> y;
    a.emplace_back(x, y);
  }
  sort(all(a), [&](pii a, pii b) { return a.second < b.second; });
  int ans = 0;
  for (int i = 0; i < n; i++) {
    priority_queue<int> q;
    int cost = 0, cnt = 0;
    if (a[i].first <= m) {
      cost += a[i].first, cnt++, q.emplace(a[i].first), ans = max(ans, cnt);
      for (int j = i + 1; j < n; j++) {
        cost += a[j].second - a[j - 1].second + a[j].first;
        q.emplace(a[j].first);
        cnt++;
        while (cost > m and q.size()) {
          cost -= q.top();
          cnt--;
          q.pop();
        }
        ans = max(ans, cnt);
      }
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