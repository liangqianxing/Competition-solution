[toc]

# solve

## **A**

不同的游戏有着不同的开始提示，像Are you really?Go!或者Can you start?。再一些大型的比赛中也有一些提示语，像Wa,Ac.等等。所以现在要求你输出一句提示语:Do you want to play ACM?(yes\\no)

**code**

```PHP
print Do you want to play ACM?(yes\no)
```

## **B**

小蓝给学生们组织了一场考试，卷面总分为100分，每个学生的得分都是一个0到100的整数。

请计算这次考试的最高分、最低分和平均分。

**code**

```C++
using ld = long double;
int n, m;
void solve() {
  cin >> n;
  vector<int> a(n);
  for (auto &i : a) cin >> i;
  ld ans = 0;
  int mn = INT_MAX, mx = INT_MIN;
  for (auto it : a) mn = min(it, mn), mx = max(mx, it), ans += it;
  ans /= n;
  cout << mx << "\n" << mn << "\n" << ans << "\n";
}
```

## **C**

给你一个单词，要你找到出现最多的字母和这个字母出现的次数。

单词只包含英文小写字母。

**code**

```C++
int n, m, cnt;
char a;
void solve() {
  string s;
  cin >> s;
  map<char, int> mp;
  for (auto it : s) mp[it]++;
  for (char i = 'a'; i <= 'z'; i++)
    if (mp[i] > cnt) a = i, cnt = mp[i];
  cout << a << "\n" << cnt << "\n";
}
```

## **D**

小财向你题出了请求，让你帮他读一下每个数。  

当整数为负数时，先输出 fu 字。  
十个数字对应的拼音如下：  
0: ling 
1: yi 
2: er 
3: san
4: si 
5: wu 
6: liu 
7: qi 
8: ba 
9: jiu

**code**

```C++
map<char, string> mp;
string s;
void solve() {
  mp['-'] = "fu", mp['0'] = "ling", mp['1'] = "yi", mp['2'] = "er",
  mp['3'] = "san", mp['4'] = "si", mp['5'] = "wu", mp['6'] = "liu",
  mp['7'] = "qi", mp['8'] = "ba", mp['9'] = "jiu";
  cin >> s;
  for (auto it : s) cout << mp[it] << " ";
}
```

## **E**

羊蝎子特别喜欢喝咖啡，一天不喝就难受。有一天LakerV路过咖啡店，他让LakerV顺手帮他点1杯价格**A**元的Java。

都2030年了，LakerV居然不会用手机支付。他有**n**张的5元和**无限数量**的1元纸币。他支付有个习惯，就是要让自己支付的总纸币数量尽可能少。

现在他告诉了你A和n。想让你猜猜他支付了多少张5元纸币和1元纸币。猜对了，他也会请你喝一杯咖啡。

**think**

肯定是有5 且能用 5的 情况下 一直用5

然后 用 1 补全余数

**code**

```c++
int n, a;
void solve() {
  cin >> n >> a;
  int cnt5 = min(n, a / 5);
  a -= 5 * cnt5;
  cout << cnt5 << " " << a << "\n";
}
```

## **F**

给出一个仅包含 a,b 的字符串 A。在 A 中间任意位置（包括开头结尾）插入一个字符，最大化 aab 作为子序列（可以不连续）在 A 中出现的次数。

**think**

操作一共有两种

1. 放 $b$ 此时在末尾放最优
2. 放 $a$ 此时在开头放最优

然后就两种情况取最大值

每一种情况分别计算
$$
\begin{align}
i &\in [0,size_s) \\
if\qquad s_i&=b\qquad 以本位结尾的aab字符串数量为 \binom{cnt_a}{2}\\
if\qquad s_i&=a\qquad cnt_a+1
\end {align}
$$
然后计算就好

记得开`ULL` 不开扣15分

**code**

```C++
#define int unsigned long long
int n, m;
void solve() {
  string s;
  cin >> s;
  int ans = 0, cnta = 0, cntb = 0;
  for (auto it : s)
    if (it == 'b')
      ans += (cnta) * (cnta - 1) / 2;
    else if (it == 'a')
      cnta++;
  int f1 = (cnta) * (cnta - 1) / 2, f2 = 0, cnta = 1;
  for (auto it : s)
    if (it == 'b')
      f2 += (cnta) * (cnta - 1) / 2;
    else if (it == 'a')
      cnta++;
  cout << max(f2, f1 + ans) << "\n";
}
```

## **G**

朝阳在自己生日那天和小伙伴买了一些贴纸，为了迎接生日派对，朝阳打算把自己的房间里的一面墙打扮一下，现在朝阳把他的墙转化成二维直角坐标系的第一象限。他会告诉他的小伙伴也就是你，应该把贴纸按照购买的顺序依次贴在坐标系上，最后朝阳会询问你一次，关于墙上的坐标点（x，y），最后贴着的是第几次购买的贴纸，请注意你可以认为朝阳的墙无限大。

第一行输入一个整数$n(1\le n\le10^5)$，代表共有n张贴纸。  
接下来的n行中，第$i+1$行代表第i次购买的贴纸信息。它包括四个整数$a,b,c,d(0\le a,b\le10^5,1\le c,d\le10^3)$整数之间使用空格隔开，它代表这张贴纸的左下角应该贴在(a，b)点以及贴纸在x轴的长度c和在y轴的长度d。请注意贴纸的边缘以及四个最边界的点都算被这个贴纸覆盖住了。  

第n+2行输入两个整数$x,y(0\le x,y\le10^9)$。代表朝阳想要知道(x，y)坐标点最后贴的贴纸是第几次购买的。

**think**

思路就是将所有的 贴纸坐标存起来

