# [Codeforces Round 920 (Div. 3)](https://codeforces.com/contest/1921)

**进算竞小群加我**

[Problem - A - Codeforces](https://codeforces.com/contest/1921/problem/A)

## A. Square

正面积(严格大于 $0$ )的正方形位于坐标平面上，其边平行于坐标轴。您会以随机顺序获得其角点的坐标。你的任务是求出正方形的面积。

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \le t \le 100$ ) — 测试用例的数量。以下是测试用例的描述。

每个测试用例包含四行，每行包含两个整数 $x_i, y_i$( $-1000\le x_i, y_i\le 1000$)，即正方形角点的坐标。

保证存在一个正方形，其边平行于坐标轴，面积为正(严格大于 $0$)，角在给定点处。

对于每个测试用例，打印一个整数，即正方形的面积。



怎么求一个 正方形的面积，那就是 边长乘边长对吧

```c++
// Codeforces - Codeforces Round 920 (Div. 3) A. Square
// https://codeforces.com/contest/1921/problem/A 2024-01-15 22:35:58
void solve() {
  using pii = pair<int, int>;
  vector<pii> a;
  for (int i = 0; i < 4; i++) {
    int x, y;
    cin >> x >> y;
    a.emplace_back(x, y);
  }
  sort(all(a));  // 排序先按第一关键字 排序，这样可以确保a[0]和a[1] 是同一个高度的
  int Len = abs(a[0].second - a[1].second);
  cout << Len * Len << "\n";
}
```

****

