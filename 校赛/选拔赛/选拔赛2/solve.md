[toc]

# XJU-GPLT2

## L1-1

比较整数a和整数b的大小。

**code**

```C++
int n, m;
void solve() {
  cin >> n >> m;
  if (n < m)
    cout << "<\n";
  else if (n == m)
    cout << "=\n";
  else
    cout << ">\n";
}

```

## L1-2

给出n个数字,请你求出在给出的这n个数字当中,最大的数字与次大的数字之差,最大的数字与次小的数字之差,次大的数字与次小的数字之差,次大的数字与最小的数字之差.

**think**

这个题需要去一下重

比如`1 2 2 3 3`

那么次小的数字就是`2` 而不是`3`

**code**

```C++
int n, m;
void solve() {
  cin >> n;
  vector<int> a;
  set<int> S;
  for (int i = 0; i < n; i++) {
    int x;
    cin >> x;
    S.emplace(x);
  }
  for (auto it : S) a.emplace_back(it);
  n = S.size();
  sort(all(a));
  auto b = a;
  reverse(all(b));
  cout << b[0] - b[1] << " " << b[0] - a[1] << " " << b[1] - a[1] << " "
       << b[1] - a[0] << "\n";
}
```

## L1-3

Reverie正在训练一个三角形分类模型，她手里有一堆数据，你能帮她标注一下么？
    
给定三个正整数，代表可能组成一个三角形的三条边长。
    
如果能组成 一个等边三角形，输出"equilateral"; 如果能组成 一个直角三角形，输出"right"; 如果不能组成 一个三角形，输出"error", 否则，输出"normal".

**think**

小分类讨论

**code**

```C++
int n, m;
void solve() {
  vector<int> a(3);
  for (auto &i : a) cin >> i;
  sort(all(a));
  if (a[0] + a[1] <= a[2])
    cout << "error\n";
  else if (a[0] == a[2])
    cout << "equilateral\n";
  else if (a[0] * a[0] + a[1] * a[1] == a[2] * a[2])
    cout << "right\n";
  else
    cout << "normal\n";
}
```

## L1-4

这一天你跟你的n个朋友一起出去玩，在出门前妈妈给了你k块糖果，你决定把这些糖果的一部分分享给你的朋友们。由于你非常热心，所以你希望你的每一个朋友分到的糖果数量都比你要多（严格意义的多，不能相等）。求你最多能吃到多少糖果？

**think**

这个题我不喜欢,应该是有`O(1)`的公式的,但我没推出来

观察这个是有单调性的,所以我们可以写个`log`的二分

注意`long long `!

**code**

```C++
long long Maximumcandies(long long n, long long k) {
  long long l = 0, r = k;
  long long ans = 0;
  while (l <= r) {
    long long mid = l + r >> 1;
    if ((k - mid) / n > mid) {
      l = mid + 1;
      ans = mid;
    } else
      r = mid - 1;
  }
  return ans;
}
```

## L1-5

小A最近买了一台新的打印机,她想测试一下她的打印机是否能正常工作。  
于是，她准备在纸张上打上一个n \* n的相同字符图形来测试它的打印机是否出了问题。  
如果打印机正常，则所有字符均一样。

**think**

感觉这个应该放到 `L1-1` 与 `L1-2` 之间

**code**

```C++
int n, m;
void solve() {
  cin >> n;
  set<char> S;
  for (int i = 1; i <= n * n; i++) {
    char x;
    cin >> x;
    S.emplace(x);
    if (S.size() >= 2) {
      cout << "N0, it broken:(\n";
      return;
    }
  }
  cout << "YE5, the printer is working properly.\n";
}
```

## L1-6

昌子哥特别喜欢吃糖果，每天他都要吃两颗糖且要是**不同种类**的糖果**各一颗**.

现在他面前有三种糖果，分别为a,b,c个,他希望他能吃最多的天数，现在他问你寻求帮助。请你告诉他最多能吃多少天

**think**

如果是两个最小的数目加起来大于等于最大的数目

那么一定不会有因为是相同而无法吃掉的2个剩下

