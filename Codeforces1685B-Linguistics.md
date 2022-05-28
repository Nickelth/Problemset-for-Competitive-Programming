[题目链接](https://codeforces.com/contest/1686/problem/D)

题目大意：

给出四种字符串的数量：$A,B,AB,BA$，数量分别为$a,b,c,d$,问能否用这些串拼出指定的一个字符串$s$.

题解：

记$cntX$是$s$中字符$X$出现的次数，首先检查字符串是否满足 $cntA = a + c + d$ 且 $cntB = b + c + d;$

注意到，我们将$AB$和$BA$用完后，剩下的部分直接填上$A$和$B$即可，而由于我们检查过了数量关系，所以如果$AB$和$BA$可以用完，那么剩下的部分一定可以用$A$和$B$构造出来；

考虑到，形如$AA$或$BB$的串无法用$AB$和$BA$覆盖，所以我们可以将$s$切分成若干“没有相邻相同字符的子串”，然后研究这些子串是否包含大于等于$c + d$个不相交的$AB$和$BA$.

我们可以将这些子串分为三类：

第一类，长度为奇数$2k + 1$，这种子串形如$ABABA$或$BABAB$，如果令$x$为填写该串所需$AB$的数量，$y$为所需$BA$的数量，那么一定有以下数量关系：

$x,y >= 0 且 x + y <= k;$

第二类，长度为偶数$2k$且形如$ABAB$，其满足以下数量关系：

$x = k 且 y = 0 或 x > 0 且 y > 0 且 x + y <= k - 1;$

第三类，长度为偶数$2k$且形如$BABA$，其满足以下数量关系：

$x = 0 且 y = k 或 x > 0 且 y > 0 且 x + y <= k - 1;$

我们有这样一个贪心的算法：

将所有第二类子串按长度排序，然后尽量用$AB$去填，直到$AB$用完或第二类子串用完；对第三类子串和$BA$也这样做；

最后，直接用$AB$和$BA$填剩下的串，如果能消耗掉$AB$和$BA$，那么有解，否则无解；

首先可以从直觉上看一下这个算法：如果我们用$BA$去拼第二类子串，显然每个子串会产生两个多余字符，为了使这样的多余字符最少，我们尽量让$BA$去拼长度较长的第二类子串；

接下来我们尝试严格证明这个算法，具体思路如下：

我们可以假设我们当前已经找到了一个构造方式，此时，我们的$(c,d)$由三部分组成：

一部分来自第一类子串，另外两部分来自第二类子串和第三类子串；

对于后两类，我们又可以细分，比如对于第二类子串，其中有仅使用$AB$构造的，即满足$(k,0)$，也有既使用$AB$又使用$BA$的或只使用$BA$，即满足$x + y <= k - 1$；第三类子串也可以这样细分；

现在，我们将仅使用$AB$的部分记为$V_1$，而不是仅使用$AB$的部分记为$V_2$，注意对于不是仅使用$AB$的部分，我们总会产生至少一个多余的$A$与一个多余的$B$；

注意到，如果我们有串$s_{1} \in V_{1},s_{2} \in V_{2},|s_{1}| > |s_{2}|$，那么我们总能改变构造方式使$s_{1} \in V_{2},s_{2} \in V_{1}$，也就是说，我们可以假设$V_1$中的串是第二类串中长度较小的部分，即按长度排序后的一个前缀；

假设存在一个第二类子串$t \in V_2$，且$V_1$中所有子串的所需的$AB$数量加上仅使用$AB$构造$t$所需的数量小于$c$，那么我们总能仅用$AB$构造$t$，假设原来我们花了$x$个$AB$和$y$个$BA$构造$t$，那么仅使用$AB$时，我们会得到$x + y + 1$个$AB$，对于损失的$y$个$BA$，注意到我们其他的串中多了$y + 1$个$AB$，那么只需要在这$y + 1$个$AB$中构造出这$y$个$BA$即可。这个结论的意义是，我们可以令尽量多的子串归于$V_1$，在有解的情况下，这样总能构造出一个合法的答案。

吐槽：$Codeforces$的官方题解将这个逻辑写成了一套极其复杂的形式，折磨了我一下午。

代码：
```
#include<bits/stdc++.h>
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
ll a,b,c,d;
ll n;
char s[N];
void show(vector<ll>&arr)
{
	cout << "listing : " << endl;
	for(auto i : arr)
	{
		cout << i << ' ' ;
	}
	cout << endl;
}
int check()
{
	ll cnta = 0,cntb = 0;
	for(ll i = 1; i <= n; i ++ )
	{
		if(s[i] == 'A')cnta ++ ;
		else cntb ++ ;
	}
	if(!(cnta == a + c + d && cntb == b + c + d))return 0;
	return 1;
}
int main()
{
	ll _;
	cin >> _;
	while(_ -- )
	{
		cin >> a >> b >> c >> d;
		cin >> (s + 1);
		n = strlen(s + 1);
		if(!check())
		{
			cout << "NO" << endl;
			continue;
		}
		vector<ll>a1,a2,a3;
		ll cur = -1,cnt = 0;
		for(ll i = 1; i <= n; i ++ )
		{
			cnt ++;
			if(cur == -1)cur = s[i] - 'A';
			if(i + 1 <= n && s[i] == s[i + 1])
			{
				if(cnt % 2 == 1)
				{
					a1.pb(cnt);
				}
				else
				{
					if(cur == 0)
					{
						a2.pb(cnt / 2);
					}
					else
					{
						a3.pb(cnt / 2);
					}
				}
				cnt = 0;
				cur = -1;
			}
		}
		if(cnt > 0)
		{
			if(cnt % 2 == 1)
			{
				a1.pb(cnt);
			}
			else
			{
				if(cur == 0)
				{
					a2.pb(cnt / 2);
				}
				else
				{
					a3.pb(cnt / 2);
				}
			}
			cnt = 0;
			cur = -1;
		}
		sort(a2.begin(),a2.end());
		sort(a3.begin(),a3.end());
		//show(a1),show(a2),show(a3);
		int f = 0;
		ll sum1 = 0;
		ll tot1 = accumulate(a2.begin(),a2.end(),0);
		ll tot2 = accumulate(a3.begin(),a3.end(),0);
		ll tot3 = accumulate(a1.begin(),a1.end(),0);
		ll amt1 = a2.size(),amt2 = a3.size();
		for(auto i : a2)
		{
			if(sum1 + i <= c)
			{
				sum1 += i;
				tot1 -= i;
				amt1 -- ;
			}
			else
			{
				tot1 -= c - sum1;
				sum1 = c;
				break;
			}
		} 
		ll sum2 = 0;
		for(auto i : a3)
		{
			if(sum2 + i <= d)
			{
				sum2 += i;
				tot2 -= i;
				amt2 -- ;
			}
			else
			{
				tot2 -= d - sum2;
				sum2 = d;
				break;
			}
		}
		if(sum1 == c && sum2 == d)
		{
			cout << "YES" << endl;
		}
		else if(sum1 == c && sum2 < d)
		{
			if(tot1 - amt1 + (tot3 - a1.size()) / 2 >= d - sum2)
			{
				cout << "YES" <<endl;
			}
			else
			{
				cout << "NO" << endl;
			}
		}
		else if(sum1 < c && sum2 == d)
		{
			if(tot2 - amt2 + (tot3 - a1.size()) / 2 >= c - sum1)
			{
				cout << "YES" << endl;
			}
			else
			{
				cout << "NO " << endl;
			}
		}
		else
		{
			if((tot3 - a1.size()) / 2 >= c - sum1 + d - sum2)
			{
				cout << "YES" << endl;
			}
			else
			{
				cout << "NO" << endl;
			}
		}

	}
}
```
 
 