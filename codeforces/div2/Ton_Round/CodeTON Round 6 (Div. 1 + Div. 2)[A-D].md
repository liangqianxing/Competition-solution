#! https://zhuanlan.zhihu.com/p/657133728
# CodeTON Round 6 (Div. 1 + Div. 2)[A-D]

[CodeTON Round 6 (Div. 1 + Div. 2, Rated, Prizes!)](https://codeforces.com/contest/1870)

## A. MEXanized Array[分类讨论]

给你三个非负整数$n$，$k$和$x$。求一个由非负整数组成的数组中元素的最大可能和，该数组有 $n$个元素，其 MEX 等于 $k$，且所有元素不超过 $x$。如果不存在这样的数组，则输出 $-1$。

数组的 MEX（最小排除数）是不属于数组的最小非负整数。例如

-  $[2,2,1]$的 MEX 是$0$，因为$0$不属于数组。
-  $[3,1,0,1]$的 MEX 为$2$，因为$0$和$1$属于数组，而$2$不属于数组。
-  $[0,3,1,2]$的 MEX 为$4$，因为$0$、$1$、$2$和$3$属于数组，而$4$不属于数组。

****

我们分析一下题目

要让其 MEX 等于 $k$ ，那么必须有$0$~ $k-1$对吧

那就必须填充满$0$~ $k-1$

又因为填充的数都是小于 $x$ 的

所以 $x<k-1$的时候是无法填充完的

又因为$0$~ $k-1$是$k$个数 如果$n<k$ 的话也没法填充完整

那么满足上面的条件就能填充完整，那么我们来满足一下最大值

当$k=x$时 剩下的地方就不能填充$x$了因为此时 其 MEX 会变成$k+1$

当$k≠x$时 剩下的地方就全部填充$x$了 对他的MEX没有影响

由此便可以解决

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m, k, x;
void solve() {
  std::cin >> n >> k >> x;
  int sum = 0;
  if (x < k - 1) {
    std::cout << "-1\n";
    return;
  }
  if (k > n) {
    std::cout << "-1\n";
  } else {
    sum += (k - 1) * k / 2;
    if (k == x) {
      sum += (n - k) * (x - 1);
    } else {
      sum += (n - k) * x;
    }
    std::cout << sum << "\n";
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

## B. Friendly Arrays[贪心，思维，|的性质]

给你两个整数数组 - 长度为 $n$的 $a_1, \ldots, a_n$和长度为 $m$的 $b_1, \ldots, b_m$。你可以从数组 $b$（$1 \leq j \leq m$）中选择任意元素 $b_j$，并对所有*** $1 \leq i \leq n$执行 $a_i = a_i | b_j$。您可以执行任意数量的此类操作。

完成所有运算后，将计算出 $x = a_1 \oplus a_2 \oplus \ldots \oplus a_n$ 的值。求进行任意一组运算后可得到的 $x$的最小值和最大值。

以上，$|$是【位顺 OR 运算】(https://en.wikipedia.org/wiki/Bitwise_operation#OR)，$\oplus$是【位顺 XOR 运算】(https://en.wikipedia.org/wiki/Bitwise_operation#XOR)。

****

那么我们按位拆分来看，某一位|上一个数 一定不变或变大

那么我们通过使用所有的$b_i$可以将原数组的每一位变得尽可能的大

然后就是进行异或操作

**如果$n$是偶数的话**

偶数个1异或起来会变成0，那么对原数组操作会使得最后的值不变或者变小

因此最大值就是不进行操作的值，最小值就是全部进行操作的值

**如果$n$是奇数的话**

奇数个1异或起来会变成1，那么对原数组操作会使得最后的值不变或者变大

因此最小值就是不进行操作的值，最大值就是全部进行操作的值

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m;
void solve() {
  std::cin >> n >> m;
  std::vector<int> a(n);
  for (auto &i : a) {
    std::cin >> i;
  }
  std::vector<int> b(m);
  for (auto &j : b) {
    std::cin >> j;
  }
  // 如果n是奇数肯定进行所有操作最大
  if (n & 1) {
    int res = 0;
    for (auto it : b) {
      res |= it;
    }
    int ans = 0;
    int ans_min = 0;
    for (auto it : a) {
      int res_now = it | res;
      ans ^= res_now;
      ans_min ^= it;
    }
    std::cout << ans_min << " " << ans << "\n";
  } else {
    // 如果是偶数那么操作可以变小
    int res = 0;
    for (auto it : b) {
      res |= it;
    }
    int ans = 0;
    int ans_MAX = 0;
    for (auto it : a) {
      int res_now = it | res;
      ans ^= res_now;
      ans_MAX ^= it;
    }
    std::cout << ans << " " << ans_MAX << "\n";
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

## C. Colorful Table[单调性,双指针]

给你两个整数 $n$ 和 $k$。同时给你一个大小为 $n$的整数数组 $a_1, a_2, \ldots, a_n$。已知对于所有 $1 \leq i \leq n$， $1 \leq a_i \leq k$.

定义大小为 $n \times n$的二维数组 $b$如下：$b_{i, j} = \min(a_i, a_j)$.将数组$b$表示为一个正方形，其中左上角的单元格为$b_{1, 1}$，行的编号从上到下从$1$到$n$，列的编号从左到右从$1$到$n$。让一个单元格的颜色就是写在其中的数字（坐标为$(i, j)$的单元格的颜色是$b_{i, j}$）。

对于从 $1$ 到 $k$ 的每种颜色，找出数组 $b$ 中包含该颜色所有单元格的最小矩形。输出该矩形的宽和高之和。

****

这个题的trick就是看出单调性

显然通过观察答案可以看出存在于原数组的数中

他的矩形的大小是依次减小的，我们也可以证明出来

假设$a<b$ 那么$b$作用的地方，$a$也一定可以作用

所以矩阵的大小一定是单调不减的

然后我们用双指针单调的扫一遍就好

`注意：没出现的数不用覆盖，输出0就好`

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m, k;
void solve() {
  std::cin >> n >> k;
  std::vector<int> a(n);
  std::set<int> S;
  for (auto &i : a) {
    std::cin >> i;
    S.insert(i);
  }
  int l = 0, r = n - 1;
  for (int i = 1; i <= k; i++) {
    if (!S.count(i)) {
      std::cout << 0 << " ";
      continue;
    }
    while (l <= r) {
      if (a.at(l) < i) {
        l++;
      } else if (a.at(r) < i) {
        r--;
      } else {
        std::cout << (r - l + 1) * 2 << " ";
        break;
      }
    }
  }
  std::cout << "\n";
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

## D. Prefix Purchase[后缀，单调性]

你有一个数组 $a$，大小为 $n$，初始填充为零（$a_1 = a_2 = \ldots = a_n = 0$）。你还有一个大小为 $n$的整数数组 $c$。

最初，你有$k$个硬币。通过支付$c_i$个硬币，你可以将$1$个元素添加到数组的$a$个元素中，从第一个元素到$i$个元素（所有$1 \leq j \leq i$个元素为$a_j \mathrel{+}= 1$个）。您可以购买任意 $c_i$ 次。只有在$k \geq c_i$的情况下才可能购买，也就是说在任何时刻$k \geq 0$都必须成立。

求可以得到的最大数组 $a$。

当且仅当在$a$和$b$不同的第一个位置上，数组$a$中的元素小于$b$中的相应元素时，数组$a$在词典上小于相同长度的数组$b$。

****

我们首先要保证前面的次数最多

其次次数一样的情况下让后面的每一位尽可能的大

如果一个后面位置的消耗大于前面的位置，但是可以保证购买的次数一样，我们就购买后面的

这个题`jiangly`是对每一位进行分别考虑了

维护每一位最优，便可以维护整体最优

每个位置都维护一个后缀最小值 ，然后用最小值的次数进行购买

**code**

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, k;
void solve() {
  std::cin >> n;
  std::vector<int> a(n);
  for (auto &i : a) std::cin >> i;
  std::vector<int> minn(n);
  minn.at(n - 1) = a.back();
  for (int i = n - 2; i >= 0; i--) {
    minn.at(i) = std::min(a.at(i), minn.at(i + 1));
  }
  std::vector<int> ans(n);
  std::cin >> k;
  int tmp = k;
  for (int i = 0; i < n; i++) {
    int cost = minn.at(i) - (i == 0 ? 0 : minn.at(i - 1));
    if (cost > 0) {
      tmp = std::min(tmp, k / cost);
    }
    k -= tmp * cost;
    a[i] = tmp;
    std::cout << a[i] << " \n"[i == n - 1];
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

