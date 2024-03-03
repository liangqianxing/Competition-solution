#! https://zhuanlan.zhihu.com/p/660183147
# Codeforces Round 902 (Div. 2)[A-D]

[Codeforces Round 902 (Div. 2)](https://codeforces.com/contest/1877)

## A. Goals of Victory[签到]

有$n$支球队参加足球锦标赛。每对队比赛一次。每场比赛结束后，帕克·查内克都会收到两个整数作为比赛结果，即两队在比赛中的进球数。一支球队的效率等于该队每场比赛的总进球数减去对手每场比赛的总进球数。在它的每一个匹配中都有Pponent。比赛结束后，帕克·登克莱克（Pak Dengklek）统计了每支球队的效率。结果他忘记了其中一支球队的效率。

考虑到$n-1$个团队$a_1,a_2,a_3,\ldots,a_{n-1}$的效率。缺失的团队效率如何？可以证明，缺失团队的效率可以唯一确定。

**code**

```c++
#include <bits/stdc++.h>
#define int long long
int n, m;
void solve() {
  std::cin >> n;
  std::vector<int> a(n - 1);
  for (auto &i : a) {
    std::cin >> i;
  }
  int ans = 0;
  for (auto it : a) {
    ans += it;
  }
  std::cout << -ans << "\n";
}

signed main() {
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## B. Helmets in Night Light[贪心]

Pak Chanek是Khuntien村的村长。在一个灯火通明的夜晚，Pak Chanek有一个突发的重要公告，需要通知Khuntien的所有$n$居民。首先，Pak Chanek将公告直接分享给一个或多个居民，每个人的费用为$p$。

之后，居民可以使用一个神奇的头盔形状的设备将公告分享给其他居民。然而，使用头盔形装置存在成本。对于每个$i$，如果第$i$位居民至少收到一次公告（直接从Pak Chanek或从其他居民），他/她最多可以将公告共享给$a_i$个其他居民，每个共享**的费用为$b_i$**。如果Pak Chanek还可以控制居民如何与其他居民共享公告，

Pak Chanek通知Khuntien所有$n$居民有关公告的最低费用是多少？

****

显然是能用更便宜的就用更便宜的

不能的话就用p

**code**

```c++
#include <bits/stdc++.h>
#define int long long
int n, m, p;
struct point {
  int x, cnt;
  bool operator<(const point& t) const { return x < t.x; };
};
void solve() {
  std::cin >> n >> p;
  std::vector<point> tt;
  for (int i = 0; i < n; i++) {
    int a, b;
    std::cin >> a;
    tt.push_back({0, a});
  }
  for (int i = 0; i < n; i++) {
    int a, b;
    std::cin >> b;
    tt[i].x = b;
  }
  sort(begin(tt), end(tt));
  int cost = p;
  int cnt = 1;
  for (auto g : tt) {
    int a = g.x;
    int b = g.cnt;
    if (a >= p)
      break;
    else {
      if (cnt + b > n) {
        cost += a * (n - cnt);
        cnt = n;
      } else {
        cost += a * b;
        cnt += b;
      }
    }
  }
  cost += (p) * (n - cnt);
  std::cout << cost << "\n";
}

signed main() {
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## C. Joyboard[打表]

Chaneka是一名游戏玩家，他发明了一种名为Joyboard的新型游戏控制器。有趣的是，她发明的游戏板只能用来玩一种游戏。Joyboard有一个包含$n+1$个插槽的屏幕，从左到右编号为$1$到$n+1$。

将使用非负整数$[a_1,a_2,a_3,\ldots,a_{n+1}]$的数组填充$n+1$插槽。作为玩家，Chaneka必须为$a_{n+1}$分配一个介于$0$和$m$之间的整数。然后，对于从$n$到$1$的每个$i$，$a_i$的值将等于除以$a_{i+1}$的**余数**（右边的相邻值）由$i$。换句话说，$a_i = a_{i + 1} \bmod i$。Chaneka希望在为每个时隙分配一个整数之后，

在整个屏幕中正好有$k$个不同的值（在所有$n+1$个插槽中）. 有多少种有效方法可以将非负整数分配到插槽$n+1$中？

****

打个表就知道了

**code**

```c++
#include <bits/stdc++.h>
#define int long long
int n, m, k;
void solve() {
  std::cin >> n >> m >> k;
  std::vector<int> ans(n + 15);
  std::map<int, int> mp;
  for (int val = 0; val <= m; val++) {
    std::set<int> S;
    ans[n + 1] = val;
    for (int i = n; i >= 1; i--) {
      ans[i] = ans[i + 1] % i;
    }
    for (int i = 1; i <= n + 1; i++) {
      std::cerr << ans[i] << " ";
      S.insert(ans[i]);
    }
    mp[S.size()]++;
    std::cerr << "\n";
  }
  std::cout << mp[k] << "\n";
}

signed main() {
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

**case**

```c++
[22:53:16] [编译器] [编译已开始]
[22:53:17] [编译器] [编译已结束]
[22:53:17] [运行器[4]] [程序已开始运行]
[22:53:17] [运行器[4]] [程序在测试点 #4 上运行了 120ms 后终止了]
[22:53:17] [运行器[4]/stderr] [
Time elapsed: 0.001 s. 
0 0 0 0 0 0  
0 1 1 1 1 1  
0 0 2 2 2 2  
0 0 0 3 3 3  
0 0 0 0 4 4  
0 0 0 0 0 5  
0 0 0 0 0  
0 1 1 1 1  
0 0 2 2 2  
0 0 0 3 3  
0 0 0 0 4  
0 1 1 1 5  
0 0 0 0 0 0  
0 1 1 1 1 1  
0 0 2 2 2 2  
0 0 0 3 3 3  
0 0 0 0 4 4  
]

```

**code**

```c++
#include <bits/stdc++.h>
#define int long long
int n, m, k;
void solve() {
  std::cin >> n >> m >> k;
  if (n == m) {
    if (k == 1) {
      std::cout << "1\n";
    } else if (k == 2) {
      std::cout << n << "\n";
    } else if (k == 3) {
      std::cout << 0 << "\n";
    } else {
      std::cout << 0 << "\n";
    }
  } else if (n < m) {
    if (k == 1) {
      std::cout << "1\n";
    } else if (k == 2) {
      std::cout << n - 1 + m / n << "\n";
    } else if (k == 3) {
      std::cout << m - (n - 1 + m / n) << "\n";
    } else {
      std::cout << 0 << "\n";
    }
  } else {
    if (k == 1) {
      std::cout << "1\n";
    } else if (k == 2) {
      std::cout << m << "\n";
    } else if (k == 3) {
      std::cout << 0 << "\n";
    } else {
      std::cout << 0 << "\n";
    }
  }
}

signed main() {
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## D. Effects of Anti Pimples[计数]

Chaneka有一个数组$[a_1,a_2,\ldots,a_n]$。最初，所有元素都是白色的。Chaneka将选择一个或多个不同的索引，并将这些选定索引的元素涂成黑色。然后，她将选择索引是**至少一个**黑色元素的索引的倍数的所有白色元素，并将这些元素着色为绿色。之后，她的分数是所有黑色和绿色元素中的**最大**值$a_i$。Chaneka有$2^n-1$种选择黑色指数的方法。找出Chaneka可以选择黑色指数的所有可能方式的得分总和。

因为答案可能很大，所以以$998\,244\,353$为模打印答案。

****

将可以筛到的地方维护到底数就好

然后再枚举一下底数的集合和贡献数

**code**

```c++
#include <bits/stdc++.h>
#define int long long
int n, m;
const int mod = 998244353;
struct point {
  int val, bf_idx, rank;
  bool operator<(const point& t) const { return val < t.val; };
};
int qmi(int a, int b, int p = mod) {
  int res = 1 % p;
  while (b) {
    if (b & 1) res = res * a % p;
    a = a * a % p;
    b >>= 1;
  }
  return res;
}
using p = point;
void solve() {
  std::cin >> n;
  std::vector<int> a(n);
  for (auto& i : a) {
    std::cin >> i;
  }
  std::vector<p> now;
  std::vector<int> val(n + 1);
  std::vector<int> arr(n + 1);
  for (int i = 1; i <= n; i++) {
    arr[i] = a[i - 1];
  }
  for (int i = 1; i <= n; i++) {
    for (int j = i; j <= n; j += i) {
      val[i] = std::max(val[i], arr[j]);
    }
  }
  for (int i = 0; i < n; i++) {
    now.push_back((point){val[i + 1], i, -1});
  }
  sort(begin(now), end(now));
  for (int i = 0; i < n; i++) {
    now[i].rank = i + 1;
  }
  sort(begin(now), end(now), [&](const point& a, const point& b) -> bool {
    return a.bf_idx < b.bf_idx;
  });
  int ans = 0;
  for (int i = 0; i < n; i++) {
    ans += qmi(2, now[i].rank - 1) * now[i].val;
    ans %= mod;
  }
  std::cout << ans << "\n";
}

signed main() {
  solve();
  return 0;
}
```

