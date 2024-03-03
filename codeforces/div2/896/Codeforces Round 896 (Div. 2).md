#! https://zhuanlan.zhihu.com/p/655995969
# Codeforces Round 896 (Div. 2)

[Codeforces Round 896 (Div. 2)](https://codeforces.com/contest/1869)

## A. Make It Zero[xor 性质]

在中考期间，Reycloer 遇到了一个有趣的问题，但他无法立即想出解决办法。时间不多了！请帮助他。

最初，给你一个由 $n \ge 2$个整数组成的数组 $a$，你想将其中的所有元素都改为 $0$。

在一次操作中，您选择两个索引 $l$和 $r$（$1\le l\le r\le n$），并执行以下操作：

- 设 $s=a_l\oplus a_{l+1}\oplus \ldots \oplus a_r$，其中 $\oplus$表示 [bitwise XOR 运算](https://en.wikipedia.org/wiki/Bitwise_operation#XOR)；
- 然后，对于所有 $l\le i\le r$，用 $s$替换 $a_i$。

以任何顺序进行上述操作，总共最多可以使用 $8$ 次。

求一个运算序列，使得依次进行运算后，$a$中的所有元素都等于$0$。可以证明解总是存在的。

****

**显然有一个性质 对一段子序列做操作以后 那么这段子序列都会变成 相同的数**

那么根据这个性质 我们可以构造 一个算法 

我们可以通过 选中 $[1,n]$ 来让所有的 数都变成相同的

那么 有一个性质： **一个数 异或 自己偶数次 为0**,**一个数 异或 自己奇数次 为自身**

可以自己证明一下

**如果 序列的长度为  偶数**

我们可以通过再次选中$[1,n]$ 让所有数都变成0

**如果 序列的长度为  奇数**

我们可以通过再次选中$[1,n-1]$ 让$[1,n-1]$ 都变成0

然后选中 $[n-1,n]$ 让  $[n-1,n]$变成 相同的

又因为 $[n-1,n]$的长度是偶数 ，所以再次选中 $[n-1,n]$便可以变为0

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m;
void dfs(int l, int r) {
  if ((r - l + 1) % 2 == 0) {
    std::cout << 2 << "\n";
    std::cout << l << " " << r << '\n';
    std::cout << l << " " << r << '\n';
  } else {
    std::cout << 4 << "\n";
    std::cout << l << " " << r - 1 << '\n';
    std::cout << l << " " << r - 1 << '\n';
    std::cout << r - 1 << " " << r << '\n';
    std::cout << r - 1 << " " << r << '\n';
  }
}
void solve() {
  std::cin >> n;
  std::vector<int> a(n);
  for (auto &i : a) std::cin >> i;
  dfs(1, n);
}

signed main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(0), std::cout.tie(0);
  std::cout << std::fixed << std::setprecision(15);
  std::cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## **B. 2D Traveling**[枚举]

小猪生活在一个无限大的平面上，上面有直角坐标系。

平面上有$n$个城市，编号从$1$到$n$，前$k$个城市被定义为主要城市。第$i$个城市的坐标为$(x_i,y_i)$。

小猪作为一个经验丰富的旅行者，想在中考之后来一次轻松的旅行。目前，他在城市$a$，他想乘飞机去城市$b$。你可以在任意两个城市之间飞行，你也可以在旅行途中以任意顺序访问几个城市，但最终目的地必须是城市$b$。

由于主要城市之间的贸易非常活跃，因此可以免费乘坐飞机在这些城市之间旅行。形式上，两个城市$i$和$j$之间的机票价格$f(i,j)$定义如下：

$
f(i,j)= \begin{cases} 0, & \text{if cities }i \text{ and }j \text{ are both major cities} \\ |x_i-x_j|+|y_i-y_j|, & \text{otherwise} \end{cases}
$

小猪不想节省时间，但他想省钱。所以你需要告诉他，如果他可以乘坐任意数量的航班，那么所有机票总费用的**最小**值。

****

既然起点和终点是固定的 

而且很容易发现 在 没有主要城市的影响的时候 直飞最优

那么在有主要城市影响的时候 可以O(n)枚举一遍  ， 记录最佳答案

由此便可以得出答案

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m, k, a, b;
void solve() {
  std::cin >> n >> k >> a >> b;
  std::vector<std::array<int, 2>> arr(n);
  for (auto &[A, B] : arr) {
    std::cin >> A >> B;
  }
  int ans = 0x3f3f3f3f3f3f3f3f;
  if (a <= k and b <= k) {
    std::cout << 0 << '\n';
    return;
  } else {
    if (b <= k) {
      for (int i = 0; i < k; i++) {
        ans = std::min(ans, std::abs(arr[a - 1][0] - arr[i][0]) +
                                std::abs(arr[a - 1][1] - arr[i][1]));
      }
    } else if (a <= k) {
      for (int i = 0; i < k; i++) {
        ans = std::min(ans, std::abs(arr[b - 1][0] - arr[i][0]) +
                                std::abs(arr[b - 1][1] - arr[i][1]));
      }
    } else {
      ans = std::abs(arr[a - 1][0] - arr[b - 1][0]) +
            std::abs(arr[a - 1][1] - arr[b - 1][1]);
    }
  }
  int res0 = 0x3f3f3f3f3f3f3f3f;
  int res1 = 0x3f3f3f3f3f3f3f3f;
  for (int i = 0; i < k; i++) {
    res0 = std::min(res0, std::abs(arr[a - 1][0] - arr[i][0]) +
                              std::abs(arr[a - 1][1] - arr[i][1]));
    res1 = std::min(res1, std::abs(arr[b - 1][0] - arr[i][0]) +
                              std::abs(arr[b - 1][1] - arr[i][1]));
  }
  std::cout << std::min(ans, res0 + res1) << "\n";
}

signed main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(0), std::cout.tie(0);
  std::cout << std::fixed << std::setprecision(15);
  std::cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## C. Fill in the Matrix [构造]

有一个大小为 $n\times m$的空矩阵 $M$。

中考结束了，丹尼尔想做一些益智游戏。他要用长度为 $m$的排列填入矩阵 $M$。也就是说，$M$的每一行都必须是长度为$m^\dagger$的排列。

将$M$中第$i$列的值定义为$v_i=\operatorname{MEX}(M_{1,i},M_{2,i},\ldots,M_{n,i})^\ddagger$。因为丹尼尔喜欢多样性，所以$M$的美是$s=\operatorname{MEX}(v_1,v_2,\cdots,v_m)$。

你必须帮助丹尼尔填入矩阵$M$，并**最大**它的美。

$^\dagger$长度为$m$的排列是一个由$m$个不同的整数组成的数组，这些整数从$0$到$m-1$依次排列。例如，$[1,2,0,4,3]$是一个排列，但$[0,1,1]$不是一个排列（$1$在数组中出现了两次），$[0,1,3]$也不是一个排列（$m-1=2$但在数组中有$3$）。

$^\ddagger$数组的 $\operatorname{MEX}$是不属于数组的最小非负整数。例如，$\operatorname{MEX}(2,2,1)=0$是因为$0$不属于数组，而$\operatorname{MEX}(0,3,1,2)=4$是因为$0$、$1$、$2$和$3$出现在数组中，但$4$不属于数组。

****

我们 可以 通过手算来发现 

答案是 n+1 和 m 当中 最小那一个

然后 就可以分情况来讨论

首先肯定是 正方形的时候 最简单 想成一个n*n的矩阵

然后可以构造 第一列1~n 第二列 2~n,0,第二列 3~n,0,1

这样第i列没有i

于是 MEX（矩阵）就是 n+1

再转成长方形 

如果 高度小于宽度 就在 正方形的基础上砍去就好

可以证明是最优解

如果高度大于宽度，就重复构造与正方形区域相同的矩阵

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m;
#define debug(a) std::cerr << #a << "=" << a << std::endl
#define debug2(a, b) \
  std::cerr << #a << "=" << a << " " << #b << "=" << b << std::endl
#define debug3(a, b, c)                                                    \
  std::cerr << #a << "=" << a << " " << #b << "=" << b << " " << #c << "=" \
            << c << std::endl
void solve() {
  std::cin >> n >> m;
  // 每一行都是0~m-1的排列
  // 第一列没有0 第二列没有 1
  if (m == 1) {
    std::cout << 0 << "\n";
    for (int i = 0; i < n; i++) {
      std::cout << 0 << "\n";
    }
  } else {
    int ans_A = n + 1ll;
    int ans_B = m;
    std::vector<std::vector<int>> a(n + 10, std::vector<int>(m + 10));
    if (ans_A <= ans_B) {
      int idx = 0;
      for (int j = 0; j < m; j++) {
        idx = j;
        for (int i = 0; i < n; i++) {
          a[i][j] = (i + j) % m;
        }
      }
    } else {
      int idx = 0;
      int i;
      for (int j = 0; j < m; j++) {
        idx = j + 1;
        for (i = 0; i < n; i++) {
          if (idx == j) idx++;
          idx %= m;
          if (idx == j) break;
          a[i][j] = idx;
          (idx++);
          if (idx == j) break;
        }
      }
      for (int t = i + 1; t < n; t++) {
        a[t] = a[t % (i + 1)];
      }
    }
    std::cout << std::min(ans_A, ans_B) << "\n";
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        std::cout << a[i][j] << " ";
      }
      std::cout << "\n";
    }
  }
}

