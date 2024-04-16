[原题链接](https://codeforces.com/contest/1946/problem/C)

题目大意：

给出一棵树，切掉其中k条边，求所得最小子树规模的最大值

题解：

二分答案，然后检查对于当前答案mid，能否令树切得的每颗子树规模均大于等于mid且子树数量大于k。

检查的过程用贪心，从任意根开始dfs，每遇到一颗规模恰好大于mid的子树，就将其切掉，这个过程中递归地维护每个节点为根的连通量的规模：如果子树v从u切掉了，那么u的连通量不算入v的连通量，如果v不切掉，那么v的连通量规模（注意不是子树规模，因为v可能也切掉了一些子树）也计入u的连通量规模。

代码：

```c++
#include<bits/stdc++.h>
#define ll long long
#define ull unsigned long long
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 2e5 + 5;
const ll P = 131;
ll n, k;
vector<ll> g[N];
ll sz[N];
ll c[N];
ll cnt;
ll cal_sz(ll cur, ll fa)
{
    sz[cur] = 1;
    c[cur] = 1;
    for(auto i : g[cur])
    {
        if(i != fa)
        {
            sz[cur] += cal_sz(i, cur);
        }
    }
    c[cur] = sz[cur];
    return sz[cur];
}
int dfs(ll cur, ll fa, ll tot)
{
    if(c[cur] < tot)
    {
        return 0;
    }
    else if(c[cur] == tot)
    {
        return 1;
    }
    ll tmp = 1;
    for(auto i : g[cur])
    {
        if(i != fa && dfs(i, cur, tot))
        {
            //c[cur] -= sz[i];
            cnt ++ ;
            //cout << cur << " subtract " << i << endl;
        }
        else if(i != fa)
        {
            tmp += c[i];
        }
    }
    c[cur] = tmp;
    //debug(cur);
    //debug(c[cur]);
    if(c[cur] < tot)
    {
        return 0;
    }
    else{
        return 1;
    }

}
int check(ll cur)
{
    cnt = 0;
    for(ll i = 1; i <= n; i ++ ) c[i] = sz[i];
    dfs(1, -1, cur);

    //debug(cnt);
    if(c[1] >= cur) cnt ++ ;
    if(cnt >= k + 1)
    {
        return 1;
    }
    else{
        return 0;
    }
}
void solve()
{
    cin >> n >> k;
    for(ll i = 1; i <= n; i ++ )
    {
        g[i].clear();
    }
    for(ll i = 0; i < n - 1; i ++ )
    {
        ll u, v;
        cin >> u >> v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    cal_sz(1, -1);
    ll l = 1, r = n;
    while(l < r)
    {
        ll mid = (l + r + 1) / 2;
        //debug(mid);
        if(check(mid))
        {
            l = mid;
        }
        else{
            r = mid - 1;
        }
    }
    cout << l << endl;

}
int main()
{
    ll _;
    cin >> _;
    while(_ -- )
    {
        solve();
    }
}


```

