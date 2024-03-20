如果至少存在一个长度为 $k$ 的子串 $^\dagger$ 不是回文 $^\ddagger$ ，则称字符串 $t$ 为 $k$ （\-good）。让 $f(t)$ 表示所有 $k$ 的值之和，使得字符串 $t$ 是 $k$ \-good。

给你一个长度为 $n$ 的字符串 $s$ 。您需要回答以下问题中的 $q$ 个：

- 给定 $l$ 和 $r$ ( $l<r$ ), 求 $f(s_ls_{l + 1}\ldots s_r)$ 的值。

$^\dagger$ 字符串 $z$ 的子串是来自 $z$ 的连续字符段。例如，" $\mathtt{defor}$ "、" $\mathtt{code}$ "和" $\mathtt{o}$ "都是" $\mathtt{codeforces}$ "的子串，而" $\mathtt{codes}$ "和" $\mathtt{aaa}$ "不是。

$^\ddagger$ 回文字符串是指前后读法相同的字符串。例如，字符串" $\texttt{z}$ "、" $\texttt{aa}$ "和" $\texttt{tacocat}$ "是回文字符串，而" $\texttt{codeforces}$ "和" $\texttt{ab}$ "不是。

**输入**

每个测试包含多个测试用例。第一行包含一个整数 $t$ ( $1 \leq t \leq 2 \cdot 10^4$ ) - 测试用例的数量。测试用例说明如下。

每个测试用例的第一行包含两个整数 $n$ 和 $q$ （ $2 \le n \le 2 \cdot 10^5, 1 \le q \le 2 \cdot 10^5$ ），分别是字符串的大小和查询次数。

每个测试用例的第二行包含字符串 $s$ 。保证字符串 $s$ 只包含小写英文字符。

接下来的 $q$ 行分别包含两个整数 $l$ 和 $r$ （ $1 \le l <r \le n$ ）。( $1 \le l <r \le n$ ).

保证 $n$ 和 $q$ 的总和不超过 $2 \cdot 10^5$ 。

**输出**

对于每个查询，输出 $f(s_ls_{l + 1}\ldots s_r)$ 。

**注**

在第一个测试用例的第一个查询中，字符串为 $\mathtt{aaab}$ 。 $\mathtt{aaab}$ 、 $\mathtt{aab}$ 和 $\mathtt{ab}$ 都不是回文子串，它们的长度分别为 $4$ 、 $3$ 和 $2$ 。因此，这个字符串是 $2$ \-good, $3$ \-good 和 $4$ /good。因此是 $f(\mathtt{aaab}) = 2 + 3 + 4 = 9$ 。

在第一个测试用例的第二个查询中，字符串为 $\mathtt{aaa}$ 。没有非回文子串。因此是 $f(\mathtt{aaa}) = 0$ 。

在第二个测试用例的第一次查询中，字符串为 $\mathtt{abc}$ 。 $\mathtt{ab}$ 、 $\mathtt{bc}$ 和 $\mathtt{abc}$ 都是非回文子串，它们的长度分别为 $2$ 、 $2$ 和 $3$ 。因此，这个字符串是 $2$ \-good 和 $3$ \-good 。因此是 $f(\mathtt{abc}) = 2 + 3 = 5$ 。请注意，即使长度为 $2$ 的非 palindromic 子串有 $2$ 个，我们也只计算一次。

**think**

我们先跑一遍马拉车

然后得出每个位置包括 两个位置间隔的 最长回文长度
$$
\begin{align}
如果说 l,r\\
\exist i\in[l,r] \quad i-p[i]+1>l 那么就更新为 p[i]以上的都不行\\
同理\exist i\in[l,r] \quad i+p[i]-1<r 那么就更新为 p[i]以上的都不行\\
也就是对于区间维护 最小的 p[i]<i-l+1,也不一定哈 ,可以确定以这个位置的中心拓展是不行的\\
可以延伸到[p[i],min(i-l+1,r-i+1)]
\end{align}
$$




1 4

1 3

3 4

2 4

离线?

[1,3]

[1,4]

[2,4]

[3,4]