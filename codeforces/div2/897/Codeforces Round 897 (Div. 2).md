[toc]

# Codeforces Round 897 (Div. 2)

[Codeforces Round 897 (Div. 2)](https://codeforces.com/contest/1867)

## A. green_gold_dog, array and permutation[排序]

green_gold\_dog 有一个长度为 $n$的数组 $a$，他想找到一个长度为 $n$的排列组合 $b$，使得数组 $a$和排列组合 $b$之间的元素差中的不同数的个数最大。

长度为 $n$的排列是由 $n$个不同的整数组成的数组，这些整数从 $1$到 $n$依次排列。例如，$[2,3,1,5,4]$是一个排列，但$[1,2,2]$不是排列（因为$2$在数组中出现了两次），$[1,3,4]$也不是排列（因为$n=3$，但$4$出现在数组中）。

长度为 $n$的两个数组 $a$和 $b$的元素向差是长度为 $n$的数组 $c$，其中 $c_i$ = $a_i - b_i$（$1 \leq i \leq n$）。

---

**题目要找什么？**

使得数组 $a$和排列组合 $b$之间的元素差中的不同数的个数最大。

就是让数组 $a$和排列组合 $b$ 以一个特定的排序，使得两两相减不同的数最多

数组$a$是给定的，$b$是从1到n的数列

那么我们可以让$a$排序

使得所有$a_i\le a_{i+1}$

我们让$b$逆序排列

那么所有$b_{i}>b_{i+1}$

那么$a_i-b_i<a_{i+1}-b_{i+1}$

那么此时所有的差都是不同的，为最大值n个

由此便可以解决

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
  std::cin >> n;
  std::vector<int> a(n);
  for (auto &i : a) std::cin >> i;
  std::map<int, int> mp;
  std::vector<std::array<int, 3>> b;
  for (int i = 0; i < n; i++) {
    b.push_back({a[i], i, 0});
  }
  sort(begin(b), end(b),std::greater<std::array<int, 3>>());
  for (int i = 0; i < n; i++) {
    b[i][2] = i;
  }
  sort(begin(b), end(b),
       [&](std::array<int, 3> A, std::array<int, 3> B) -> bool {
         return A[1] < B[1];
       });
  for (auto [x, y, z] : b) {
    std::cout << z + 1 << " ";
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

## B. XOR Palindromes[二进制，回文串，思维]

给你一个长度为$n$的二进制数串$s$（一个只由$0$和$1$组成的数串）。如果存在一个长度为$n$的二进制字符串$l$，其中包含$x$个1，那么如果每个符号$s_i$都被$s_i \oplus l_i$替换（其中$\oplus$表示[位XOR操作](https://en.wikipedia.org/wiki/Bitwise_operation#XOR)），那么字符串$s$就变成了一个回文，那么数字$x$就是好数。

您需要输出长度为 $n+1$的二进制字符串 $t$，其中如果数字 $i$是好的，则 $t_i$（$0 \leq i \leq n$）等于 $1$，否则等于 $0$。

回文字符串是指从左往右读与从右往左读相同的字符串。例如，01010、1111、0110 都是回文。

---

研究一下题意

如果一个 $x$ 能够存在 一个长度为$n$的二进制字符串$l$，其中包含$x$个1，每个符号$s_i$都被$s_i \oplus l_i$替换，使得字符串$s$变成了一个回文，那么数字$x$就是好数，就可以在二进制字符串 $t$中将 $t_i$设为 $1$

如果$s$等于$101011$

那么答案是$0010100$

- $t_2 = 1$因为我们可以选择 010100，那么字符串$s$就变成了 111111，这是一个回文。
- $t_4 = 1$因为我们可以选择 101011.  那么字符串$s$就变成了 000000，这也是一个回文。
- 可以证明，对于所有其他的$i$，都没有答案，所以剩下的符号是$0$。

所以答案是$0010100$

---

**思路**

对n进行分类讨论

**如果n为偶数时**

我们思考如何最低限度的将字符串弄成回文串

如果$s$等于$101011$

那么有两个地方 与对应的地方不一样

那么就需要两个花费让 它改变 ，那么答案的第二位就是1，因为2是可行的

剩下的 1 1对应的我们可以用 0 0来替换 ，需要两个花费 ，2+2=4，那么第四位就是1 因为4是可行的

那么就得到了思路

先求的变成回文串的最小花费，再将先前已经对称的字符 变成相反的 比如(0,0)->(1,1)还是对称的

**如果n为奇数时**

那么中间的数跟自身对称，那么变不变都行，剩下的部分就和偶数相同了

```c++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
#define int long long
int n, m;
void solve() {
  std::cin >> n;
  std::string s;
  std::cin >> s;
  if (n & 1) {
    int cnt = n / 2;
    int cost = 0;
    for (int i = 0; i < cnt; i++) {
      if (s[i] != s[n - i - 1]) {
        cost++;
      }
    }
    std::string ans = std::string(n + 1, '0');
    for (int idx = cost; idx <= cost + (cnt - cost) * 2 + 1; idx++) {
      ans[idx] = '1';
    }
    std::cout << ans << std::endl;
  } else {
    int cnt = n / 2;
    int cost = 0;
    for (int i = 0; i < cnt; i++) {
      if (s[i] != s[n - i - 1]) {
        cost++;
      }
    }
    std::string ans = std::string(n + 1, '0');
    for (int idx = cost; idx <= cost + (cnt - cost) * 2; idx += 2) {
      ans[idx] = '1';
    }
    std::cout << ans << std::endl;
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

## C. Salyg1n and the MEX Game[交互，贪心，猜结论]

**这是一个交互问题！**

salyg1n 给了爱丽丝一个由 $n$个不同整数 $s_1, s_2, \ldots, s_n$（$0 \leq s_i \leq 10^9$）组成的集合 $S$。爱丽丝决定用这一组数与鲍勃玩一个游戏。游戏规则如下

- 双方轮流下棋，爱丽丝先下。
- 在一次移动中，爱丽丝在集合$S$中增加一个数字$x$（$0 \leq x \leq 10^9$）。在移动时，集合$S$必须不包含数字$x$。
- 在一次移动中，鲍勃从集合$S$中删除了一个数字$y$。在移动时，集合$S$必须包含数字$y$。此外，数字$y$必须严格小于爱丽丝添加的最后一个数字。
- 当鲍勃无法下棋或下了$2 \cdot n + 1$步棋后，对局结束（在这种情况下，爱丽丝的棋步将是最后一步）。
- 对局结果为$\operatorname{MEX}\dagger(S)$（对局结束时为$S$）。
- 爱丽丝的目标是使结果最大化，而鲍勃的目标是使结果最小化。

让$R$成为双方都以最优方式下棋时的结果。**在这个问题中，你将扮演爱丽丝，与扮演鲍勃的陪审员程序对弈。**你的任务是为爱丽丝实施一种策略，使博弈结果总是至少为$R$。

$\dagger$一个整数集合$c_1, c_2, \ldots, c_k$中的$\operatorname{MEX}$被定义为在集合$c$中不出现的最小非负整数$x$。例如，$\operatorname{MEX}(\{0, 1, 2, 4\})$$=$ $3$.

---

这个题我们可以猜一下

一般交互题 都是二分 或者 有简单解

这个题没有看出二分的单调性，那我们就猜有简单解

看这一条

- 在一次移动中，鲍勃从集合$S$中删除了一个数字$y$。在移动时，集合$S$必须包含数字$y$。此外，数字$y$必须严格小于爱丽丝添加的最后一个数字。

那么爱丽丝插入一个数字，鲍勃一定删除的比它更小 ，那么一轮下来就会减小$MEX$

所以我们要让回合更快结束

在**没有0**的情况下我们可以发现 插入0可以直接结束游戏

所以如果没有0我们就直接插入0

那**如果有0**呢？

那么鲍勃一定会删除一个数字，如果添加别的数字一定会让$MEX$减小,只有加上当前他删去的数字才可以让$MEX$不减小

那么每一次我们都补上他删去的数字

又由于n之和不超过 $10^5$

所以我们不用担心超时（计算机一秒可以运行 $10^8--10^9$的数据

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
	std::cin>>n;
	std::set<int>S;
	for(int i=0;i<n;i++){
		int x;std::cin>>x;
		S.insert(x);
	}
	if(*S.begin()==0){
		for(int i=0;i<1e5+10;i++){
			if(!S.count(i)){
				S.insert(i);
				std::cout<<i<<std::endl;
				break;
			}
		}
		int y=0;
		while(std::cin>>y,y!=-1 and y!=-2){
			std::cout<<y<<std::endl;
		}
		if(y==-1 or y==-2){
			return;
		}
	}else{
		std::cout<<0<<std::endl;
		int y=0;
		std::cin>>y;
		std::cout<<std::endl;
	}
}

signed main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(0), std::cout.tie(0);
  std::cout << std::fixed << std::setprecision(15);
  std::cerr << "Time elapsed: " << 1.0 * clock() / CLOCKS_PER_SEC << " s.\n";
  int __;
  std::cin >> __;
  while (__--) 
  	solve();
  return 0;
}
```

## D. Cyclic Operations[基环树 建图[下标与值]]

Egor 有一个长度为 $n$的数组 $a$，最初由零组成。然而，他想把它变成另一个长度为 $n$的数组 $b$。

由于埃戈尔不走捷径，所以只能使用下面的操作（可能是零次或多次）：

- 选择一个长度为$k$的数组$l$（$1 \leq l_i \leq n$，所有的$l_i$都是**分明的**），并将每个元素$a_{l_i}$改为$l_{(i\%k)+1}$（$1 \leq i \leq k$）。

他开始关注是否有可能只用这些操作就得到数组 $b$。由于 Egor 还是个程序员初学者，他请求您帮助他解决这个问题。

运算$\%$表示取余数，即$a\%b$等于数$a$除以数$b$的余数。

---

首先 有index(下标)和value(值)之间进行赋值操作的我们都可以在下标和值之间进行建边

由于一个下标可以对应一个值，但一个值会对应很多下标

那我们从下标向值进行建有向边

这样可以使得遍历的时候最后汇合在一块

否则的话就会变成森林，不好进行处理

通过手算我们可以发现建成的树 是有一个环的树

那么这种树叫做基环树

那么这个题跟基环树有什么关系呢

在这个题中，选择一个长度为$k$的数组$l$，并将每个元素$a_{l_i}$改为$l_{(i\%k)+1}$是操作

那么我们显然可以将$n-1$个位置填充到目标值

注意这个数组取值没有限制

操作是以当前的数为下标，下一个数为价值，正好与下标和价值的建边联系起来了

又因为基环树可以看成一个环上连着很多长链，对于每一个长链，他都可以将除了与环相接的点分配好值

那么剩下就是能不能给环分配好值

又因为在环的第一个的值是最后一个数列的目标值时才能恰好满足

那么所以当环的大小是$k$时才能恰好分配

又因为当$k$是1时，无法使用操作将链填充好，所以需要判断每一个点能否到达目标值

以上就是这个题的思路

**code**

```C++
#pragma GCC optimize(                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           \
    "-fdelete-null-pointer-checks,inline-functions-called-once,-fexpensive-optimizations,-foptimize-sibling-calls,-ftree-switch-conversion,-finline-small-functions,inline-small-functions,-frerun-cse-after-loop,-fhoist-adjacent-loads,-findirect-inlining,-freorder-functions,no-stack-protector,-fpartial-inlining,-fsched-interblock,-fcse-follow-jumps,-falign-functions,-fstrict-aliasing,-fschedule-insns2,-ftree-tail-merge,inline-functions,-fschedule-insns,-freorder-blocks,-funroll-loops,-fthread-jumps,-fcrossjumping,-fcaller-saves,-fdevirtualize,-falign-labels,-falign-loops,-falign-jumps,unroll-loops,-fsched-spec,-ffast-math,Ofast,inline,-fgcse,-fgcse-lm,-fipa-sra,-ftree-pre,-ftree-vrp,-fpeephole2", \
    3, 2)
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include <bits/stdc++.h>
int n, m;
void solve() {
  std::cin >> n >> m;
  std::vector<int> p(n);
  for (auto &i : p) {
    std::cin >> i;
    i--;
  }
  if (m == 1) {
    int idx = 0;
    for (int i = 0; i < n; i++) {
      if (p[i] != i) {
        std::cout << "NO\n";
        return;
      }
    }
    std::cout << "YES\n";
    return;
  }
  std::vector<short> vis(n);
  for (int i = 0; i < n; i++) {
    if (vis[i]) continue;
    int v = i;
    while (!vis[v]) {
      vis[v] = 1;
      v = p[v];
    }
    if (vis[v] == 1) {
      int u = v;
      int cnt = 0;
      do {
        ++cnt;
        u = p[u];
      } while (u != v);
      if (cnt != m) {
        std::cout << "NO\n";
        return;
      }
    }
    for (int j = i; vis[j] != 2; j = p[j]) vis[j] = 2;  // do huan
  }
  std::cout << "YES\n";
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