所以就将所有的加起来除以二就是答案

否则的话只能吃掉 最小的两个加起来的天数

**code**

```C++
int n, m;
void solve() {
  vector<int> a(3);
  for (auto &i : a) cin >> i;
  sort(all(a));
  if (a[0] + a[1] >= a[2]) {
    cout << (a[0] + a[1] + a[2]) / 2 << "\n";
  } else
    cout << (a[0] + a[1]) << "\n";
}
```

## L1-7

小民今天在C语言课上学习了位运算。  
他写下了一段程序：  

```c++
long long n, a, b, i;
for(a = 1, i = 1; i <= n; ++ i)
    a &= i;
for(b = 1, i = 1; i <= n; ++ i)
    b |= i;```

小民想知道a和b的值，但刚好下课了，你能帮帮他么？
```

**think**

我们知道$x\&y\le x$

一开始`a` 就是`1`

当`a&2`后,`a`恒为`0`

对于`|` 我们找最高位

从`0000~1000` 后面所有的位数一定经过`1`

所以就是`1111`

找最高位就可以知道答案

**code**

```C++
int n, m;
void solve() {
  cin >> n;
  if (n == 1)
    cout << "1 ";
  else
    cout << "0 ";
  int h = 0;
  for (int i = 0; i < 64; i++) {
    if ((n >> i) & 1) h = i;
  }
  cout << (1ll << (h + 1)) - 1ll << "\n";
}
```

## L1-8

已知一个正方体，每个面上都有任意一个数（假设每一面的面积足够大来装下当前面上的数字），现被展开成了如下形式：

![](https://uploadfiles.nowcoder.com/images/20181210/303785_1544441144881_17992B38D412F3C01EAECD17AAACB4F7)

输入中保证第一行有一个面，第二行有四个面，第三行有一个面。请用代码检查这个正方体对立面上的数是否相同。

**think**

我们可以发现一个规律

那就是第一行的数和第三行的数一定是对立的

而这两行又只有一个数

第二行的第一个和第三个对立

第二个和第四个对立

那么就解决了

还有一个需要注意的地方是数字是可以大于10的,注意一下数值范围

还有格式每五十个答案多换一次行

**code**

```C++
// NowCoder L1-8(200pts)
// https://ac.nowcoder.com/acm/contest/77713/H 2024-03-23 16:54:39

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  vector<vector<int>> a(3, vector<int>(4));
  for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 4; j++) {
      cin >> a[i][j];
    }
  }
  vector<int> val(3);
  for (int j = 0; j < 4; j++)
    if (a[0][j]) val[0] = a[0][j];
  for (int j = 0; j < 4; j++)
    if (a[2][j]) val[2] = a[2][j];
  if (val[2] != val[0]) return (void)(cout << "No!\n");
  if (a[1][0] != a[1][2]) return (void)(cout << "No!\n");
  if (a[1][1] != a[1][3]) return (void)(cout << "No!\n");
  return (void)(cout << "Yes!\n");
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int __;
  cin >> __;
  for (int i = 1; i <= __; i++) {
    solve();
    if (i % 50 == 0) cout << "\n";
  }
  return 0;
}
```

## L2-1

    小H正在玩一个战略类游戏，她可以操纵己方的飞机对敌国的N座城市(编号为1~N)进行轰炸  
    敌国的城市形成了一棵树，小H会依次进行Q次轰炸，每次会选择一个城市A进行轰炸，和这座城市距离不超过2的城市都会受损(这里距离的定义是两点最短路径上的边数)，轰炸结束后，小H还想知道当前城市A受损的次数  
    作为游戏的开发者之一，你有义务回答小H的问题

**think**

我们先想朴素的做法

就是往上递归父亲

往下递归儿子

那么我们发现 在往下递归儿子的时候是会超时的

那么我们可以优化这个操作

我们可以发现,一个节点对于其父辈最多追溯两代

然后我们可以设置三格深度来转移

下面是状态的转移
$$
val[x][0], val[fa[x]][1], val[fa[x]][2], val[fa[fa[x]]][2]\\
自己+父亲+父亲作爷爷+真爷爷\\
val[x][2] + val[fa[x]][1] + val[fa[x]][0] + val[fa[fa[x]]][0]\\
子孙+同辈+父亲+爷爷\\
$$
记录状态时可能父亲作爷爷比较难理解, 之所以`val[fa[x]][1], val[fa[x]][2]`都要记录是因为 要同时统计子孙

这样就能在下面同时记录子孙的影响了



**code**

```c++
#define int long long
int n, q;
constexpr int N = 8E5 + 10;
int val[N][3];
int fa[N];
void solve() {
  cin >> n >> q;
  vector<vector<int>> G(n + 1);
  for (int i = 1; i < n; i++) {
    int x, y;
    cin >> x >> y;
    if (x > y) swap(x, y);
    G[x].emplace_back(y);
    fa[y] = x;
  }
  while (q--) {
    int sum = 0, x = 0;
    cin >> x;
    val[x][0]++, val[fa[x]][1]++, val[fa[x]][2]++, val[fa[fa[x]]][2]++;
    cout << val[x][2] + val[fa[x]][1] + val[fa[x]][0] + val[fa[fa[x]]][0]
         << "\n";
  }
}
```

## L2-2

可能很多人要吐槽为什么标题不是“救救blabla”了。

怪人PM6喜欢数糖纸，不同的糖纸有不同的颜色，一共有 N 张糖纸，第 i 张糖纸颜色为 Ci ，它们的位置都是固定的。PM6喜欢五彩缤纷的糖纸，所以他不希望有重复的颜色。他有一次机会，可以收集任意一段连续区间内的糖纸。求出PM6最多能收集多少张糖纸。

**think**

我们用双指针来维护区间

当当前区间内有重复元素时, 弹出左端元素直到无重复元素

根据势能分析法

`r`最多移动`n`次

`l`最多移动`n`次

所以时间复杂度是`O(n)`的

**code**

```C++
// NowCoder L2-2(250pts)
// https://ac.nowcoder.com/acm/contest/77713/J 2024-03-23 16:04:39

