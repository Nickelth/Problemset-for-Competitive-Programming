[题目链接](https://codeforces.com/contest/1948/problem/D)

题目描述：

给出一个字符串，包含小写英文字母和问号，求将问号换成任意字母后，形如一个串连续重复两遍组成的串的最长子串。

题解：

枚举子串长度d，然后枚举左边界l，对每个d，构造一个数列a，如果s[l]与s[l + d]匹配则a[l] = 1, 否则为0。如果有连续d个1，则说明有长度为d的符合要求的子串。

代码：

```cpp
#include<bits/stdc++.h>
#define ll long long
#define ull unsigned long long
#define debug(x) cout << #x << " = " << (x) << endl
using namespace std;
const ll N = 2e5 + 5;
const ll P = 131;
ll a[N];
string s;
ll get_tail(ll l, ll r)
{
    return r + r - l - 1;
}
int check_char(char c1, char c2)
{
    if(c1 == '?' || c2 == '?' || c1 == c2) return 1;
    else return 0;
}
void solve()
{
    cin >> s;
    ll ans = 0;
    for(ll d = 1; 2 * d <= s.length(); d ++ )
    {
        ll cnt = 0;
        for(ll l = 0; l < s.length() - d; l ++ )
        {
            if(check_char(s[l], s[l + d]))
            {
                cnt ++ ;
                if(cnt == d)
                {
                    ans = max(ans, d * 2);
                    break;
                }
            }
            else
            {
                cnt = 0;
            }
        }
    }
    std::cout << ans << endl;
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