[Problem - B - Codeforces](https://codeforces.com/contest/1921/problem/B)

## B. Arranging Cats

为了验证关于猫的假设，科学家们必须以特定的方式把猫放在盒子里。当然，他们希望尽快验证假设并发表一篇轰动性的文章，因为他们太沉迷于下一个关于手机电量的假设了。

科学家们有 $n$ 个箱子，猫可以坐在里面，也可以不坐在里面。盒子的当前状态用序列 $b_1, \dots, b_n$ 表示：如果 $i$ 号盒子里有猫，则为 $b_i = 1$ ，否则为 $b_i = 0$ 。

幸运的是，猫的无限量生产已经确定，因此科学家们可以在一天内完成以下操作之一：

- 取一只新猫并将其放入盒子中（对于 $i$ 中的 $b_i = 0$ ，指定 $b_i = 1$ ）。
- 从盒子中取出一只猫，让它退休（对于 $i$ 中的 $b_i = 1$ ，指定 $b_i = 0$ ）。
- 将一只猫从一个盒子移到另一个盒子（对于 $i, j$ 中的 $b_i = 1, b_j = 0$ ，指定 $b_i = 0, b_j = 1$ ）。

我们还发现，有些盒子里马上就装满了猫。因此，科学家们知道猫在盒子中的初始位置 $s_1, \dots, s_n$ 和期望位置 $f_1, \dots, f_n$ 。

由于大量的文书工作，科学家们没有时间来解决这个问题。为了科学起见，请帮助他们，并指出检验假设所需的最少天数。

**输出**

对于每个测试用例，在单独一行上输出一个整数--从初始位置获得期望位置所需的最小操作数。可以证明解总是存在的。

**思路**

这个题我们用 贡献的 思路来 做一下

1. 如果将 一个 0 转换成 1 那就对答案产生了 1 个的 贡献
2. 如果将 一个 1 转换成 0 那就对答案产生了 1 个的 贡献
3. 如果将 一个 1 移动到另一个位置 那就对答案产生了 1 个的 贡献

说明 3 操作 可以以 1 的贡献 同时完成 1 2两次 操作

那么我们就优先 选择 3 操作,一共 可以操作 $min(1操作 个数，2操作个数)$次

剩下的 一定全是 1 操作 或者是 2 操作 ，否则 还能继续 3操作

那么操作次数就是$ min(1操作 个数，2操作个数)+ max(1操作 个数，2操作个数)-min(1操作 个数，2操作个数)$

最终答案就是 $max(1操作 个数，2操作个数)$

```C++
// Codeforces - Codeforces Round 920 (Div. 3) B. Arranging Cats
// https://codeforces.com/contest/1921/problem/B 2024-01-15 22:38:15
int n, m;
void solve() {
  cin >> n;
  string s, f;
  cin >> s >> f;
  int a = 0, b = 0;
  for (int i = 0; i < n; i++) {
    if (s[i] == '0' and f[i] == '1') a++;
    if (s[i] == '1' and f[i] == '0') b++;
  }
  cout << max(a, b) << "\n";
}

```

[Problem - C - Codeforces](https://codeforces.com/contest/1921/problem/C)

## C. Sending Messages

斯捷潘是个大忙人。今天，他需要在 $m_1, m_2, \dots m_n$ ( $m_i< m_{i + 1}$ ) 时刻发送 $n$ 条信息。不幸的是，在 $0$ 时刻，他的手机只剩下 $f$ 单位的电量。在 $0$ 时刻，手机处于开机状态。

每开机一个单位，手机就会损失 $a$ 个单位的电量。此外，斯捷潘可以随时关闭手机，稍后再打开。这一操作每次消耗 $b$ 个单位的能量。考虑到开机和关机是瞬时的，因此您可以在 $x$ 时刻开机，并在同一时刻发送信息，反之亦然，在 $x$ 时刻发送信息，并在同一时刻关闭手机。

如果在任何时刻电量降至 $0$ （变为 $\le 0$ ），则无法在该时刻发送信息。

由于所有信息对斯捷潘都非常重要，他想知道是否可以在不给手机充电的情况下发送所有信息。

**思路**

在一个时刻 斯捷潘 有两种选择，一种是 关机 一种是 不关机

那么 这两种操作的状态 不会影响到 发送下一个 信息之后的状态

也就是 单个操作 只影响 当前 到下一个信息之间的 状态

那么我们可以进行贪心

开机消耗的电量是 $a*time$   

关机再开机消耗的电量是 $b$ 

我们就取最小的一个就好

**code**

```c++
// Codeforces - Codeforces Round 920 (Div. 3) C. Sending Messages
// https://codeforces.com/contest/1921/problem/C 2024-01-15 22:41:23
int n, m;
void solve() {
  int n, f, a, b;
  cin >> n >> f >> a >> b;
  vector<int> m(n + 1);
  for (int i = 1; i <= n; i++) cin >> m[i];
  int ans = 0;
  for (int i = 1; i <= n; i++) {
    ans += min(b, (m[i] - m[i - 1]) * a);
  }
  if (ans < f)
    cout << "YES\n";
  else
    cout << "NO\n";
}

```

[Problem - D - Codeforces](https://codeforces.com/contest/1921/problem/D)

## D. Very Different Array

Petya 有一个由 $n$ 个整数组成的数组 $a_i$ 。他的弟弟 Vasya 很羡慕，决定自己也做一个 $n$ 个整数的数组。

为此，他找到了 $m$ 个整数 $b_i$ ( $m\ge n$ )，现在他想从中选择一些 $n$ 个整数并按一定顺序排列，得到一个长度为 $n$ 的数组 $c_i$ 。

为了避免与哥哥的数组相似，Vasya 希望自己的数组与 Petya 的数组尽可能不同。具体来说，他希望总差 $D = \sum_{i=1}^{n} |a_i - c_i|$ 越大越好。

请帮助 Vasya 找出他能得到的最大差值 $D$ 。



**思路**

这个题我当时 是打表过的

先讲一下 正常的思路

![f5c3b0c7e324919531560f473b885127.jpg](https://cdn.acwing.com/media/article/image/2024/01/18/215908_c3f9dcafb5-f5c3b0c7e324919531560f473b885127.jpg) 

下面再讲一下 打表的思路

**code**

```C++
// Codeforces - Codeforces Round 920 (Div. 3) D. Very Different Array
// https://codeforces.com/contest/1921/problem/D 2024-01-18 16:20:25

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n >> m;
  vector<int> a(n), b(m);
  for (auto &i : a) cin >> i;
  for (auto &i : b) cin >> i;
  int res = 0;
  map<int, vector<vector<int>>> mp;
  sort(all(a)), sort(all(b));
  do {
    int ans = 0;
    for (int i = 0; i < n; i++) ans += abs(a[i] - b[i]);
    if (ans >= res) {
      res = ans;
      mp[ans].emplace_back(b);
    }
  } while (next_permutation(all(b)));
  for (auto i : a) cout << i << " ";
  cout << "\n";
  for (auto it : mp[res]) {
    for (auto i : it) {
      cout << i << " ";
    }
    cout << "\n";
  }
  cout << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << std::setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

我们可以观察 哪些情况最优

我们 可以发现 一个规律 

一定是 最大的$X $ 个 b 和 最小的 $X$  个 a 配对

一定是 最小的$X $ 个 b 和 最大 $X$  个 a 配对

那么 这个 $X$  是什么呢 ，我们可以枚举一遍

**输出**

```C++
1 2 4 6 
5 7 1 2 3 3 
5 7 2 1 3 3 
7 5 1 2 3 3 
7 5 2 1 3 3 

1 1 1 
1 1 1 1 

1 2 3 4 5 
3 4 5 1 2 
3 4 5 2 1 
3 5 4 1 2 
3 5 4 2 1 
4 3 5 1 2 
4 3 5 2 1 
4 5 1 2 3 
4 5 1 3 2 
4 5 2 1 3 
4 5 2 3 1 
4 5 3 1 2 
4 5 3 2 1 
5 3 4 1 2 
5 3 4 2 1 
5 4 1 2 3 
5 4 1 3 2 
5 4 2 1 3 
5 4 2 3 1 
5 4 3 1 2 
5 4 3 2 1 

5 8 
10 2 5 7 8 8 
10 2 5 8 7 8 
10 2 5 8 8 7 
10 2 7 5 8 8 
10 2 7 8 5 8 
10 2 7 8 8 5 
10 2 8 5 7 8 
10 2 8 5 8 7 
10 2 8 7 5 8 
10 2 8 7 8 5 
10 2 8 8 5 7 
10 2 8 8 7 5 

1 4 
6 9 
9 6 

4 6 8 10 
9 10 1 3 6 8 
9 10 1 3 8 6 
9 10 3 1 6 8 
9 10 3 1 8 6 
10 9 1 3 6 8 
10 9 1 3 8 6 
10 9 3 1 6 8 
10 9 3 1 8 6 

2 5 6 
9 1 2 7 7 
9 2 1 7 7 

3 6 7 9 10 
9 9 2 3 5 
9 9 2 5 3 
9 9 3 2 5 
9 9 3 5 2 
9 9 5 2 3 
9 9 5 3 2 

3 
10 1 1 2 5 7 
10 1 1 2 7 5 
10 1 1 5 2 7 
10 1 1 5 7 2 
10 1 1 7 2 5 
10 1 1 7 5 2 
10 1 2 1 5 7 
10 1 2 1 7 5 
10 1 2 5 1 7 
10 1 2 5 7 1 
10 1 2 7 1 5 
10 1 2 7 5 1 
10 1 5 1 2 7 
10 1 5 1 7 2 
10 1 5 2 1 7 
10 1 5 2 7 1 
10 1 5 7 1 2 
10 1 5 7 2 1 
10 1 7 1 2 5 
10 1 7 1 5 2 
10 1 7 2 1 5 
10 1 7 2 5 1 
10 1 7 5 1 2 
10 1 7 5 2 1 
10 2 1 1 5 7 
10 2 1 1 7 5 
10 2 1 5 1 7 
10 2 1 5 7 1 
10 2 1 7 1 5 
10 2 1 7 5 1 
10 2 5 1 1 7 
10 2 5 1 7 1 
10 2 5 7 1 1 
10 2 7 1 1 5 
10 2 7 1 5 1 
10 2 7 5 1 1 
10 5 1 1 2 7 
10 5 1 1 7 2 
10 5 1 2 1 7 
10 5 1 2 7 1 
10 5 1 7 1 2 
10 5 1 7 2 1 
10 5 2 1 1 7 
10 5 2 1 7 1 
10 5 2 7 1 1 
10 5 7 1 1 2 
10 5 7 1 2 1 
10 5 7 2 1 1 
10 7 1 1 2 5 
10 7 1 1 5 2 
10 7 1 2 1 5 
10 7 1 2 5 1 
10 7 1 5 1 2 
10 7 1 5 2 1 
10 7 2 1 1 5 
10 7 2 1 5 1 
10 7 2 5 1 1 
10 7 5 1 1 2 
10 7 5 1 2 1 
10 7 5 2 1 1 
```



**打表法code**

```C++
// Codeforces - Codeforces Round 920 (Div. 3) D. Very Different Array
// https://codeforces.com/contest/1921/problem/D 2024-01-15 22:45:41

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n >> m;
  vector<int> a(n + 1);
  vector<int> b(m + 1);
  vector<int> s(m + 1);
  vector<int> sa(n + 1);
  for (int i = 1; i <= n; i++) cin >> a[i];
  for (int i = 1; i <= m; i++) cin >> b[i];
  sort(begin(a) + 1, end(a));
  sort(begin(b) + 1, end(b));
  for (int i = 1; i <= m; i++) s[i] = b[i] + s[i - 1];
  for (int i = 1; i <= n; i++) sa[i] = a[i] + sa[i - 1];
  int res = 0;
  for (int di = 0; di <= n; di++) {
    int ans = 0;
    //[1,di],[di+1,n]
    // 左边 b>=a
    // 右边 b<=a
    ans += s[m] - s[m - di];
    ans -= sa[di];
    ans -= (s[n - di]);
    ans += (sa[n] - sa[di]);
    res = max(res, ans);
  }
  cout << res << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  cout << fixed << std::setprecision(15);
  int __;
  cin >> __;
  while (__--) solve();
  return 0;
}
```

[Problem - E - Codeforces](https://codeforces.com/contest/1921/problem/E)

## E. Eat the Chip

爱丽丝和鲍勃正在棋盘上玩游戏。棋盘上有 $h$ 行，从上到下依次编号；有 $w$ 列，从左到右依次编号。双方各有一个筹码。初始时，爱丽丝的筹码位于坐标为 $(x_a, y_a)$ （行 $x_a$ ，列 $y_a$ ）的单元格，而鲍勃的筹码位于 $(x_b, y_b)$ 。保证筹码的初始位置不重合。棋手轮流下棋，由爱丽丝开始。

在她的回合中，爱丽丝可以将她的筹码向下、向右或向左（对角线方向）移动一格。而鲍勃则可以将他的筹码向上、向右或向左移动一格。不允许超出棋盘边界。

更正式地说，如果爱丽丝在回合开始时位于坐标为 $(x_a, y_a)$ 的单元格中，那么她可以将筹码移动到 $(x_a + 1, y_a)$ 、 $(x_a + 1, y_a - 1)$ 或 $(x_a + 1, y_a + 1)$ 单元格中的一个。鲍勃在轮到他时，可以从坐标为 $(x_b, y_b)$ 的单元格移动到 $(x_b - 1, y_b)$ 、 $(x_b - 1, y_b - 1)$ 或 $(x_b - 1, y_b + 1)$ 。新的芯片坐标 $(x', y')$ 必须满足条件 $1 \le x' \le h$ 和 $1 \le y' \le w$ 。

![](https://espresso.codeforces.com/dbc362298c0158c22f271eca8a6d5611b3ae09a5.png) 示例棋局状态。爱丽丝下白色棋子，鲍勃下黑色棋子。箭头表示可能的走法。

如果棋手将自己的筹码放入对方筹码所占据的单元格中，则立即获胜。如果任何一方无法下棋（爱丽丝--如果她在最后一行，即 $x_a = h$ ；鲍勃--如果他在第一行，即 $x_b = 1$ ），对局立即以平局结束。

如果双方都以最佳状态下棋，对局结果会如何？

**思路**

这个题很多人想复杂了

我们想一下 他们相遇的时候能够到达的区间

设 A 走 X布后 两者 相遇

那么 A 相遇时的 X范围就是 $[X_A-X,X_A+X]$

设 B 走 X布后 两者 相遇

那么 B 相遇时的 X范围就是 $[X_B-X,X_B+X]$

如果有一个区间 能够包围另一个，那么这个区间就有吃掉另一个的 能力

```c++
// Codeforces - Codeforces Round 920 (Div. 3) E. Eat the Chip
// https://codeforces.com/contest/1921/problem/E 2024-01-16 00:01:22

int n, m;
void solve() {
  cin >> n >> m;
  int ay, ax, by, bx;
  cin >> ay >> ax >> by >> bx;
  if (ay >= by) {
    // 二者错过了就不能 回头了
    cout << "Draw\n";
  } else {
    // ay<by
    int time = by - ay;
    int cnta = (time + 1) / 2, cntb = (time) / 2;
    int valal = max(ax - cnta, 1ll), valar = min(ax + cnta, m);
    int valbl = max(bx - cntb, 1ll), valbr = min(bx + cntb, m);
    if (time & 1)
      if (valal <= valbl and valar >= valbr)
        cout << "Alice\n";
      else
        cout << "Draw\n";
    else if (valbl <= valal and valbr >= valar)
      cout << "Bob\n";
    else
      cout << "Draw\n";
  }
}

```

[Problem - F - Codeforces](https://codeforces.com/contest/1921/problem/F)

## F. Sum of Progression

给你一个由 $n$ 个数字组成的数组 $a$ 。还有 $q$ 个形式为 $s, d, k$ 的查询。

对于每个查询 $q$ ，求元素 $a_s + a_{s+d} \cdot 2 + \dots + a_{s + d \cdot (k - 1)} \cdot k$ 的和。换句话说，对于每个查询，都需要求出数组中从 $s$ 开始的 $k$ 个元素的和，步长为 $d$ ，乘以所得序列中元素的序号。

**输入**

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \le t \le 10^4$ ) - 测试用例的数量。下一行包含测试用例的描述。

每个测试用例的第一行包含两个数字 $n, q$ ( $1 \le n \le 10^5, 1 \le q \le 2 \cdot 10^5$ ) - 数组中元素的个数 $a$ 和查询次数。

第二行包含 $n$ 个整数 $a_1, ... a_n$ ( $-10^8 \le a_1, ..., a_n \le 10^8$ ) - 数组 $a$ 中的元素。

接下来的 $q$ 行分别包含三个整数 $s$ 、 $d$ 和 $k$ （ $1 \le s, d, k \le n$ 、 $s + d\cdot (k - 1) \le n$ ）。

保证所有测试用例的 $n$ 之和不超过 $10^5$ ，所有测试用例的 $q$ 之和不超过 $2 \cdot 10^5 $ 

**思路**

这个题可以用分块的思路解决

我们对 询问的$d$ 进行分析

如果 $d\ge \sqrt{n}$  那么 最多跳 $n/\sqrt{n} == \sqrt{n}$ 步

也就是 $ O(q\sqrt n)$ 的时间内可以 完成这一类 询问

如果是 

$d\le \sqrt n$ 呢

我们可以预处理 $\sqrt n$ 的范围 这样 可以在 $O(n\sqrt n )$的时间内 处理完

总的时间复杂度 是$O(n\sqrt n +q\sqrt n)$

**code**

```C++
// Codeforces - Codeforces Round 920 (Div. 3) F. Sum of Progression
// https://codeforces.com/contest/1921/problem/F 2024-01-16 00:08:55

#define int long long
int n, q;
int dp[100510][351];
int dp2[100510][351];
void solve() {
  cin >> n >> q;
  vector<int> a(n + 1);
  int len = sqrtl(n);
  for (int i = 1; i <= n; i++) cin >> a[i];
  for (int i = 0; i <= n; i++)
    for (int j = 0; j <= len; j++) dp[i][j] = 0;
  for (int j = 1; j <= len; j++)
    for (int i = n; i >= 1; i--)
      dp[i][j] = dp[i + j][j] + a[i], dp2[i][j] = dp2[i + j][j] + dp[i][j];
  while (q--) {
    int s, d, k;
    cin >> s >> d >> k;
    if (d >= len) {
      int ans = 0;
      for (int i = s, j = 1; i <= n and j <= k; i += d, j++) ans += a[i] * j;
      cout << ans << " ";
    } else
      cout << dp2[s][d] - dp2[s + d * k][d] - k * dp[s + d * k][d] << " ";
  }
  cout << "\n";
}

```