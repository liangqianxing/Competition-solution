# [E-外接圆](https://ac.nowcoder.com/acm/contest/69555/E)

在平面直角坐标系中，有 $n$ 个整点。整点是指横纵坐标都是整数的点。  

你每次可以任意选择一个点，并将其移动到坐标系中的任意位置。**移动后的坐标可以是实数。**  

请问至少需要移动多少次，才能使得所有点共圆。

输入的第一行为一个正整数 $n (3 \leq n \leq 100)$，表示点的数量。  

随后 $n$ 行，每行两个空格分隔的整数 $x (-100 \leq x \leq 100)$ 和 $y (-100 \leq y \leq 100)$，表示一个点的初始坐标。

**think**

看到这个数据范围我们就想到了一个$O(n^4)$的做法

枚举三个点确定圆 然后 $O(n)$ 枚举其余点在不在圆上

那么这个题就做完啦

没错就是这样

剩下的都是一些小trick 和高中数学了

**code**

```C++
// NowCoder 外接圆
// https://ac.nowcoder.com/acm/contest/69555/E 2024-03-21 00:15:11

#include <bits/stdc++.h>
using namespace std;
#define int long long
int n, m;
using ld = long double;
using pii = pair<ld, ld>;
vector<pii> p;
constexpr ld eps = 1E-6;
ld cross(int i, int j) {
  return (p[i].first * p[j].second - p[i].second * p[j].first);
}
ld cross(pii i, pii j) { return (i.first * j.second - i.second * j.first); }
bool same(ld a, ld b) {
  if (fabs(a - b) <= eps)
    return true;
  else
    return false;
}
pii sub(pii a, pii b) {
  pii c;
  c.first = a.first - b.first;
  c.second = a.second - b.second;
  return c;
}
bool is_line(int i, int j, int k) {
  if (same(cross(sub(p[j], p[i]), sub(p[k], p[i])), 0))
    return true;
  else
    return false;
}
ld get_dist(ld a, ld b, ld c, ld d) {
  return (a - c) * (a - c) + (b - d) * (b - d);
}
map<pii, int> mp;
void solve() {
  cin >> n;
  int _n = n;
  for (int i = 0; i < n; i++) {
    int x, y;
    cin >> x >> y;
    if (not mp.count({x, y})) p.emplace_back(x, y);
    mp[{x, y}]++;
  }
  n = p.size();
  int ans = 0;
  bool falg = false;
  for (int i = 0; i < n; i++) {
    for (int j = i + 1; j < n; j++) {
      for (int k = j + 1; k < n; k++) {
        if (is_line(i, j, k)) {
          falg = true;
          continue;
        } else {
          ld x1 = p[i].first;
          ld x2 = p[j].first;
          ld x3 = p[k].first;
          ld y1 = p[i].second;
          ld y2 = p[j].second;
          ld y3 = p[k].second;
          ld a = p[i].first - p[j].first;
          ld b = p[i].second - p[j].second;
          ld c = p[i].first - p[k].first;
          ld d = p[i].second - p[k].second;
          ld e = ((x1 * x1 - x2 * x2) - (y2 * y2 - y1 * y1)) / 2;
          ld f = ((x1 * x1 - x3 * x3) - (y3 * y3 - y1 * y1)) / 2;
          ld x = (e * d - b * f) / (a * d - b * c);
          ld y = (a * f - e * c) / (a * d - b * c);
          ld r = get_dist(x, y, x1, y1);
          int cnt = 0;
          for (int l = 0; l < n; l++)
            if (l == i)
              continue;
            else if (l == j)
              continue;
            else if (l == k)
              continue;
            else if (same(get_dist(p[l].first, p[l].second, x, y), r))
              cnt += mp[p[l]];

          int cur = mp[p[i]] + mp[p[j]] + mp[p[k]];
          ans = max(cnt + cur, ans);
        }
      }
    }
  }
  for (int i = 0; i < n; i++)
    for (int j = i + 1; j < n; j++) ans = max(ans, mp[p[i]] + mp[p[j]]);
  for (int i = 0; i < n; i++) ans = max(ans, mp[p[i]]);

  cout << _n - ans << "\n";
}

signed main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  solve();
  return 0;
}
```