不要 暴力维护坐标系上的 $id$

最后一次匹配时$O(n)$匹配就好

**code**

```C++
#define int long long
int n;
struct p {
  int a, b, c, d, id;
  p(int a = 0, int b = 0, int c = 0, int d = 0, int e = 0)
      : a(a), b(b), c(c), d(d), id(e) {}
};
void solve() {
  auto contain = [&](int x, int y, p i) -> bool {
    if (x <= i.c and x >= i.a and y <= i.d and y >= i.b)
      return true;
    else
      return false;
  };
  cin >> n;
  p a[n + 1];
  for (int i = 1; i <= n; i++) {
    int A, b, c, d;
    cin >> A >> b >> c >> d;
    c += A, d += b;
    a[i] = {A, b, c, d, i};
  }
  int x, y;
  cin >> x >> y;
  for (int i = n; i >= 0; i--) {
    if (contain(x, y, a[i]) and i) {
      cout << a[i].id << "\n";
      auto it = a[i];
      for (int j = i; j < n; j++) {
        a[j] = a[j + 1];
      }
      a[n] = it;
      break;
    } else if (i == 0) 
      cout << "-1\n";
  }
}
```

## **H**

给定公元2000年到公元3000年之间的某一天，请你给出该天的前天是哪一天.

<img src="https://uploadfiles.nowcoder.com/images/20201205/341879_1607170832681/CC97A3877A03DFA5CBA4F8A4A46F0352" style="zoom:50%;" />

**think**

如果天数够减就直接输出

不够的话就向前一个月借

然后套个年月判断天数的函数就好

**code**

```C++
int getd(int y, int m) {
  if (m == 2) {
    if ((y % 4 == 0 && y % 100 != 0) || y % 400 == 0)
      return 29;
    else
      return 28;
  } else {
    if (m == 1 or m == 3 or m == 5 or m == 7 or m == 8 or m == 10 or m == 12)
      return 31;
    else
      return 30;
  }
}
void solve() {
  int y, m, d;
  scanf("%lld-%lld-%lld", &y, &m, &d);
  d -= 2;
  if (d >= 1)
    printf("%04lld-%02lld-%02lld\n", y, m, d);
  else {
    m--;
    if (m <= 0) y--, m = 12;
    d += getd(y, m);
    printf("%04lld-%02lld-%02lld\n", y, m, d);
  }
}
```

## **I**

有一只可爱的兔子被困在了密室了，密室里有两个数字，还有一行字：  
只有解开密码，才能够出去。  
可爱的兔子摸索了好久，发现密室里的两个数字是表示的是一个区间\[L,R\]  
而密码是这个区间中任意选择两个(可以相同的)整数后异或的最大值。  
比如给了区间\[2,5\] 那么就有2 3 4 5这些数，其中 2 xor 5=7最大 所以密码就是7。  
兔子立马解开了密室的门，发现门外还是一个门，而且数字越来越大，兔子没有办法了，所以来求助你。  
提示：异或指在二进制下一位位比较，相同则 0 不同则 1  
例如$2=(010)_2$ $5=(101)_2$  
所以2 xor $5=(111)_2=7$

**think**

我们发现

对于$l\ r$ 如果 有一位他俩不相同

那么后面的位数一定可以取到不相同的 

因为总可以取形如

`100000`与 `011111`

这样的

所以我们就在$log(r)$的时间内判断有没有

如果全都相同那就是$0$

**code**

```C++
void solve() {
  int l, r;
  cin >> l >> r;
  int ans = 0;
  for (int i = 60; i >= 0; i--) {
    if (((l >> i) & 1) != ((r >> i) & 1)) {
      ans = (1LL << (i + 1)) - 1;
      break;
    }
  }
  cout << ans << "\n";
}
```

## **K**

牛妹有一张连通图，由n个点和n-1条边构成，也就是说这是一棵树，牛妹可以任意选择一个点为根，根的深度$dep_{root}$为0，对于任意一个非根的点，我们将他到根节点路径上的第一个点称作他的父节点，例如1为根，1-4的；路径为1-3-5-4时，4的父节点是5，并且满足对任意非根节点，$dep_i=dep_{fa_i}+1$，整棵树的价值$W=\sum\limits_{i=1}^n dep_i$，即所有点的深度和  

牛妹希望这棵树的W最小，请你告诉她，选择哪个点可以使W最小

**think**

那么这道题就是换根DP的模板题

我们先选中一个位置求贡献

$O(n)$时间内解决

再$O(n)$换根求贡献

由此便能解决

**code**

```C++
#define int long long
int n, m;
vector<vector<int>> G;
void solve() {
  cin >> n;
  G.resize(n);
  vector<int> siz(n, 1);
  vector<int> ans(n);
  vector<int> dep(n + 1);
  for (int i = 1; i < n; i++) {
    int x, y;
    cin >> x >> y;
    x--, y--;
    G[x].emplace_back(y);
    G[y].emplace_back(x);
  }
  auto dfs1 = [&](auto self, int u, int fa) -> void {
    if (u != 0) dep[u] = dep[fa] + 1;
    for (auto it : G[u]) {
      if (it == fa) continue;
      self(self, it, u);
      siz[u] += siz[it];
    }
  };
  auto dfs2 = [&](auto self, int u, int fa) -> void {
    for (auto it : G[u]) {
      if (it == fa) continue;
      ans[it] = ans[u] - siz[it] + (siz[0] - siz[it]);
      self(self, it, u);
    }
  };

  dfs1(dfs1, 0, n);
  for (int i = 0; i < n; i++) ans[0] += dep[i];
  dfs2(dfs2, 0, n);
  cout << *min_element(begin(ans), end(ans)) << "\n";
}
```