#include <bits/stdc++.h>
using namespace std;
#define all(a) begin(a), end(a)
#define int long long
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  map<int, int> mp;
  int l = 0, r = 0;
  mp[a[0]]++;
  int ans = 0;
  while (r < n) {
    while (r + 1 < n) {
      r++;
      mp[a[r]]++;
      if (mp[a[r]] >= 2) {
        break;
      } else
        ans = max(ans, r - l + 1);
    }
    while (mp[a[r]] >= 2) {
      mp[a[l++]]--;
    }
    if (r + 1 == n) break;
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

## L2-4

牛牛即将要参加考试，他学会了填答题卡。  

可惜他竖着的答题卡填成了横着的 : (  


好奇的他想知道对于 n 道题，每道题 n 个选项的答题卡 ( n \* n 的矩阵 )，满足横答题卡和竖答题卡图形一致的方案数有多少种。

  

注：每道题只能选择一个选项，即 n \* n 的矩阵中只能涂黑 n 个空。求横竖对称的方案数。

  

![](https://uploadfiles.nowcoder.com/files/20200404/327740_1585990029978_Gdn2Ed.png)

**think**

题目中求得就是轴对称的方案数

![](https://uploadfiles.nowcoder.com/files/20200404/327740_1585990084896_GdnIv8.png)

我们观察$f(3)$

可以发现`1,3`情况都是由 $f(2)$ 转移到来

`2,4`情况可以由$f(1)$转移到来

推出转移方程$f(x)=f(x-1)+(x-1)f(x-2)$

$f(1)=1,f(2)=2$

由此解决了本题

**code**

```c++

#define int long long
int n, m;
constexpr int N = 2E5 + 10, mod = 1E9 + 7;
int dp[N];
void solve() {
  cin >> n;
  dp[1] = 1, dp[2] = 2;
  for (int i = 3; i <= n; i++) {
    dp[i] = dp[i - 1] + (i - 1) * dp[i - 2];
    dp[i] %= mod;
  }
  cout << dp[n] << "\n";
}

```

## L3-2

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

