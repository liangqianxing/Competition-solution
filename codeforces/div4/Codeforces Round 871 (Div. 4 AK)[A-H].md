# Codeforces Round 871 (Div. 4 AK)[A-H]

[Codeforces Round 871 (Div. 4)](https://codeforces.com/contest/1829)

## A. Love Story[枚举]

帖木儿喜欢密码电报。这就是为什么他有一个长度为 $10$的字符串 $s$，其中只包含小写拉丁字母。帖木儿想知道字符串 $s$与 "编码 "的**差异**是多少。与字符串 **"codeforces "**不同。

例如字符串 $s =$"co**ol**for**s**e**z**"与 "codeforces "相差 $4$个索引，用粗体表示。

帮助 Timur 找出字符串 $s$ 与 "codeforces "不同的索引数。

注意不能对字符串 $s$中的字符重新排序。

****

思路按照题意遍历一遍

记录不同位置的数量

```c++
int n, m;
std::string ans = "codeforces";
void solve() {
  n = ans.size();
  std::string s;
  std::cin >> s;
  int res = 0;
  for (int i = 0; i < n; i++) {
    if (s[i] != ans[i]) res++;
  }
  std::cout << res << "\n";
}
```

## B. Blank Space[枚举]

给你一个由 $n$个元素组成的二进制数组 $a$，二进制数组是一个只由$0$个和$1$个元素组成的数组。

空格是指仅由$0$个元素组成的**连续**元素段。

你的任务是找出最长空白的长度。

****

记录上一个1出现的位置，枚举一遍数组更新距离

```c++
int n, m;
void solve() {
  std::cin >> n;
  std::vector<int> a(n);
  for (auto &i : a) std::cin >> i;
  int last = -1;
  int ans = 0;
  for (int i = 0; i < n; i++) {
    if (a[i] == 1) last = i;
    ans = std::max(ans, i - last);
  }
  std::cout << ans << "\n";
}
```

## C. Mr. Perfectly Fine[贪心,枚举]

维克多想成为 "完美先生"。为此，他需要掌握一定的技能。更确切地说，他有$2$项技能需要掌握。

维克多有$n$本书。阅读$i$本书需要$m_i$分钟，他将获得所需的两种技能中的一部分（可能没有），用长度为$2$的二进制字符串表示。

为了让维克多掌握所有这两种技能，最少需要多少时间？

****

**思路**

因为题目中一共就两种书

有两种方式可以达成条件

一种是`11`

一种是`01`和`10`

所以我们贪心计算一下两种方式的最优解

```c++
int n, m;
void solve() {
  std::cin >> n;
  std::map<std::string, int> mp;
  for (int i = 0; i < n; i++) {
    int a;
    std::string b;
    std::cin >> a >> b;
    if (!mp.count(b)) {
      mp[b] = a;
    } else {
      mp[b] = std::min(mp[b], a);
    }
  }
  int ans = LLONG_MAX;
  if (mp.count("11")) ans = std::min(ans, mp["11"]);
  if (mp.count("01") and mp.count("10"))
    ans = std::min(ans, mp["01"] + mp["10"]);
  std::cout << (ans == LLONG_MAX ? -1 : ans) << "\n";
}
```

## D. Gold Rush[分解因子]

最初，你有一堆金块，其中有 $n$块金块。在一次操作中，你可以进行以下操作：

- 将任意一堆金块分成两堆，使其中一堆的金块数量正好是另一堆的两倍。(所有金块堆的数量都应该是整数）。

![](https://espresso.codeforces.com/723dd5ce328f1a04932ecf7a71a71f198294eaa3.png)

一种可能的做法是将大小为$6$的一堆金块分成大小为$2$和$4$的两堆，这是有效的，因为$4$是$2$的两倍。

你能用 0 或更多的运算使一堆金块**恰好**$m$个金块吗？

****

**思路**

通过题意我们显然可以发现

一个三的因子可以拆成1或者2

所以我们要让3能够填充到不足的2上

如果还有其他的因子，那么其他的因子是无法改变的

那么就会失败

```c++
int n, m;
void solve() {
  std::cin >> n >> m;
  int cnt_2_0 = 0;
  int cnt_2_1 = 0;
  int cnt_3_0 = 0;
  int cnt_3_1 = 0;
  int tmp1 = n;
  while (tmp1) {
    if (tmp1 % 2 == 0)
      tmp1 >>= 1, cnt_2_0++;
    else if (tmp1 % 3 == 0)
      tmp1 /= 3, cnt_3_0++;
    else
      break;
  }
  int tmp = m;
  while (tmp) {
    if (tmp % 2 == 0)
      tmp >>= 1, cnt_2_1++;
    else if (tmp % 3 == 0)
      tmp /= 3, cnt_3_1++;
    else
      break;
  }
  while (n % 2 == 0 and m % 2 == 0) n /= 2, m /= 2;
  while (n % 3 == 0 and m % 3 == 0) n /= 3, m /= 3;
  if (m > n) {
    std::cout << "NO\n";
    return;
  } else {
    if (tmp1 != tmp) {
      std::cout << "NO\n";
      return;
    }
  }
  if (cnt_3_0 - cnt_3_1 >= cnt_2_1 - cnt_2_0 and cnt_2_0 <= cnt_2_1) {
    std::cout << "YES\n";
  } else
    std::cout << "NO\n";
}
```

## E. The Lakes[BFS]

给你一个由非负整数组成的 $n \times m$网格 $a$。数值 $a_{i,j}$表示第 $i$行和第 $j$列的水深。

湖泊是这样一组单元格：

- 集合中的每个单元格都有$a_{i,j}>0$，并且
- 在湖中的任意一对单元格之间存在一条路径，该路径需要向上、向下、向左或向右移动若干次，且不能踩到$a_{i,j} = 0$的单元格。

湖泊的体积是湖中所有单元格的深度总和。

请找出网格中最大的湖泊体积。

****

那么这个题是最大连通块大小变了一下下

我们用BFS可以解决

```c++
int n, m;
void solve() {
  std::cin >> n >> m;
  std::vector<std::vector<int>> a(n, std::vector<int>(m));
  std::vector<std::vector<int>> dp(n, std::vector<int>(m, -1));
  std::vector<std::vector<bool>> f(n, std::vector<bool>(m));
  for (auto &i : a)
    for (auto &j : i) {
      std::cin >> j;
    }
  int dx[] = {0, 0, 1, -1};
  int dy[] = {1, -1, 0, 0};
  auto is = [&](int x, int y) -> bool {
    if (x < 0 or x >= n)
      return false;
    else if (y < 0 or y >= m)
      return false;
    else
      return true;
  };
  int ans = 0;
  auto bfs = [&](int x, int y) -> int {
    std::queue<std::array<int, 2>> q;
    int res = a[x][y];
    q.push({x, y});
    f[x][y] = true;
    while (!q.empty()) {
      auto [x, y] = q.front();
      q.pop();
      for (int i = 0; i < 4; ++i) {
        int X = x + dx[i];
        int Y = y + dy[i];
        if (is(X, Y) and a[X][Y] and !f[X][Y]) {
          f[X][Y] = true;
          res += a[X][Y];
          q.push({X, Y});
        }
      }
    }
    return res;
  };
  for (int i = 0; i < n; i++)
    for (int j = 0; j < m; j++)
      if (a[i][j] and !f[i][j]) ans = std::max(ans, bfs(i, j));
  std::cout << ans << "\n";
}
```

## F. Forever Winter[思维，建图]

一个雪花图是由两个整数 $x$和 $y$生成的，这两个整数都大于 $1$，如下所示：

- 从一个中心顶点开始。
- 将 $x$个新顶点连接到这个中心顶点。
- 将$y$个新顶点连接到这$x$个顶点中的***个顶点。

例如，下面是$x=5$和$y=3$的雪花图。

![](https://espresso.codeforces.com/9f2183c62c4c713927bb18746d1f5455da90f612.png)

上面的雪花图有一个中心顶点$15$，然后是连接它的$x=5$个顶点（$3$、$6$、$7$、$8$、$20$和$20$），然后是连接这些顶点的$y=3$个顶点。

给定一个雪花图，确定 $x$ 和 $y$ 的值。

****

**思路**

因为题目中说了这两个整数都大于 $1$

那么有两种情况啊

 $x$ 和 $y+1$ 相等

 $x$ 和 $y+1$ 不等

-  我们先看$x$ 和 $y+1$不相等

那么中间那个点的度数是唯一的

那么我们计算所有点的度数，只出现一次的就是中间点的度数

除了1以外的另一个度数就是$y+1$

- $x$ 和 $y+1$相等时呢

那我们去掉 1 得到一个$ANS$

$x=ANS$

$y=ANS-1$

```c++
int n, m;
void solve() {
  const int N = 250;
  std::map<int, int> du;
  std::cin >> n >> m;
  std::vector<int> G[N];
  std::set<int> S;
  for (int i = 0; i < m; i++) {
    int from, to;
    std::cin >> from >> to;
    from--;
    to--;
    du[from]++;
    du[to]++;
    G[from].emplace_back(to);
    G[to].emplace_back(from);
  }
  std::map<int, int> count;
  for (auto i = 0; i < n; i++) {
    for (auto it : G[i]) {
      S.insert(du[it]);
      count[du[it]]++;
    }
  }
  std::vector<int> ans;
  for (auto it : S) {
    ans.emplace_back(it);
  }
  sort(begin(ans), end(ans));
  for (auto it : ans) {
    std::cerr << it << " " << count[it] << "\n";
    if (count[it] == it) {
      std::cout << it << " ";
      for (auto g : ans) {
        if (it == g)
          continue;
        else if (g == 1)
          continue;
        else {
          std::cout << g - 1 << "\n";
          return;
        }
      }
    }
  }
  std::cout << ans.back() << " " << ans.back() - 1 << "\n";
}
```

## G. Hits Different[简单DP]

在嘉年华游戏中，有一个巨大的罐子金字塔，上面有$2023$行，如图所示按规律编号。

![](https://espresso.codeforces.com/3f72320a7f225babc9b2b244a719e59e9e8f028d.png)

如果最初击中的是易拉罐$9^2$，那么上图中所有红色的易拉罐都会倒下。

你向金字塔扔了一个球，球击中了一个编号为$n^2$的罐子。这将导致堆叠在这个罐子上面的所有罐子掉落（也就是说，$n^2$号罐子掉落，然后$n^2$号罐子掉落，然后这些罐子上面的罐子掉落，以此类推）。例如，上图显示的是如果易拉罐$9^2$被击中，会倒下的易拉罐。

所有掉落的易拉罐上数字的和是多少？请回顾 $n^2 = n \times n$。

****

这个题可以发现，一个易拉罐倒塌的话，上面的易拉罐也会倒塌

那么我们就可以从上面的易拉罐 递推得来

但是比如 dp[9]=dp[5]+dp[6]这样的话很容易发现有重合的

那么就把重合的地方减去

dp[9]=dp[5]+dp[6]-dp[3]

由此便可以解决

```c++
int n, m;
const int N = 1e6 + 10;
int a[N], s[N], ans[N];
int idx;
int bt[N];
void solve() {
  std::cin >> n;
  std::cout << ans[n] << "\n";
}

signed main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(0), std::cout.tie(0);
  std::cout << std::fixed << std::setprecision(15);
  std::cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  for (int i = 1; i < N; i++) {
    s[i] = i;
  }
  for (int i = 1; i < N; i++) {
    s[i] += s[i - 1];
  }
  idx = 1;
  for (int i = 1; i < N; i++) {
    ans[i] = i * i;
    bt[i] = idx;
    if (idx >= 2 and i - idx + 1 <= s[idx - 1] and i - idx + 1 > s[idx - 2])
      ans[i] += ans[i - idx + 1];
    if (idx >= 2 and i - idx > s[idx - 2] and i - idx <= s[idx - 1])
      ans[i] += ans[i - idx];
    if (idx >= 3 and i - idx * 2 + 2 > s[idx - 3] and
        i - idx * 2 + 2 <= s[idx - 2])
      ans[i] -= ans[i - idx * 2 + 2];
    if (i >= s[idx]) idx++;
  }
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## H. Don't Blame Me[背包DP]

遗憾的是，问题设置者想不出一个有趣的故事，因此他只要求你解决下面的问题。

给定一个由 $n$个正整数组成的数组 $a$，数出子序列中元素的位$\mathsf{AND}$在其二进制表示中正好有$k$个置位的**非空**子序列的个数。答案可能会很大，因此输出它的模数 $10^9+7$。

回想一下，数组 $a$的子序列是通过删除一些元素（可能是零）从 $a$得到的序列。例如，$[1, 2, 3]$、$[3]$、$[1, 3]$是$[1, 2, 3]$的子序列，但$[3, 2]$和$[4, 5, 6]$不是。

请注意，$\mathsf{AND}$表示【位和运算】(https://en.wikipedia.org/wiki/Bitwise_operation#AND)。

****

一看数据范围

$n$ and $k$ ($1 \leq n \leq 2 \cdot 10^5$, $0 \le k \le 6$)

那么显然k有大用，又因为这个题跟二进制有关

那么我们可以想到枚举$2^k$ 

每个数选不选的多决策

很像背包啊，第二维枚举状态

那么我们就可以当成背包来做

```C++
int n, k;
const int mod = 1e9 + 7;
void solve() {
  std::cin >> n >> k;
  std::vector<int> a(n);
  for (auto &i : a) std::cin >> i;
  std::vector<std::vector<int>> dp(n + 1, std::vector<int>(1 << 6, 0ll));
  for (int i = 1; i <= n; i++) {
    for (int bit = 0; bit < 64; bit++) {
      (dp[i][bit] += dp[i - 1][bit]) %= mod;
      (dp[i][bit & a[i - 1]] += dp[i - 1][bit]) %= mod;
    }
    (dp[i][a[i - 1]] += 1) %= mod;
  }
  int ans = 0;
  for (int bit = 0; bit < 64; bit++) {
    if (__builtin_popcount(bit) == k) {
      (ans += dp[n][bit]) %= mod;
    }
  }
  std::cout << ans << "\n";
}
```

