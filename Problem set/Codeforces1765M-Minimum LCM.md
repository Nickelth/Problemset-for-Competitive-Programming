[题目链接](https://codeforces.com/contest/1765/problem/M)

题目大意：

给出一个整数 $n$ ，求两个正整数 $a$ 、 $b$ 满足 $a + b = n$ 且 $LCM(A,B)$ 最小.

题解：

假设 $a > b$ ， $LCM(a,b)$ 最小的 $a$ 和 $b$ 必然满足 $a\ MOD\ b = 0$ .

证明：

若 $a\ MOD\ b = 0$ ，则 $LCM(a,b) < n$ ;

若 $a\ MOD\ b \neq 0$ ，则 $LCM(a,b)$ 至少为 $2a$ ，而 $a > \frac{n}{2}$ ，所以 $LCM(a,b) > n$ .

由于 $a\ MOD\ b = 0$ ，即 $a = kb$ ，与 $a + b = n$ 联立可知 $b$ 为 $n$ 的因数，枚举即可.

代码：

略.