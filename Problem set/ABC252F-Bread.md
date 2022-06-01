[题目链接](https://atcoder.jp/contests/abc252/tasks/abc252_f)

题目大意：

给出一段长为$L$的面包，要求从中切分出长为$A_1,A_2,...,A_n$的$n$段，并使花费最少。

将一段长为$x$的面包切分为两段的花费总是$x$，无论切出来的两段长度各为多少。

题解：

首先考虑$\sum A_i = L$的情况，切分的逆过程相当于构造一棵哈夫曼树，每次选择最小的两块面包进行合并，一定可以使花费最少。

证明（即哈夫曼树最优性证明）：

考虑一棵二叉树，叶子节点的权值是面包长度，父节点权值是孩子节点权值的和，显然在求总花费过程中，高度越高的叶节点权值被计算次数越多，因此应该将权值最低的叶节点放在高度最高的地方（如果不是，那么我们总能通过将权值最低的节点与其他节点交换，来使答案更优）。

再考虑证：去掉最优树中最低的（即权值最小）、拥有相同父节点的树仍是最优树。

令$C'$为对叶节点为$A_1,A_2,...,A_n$的树的最小花费，设$A_1,A_2$为权值最小的叶节点，$C$为去掉这两个节点后的树的花费。

考虑哈夫曼树的构造过程，其花费是$C + A_1 + A_2$，由于$C'$是我们的最小花费，因此有$C' \leq C + A_1 + A_2$；类似的，再考虑根据构造过程将$A_1,A_2$去掉后的树的花费，有$C \leq C' - A_1 - A_2$，即$C' \geq C + A_1 + A_2$，因此，有$C' = C + A_1 + A_2$，故可知由哈夫曼树构造过程得出的花费一定是最小花费。

接下来需要考虑的问题是，当$L > \sum A_i$时，如何分割能使花费最少。

结论：直接将$L$截为长度为$\sum A_i$和$L - \sum A_i$的两段，花费一定最少。

证明：

设$L-\sum A_i$的这一段为$B$，我们这里比较一下不切分和将其切分为$B_1,B_2,...,B_m$的花费。

如果切分，我们尝试将这些线段插入原来的哈夫曼树，对于每个线段，由于$B_i$一定比整体的$B$要短，所以插入时，会比直接插入整体的$B$深度更深，在计算时会产生更大的贡献；此外，观察插入过程，我们发现原来的结点的高度也不会变低，比如说，假设原来有一个父节点$x$，其孩子为$x_1,x_2$，然后此时我们要将$B_i$替换$x_2$，然后将$x_2$放到上面一层去，此时，由于多了一个$B_i$，总高度会下降一层，因此$x_2$的高度没变，而$x和x_1$的孩子高度都降低了，产生的贡献就会更大，因此答案不会更优。

这个证明过程可能不太严谨，但是官解的证明我看不懂，也没有找到其他人有证明，暂时就这样证吧。

代码：

```cpp
#include<iostream>
#include<algorithm>
#include<set>
#include<cstring>
#include<vector>
#include<queue>
using namespace std;
#define ll long long
#define debug(x) cout << #x << " = " << x << endl;
#define IOS ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
#define pll pair<ll,ll>
#define ld long double
#define pb push_back
#define INF 0x3f3f3f3f3f3f3f3f
const ll N = 2e5 + 5;
const ll M = 100 + 5;
ll n,l;
ll a[N];
ll p[N];
int main()
{
	IOS;
	cin >> n >> l;
	ll sum = 0;
	for(ll i = 1; i <= n; i ++ )
	{
		cin >> a[i];
		sum += a[i];
	}
	priority_queue<ll,vector<ll>,greater<ll> >heap;
	for(ll i = 1; i <= n; i ++ )
	{
		heap.push(a[i]);
	}
	if(l > sum)heap.push(l - sum);
	ll ans = 0;
	while(heap.size() > 1)
	{
		ll t1 = heap.top();
		heap.pop();
		ll t2 = heap.top();
		heap.pop();
		ans += t1 + t2;
		heap.push(t1 + t2);
	}
	cout << ans << endl;
	
}
```