signed main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(0), std::cout.tie(0);
  std::cout << std::fixed << std::setprecision(15);
  std::cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

## D1. Candy Party (Easy Version)[二进制 贪心]
这是问题的简单版本。唯一不同的是，在这个版本中，每个人都必须向**个**人赠送糖果，并从**个**人那里接收糖果。请注意，提交的材料不能同时通过两个版本的问题。只有两个版本的问题都解决了，您才能进行破解。

中考结束后，丹尼尔和他的朋友们要开一个派对。每个人都会带一些糖果来。

派对上会有$n$个人。最初，第 $i$个人有 $a_i$颗糖果。在聚会期间，他们将交换糖果。为此，他们将按照任意顺序排成**队，每个人都将做以下**次**：

- 选择一个整数$p$（$1 \le p \le n$）和一个**非负**的整数$x$，然后把自己的$2^{x}$颗糖果分给$p$颗的人。要注意的是，一个人***给的糖果不能超过他目前拥有的糖果数量（他可能之前收到了别人给的糖果），而且他***不能**给自己糖果。

丹尼尔喜欢公平，所以只有当每个人都能从**个**人那里得到糖果时，他才会高兴。与此同时，他的朋友汤姆喜欢平均，所以只有当所有的人交换后得到的糖果数量相同时，他才会高兴。

请判断是否存在一种交换糖果的方法，使丹尼尔和汤姆在交换后都感到高兴。
****

每个数 都要经过 加一个$2^a$ 减一个$2^b$ 来 达到目标值

如果已经是目标值了那就是$a=b$

所以我们枚举每一个数 与 目标值  的差值

这个差值一定是`0111110`或者`0000000`这样的形式，要不然没法通过加一个$2^a$ 减一个$2^b$操作来实现

如果这个差值是正数  那么就是记录 [a] 位置可操作的 $2^a$ 增加一个 ， [b] 位置可操作的 $2^b$ 减少一个 

如果最后所有位置 的 可操作数都是 0 那么就是可以 操作得来

否则就无法通过操作得了

那么就可以编写我们的代码了

(PS) 不开ll WA3 

l,r写反 WA 2  和 WA 6

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m;
int idx;
void solve() {
  std::cin >> n;
  std::vector<int> a(n);
  // std::cout<<++idx<<"\n";
  for (auto &i : a) std::cin >> i;
  int sum = 0ll;
  for (auto it : a) sum += it;
  std::map<int, int> mp;
  int change_to = sum / n;
  if (sum % n) {
    std::cout << "No\n";
    return;
  } else {
    for (auto it : a) {
      int change = change_to - it;
      if (change > 0) {
        int cnt = 0;
        for (int i = 0; i < 64; i++) {
          if ((change >> i) & 1) cnt++;
        }
        int l, r;
        for (l = 63; l >= 0; l--) {
          if ((change >> l) & 1) break;
        }
        for (r = 0; r <= 63; r++) {
          if ((change >> r) & 1) break;
        }
        if (l - r + 1 == cnt) {
          mp[l + 1]--;
          mp[r]++;
        } else {
          std::cout << "No\n";
          return;
        }
      } else if (change < 0) {
        change = -change;
        int cnt = 0;
        for (int i = 0; i < 64; i++) {
          if ((change >> i) & 1) cnt++;
        }
        int l, r;
        for (l = 63; l >= 0; l--) {
          if ((change >> l) & 1) break;
        }
        for (r = 0; r <= 63; r++) {
          if ((change >> r) & 1) break;
        }
        if (l - r + 1 == cnt) {
          mp[l + 1]++;
          mp[r]--;
        } else {
          std::cout << "No\n";
          return;
        }
      }
    }
  }
  for (int i = 0; i <= 64; i++) {
    if (mp[i] != 0) {
      std::cout << "No\n";
      return;
    }
  }
  std::cout << "Yes\n";
}

signed main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(0), std::cout.tie(0);
  std::cout << std::fixed << std::setprecision(15);
  std::cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  int __;
  std::cin >> __;
  while (__--) solve();
  return 0;
}
```

