# 第十章、構造

---

## 簡介
**構造題** 是比賽中常見的一種類型。

從形式上來看，這類題目的答案往往具有某種規律性，因此即使問題的規模快速增大，也仍然有機會透過觀察規律比較容易地得到答案。

解構造題時，關鍵在於思考：當問題規模不斷增長時，答案會受到什麼影響，這種影響是否能被推廣或延伸？
舉例來說，在設計動態規劃演算法時，我們需要考慮從某個狀態轉移到後繼狀態會造成什麼影響；同樣地，在構造題中，我們也要從小規模案例中觀察規律，並設計一個能推廣到大規模情況的構造方式。

簡而言之，構造題要求我們不只是找到答案，而是要設計出一個能生成答案的方法，並保證這個方法在更大規模的情況下依然成立。

構造題的一個顯著特點是 高度自由度。也就是說，一道構造題可能存在多種不同的構造方法，但通常會有一種較為簡單的方法可以滿足題目的要求。表面上看，這似乎放寬了限制，使題目變得簡單，但實際上，正是這種高自由度往往會讓選手沒有明確思路而無從下手。

構造題的另一個特點是 形式靈活、變化多樣。這類題目並不存在一個通用的解法或固定套路可以解決所有情況，有時甚至很難從多題中找到解題思路的共性。每一道構造題通常都需要針對題目特性，仔細分析、觀察規律，才能設計出可行的構造方法。

> 如果你有刷 **CSES** 的話相信你再熟悉不過了

---

## 練習

> ### [Codeforces 743C(Div.2) - Vladik and fractions](https://codeforces.com/problemset/problem/743/C)
>
> 難度：*Easy* $(3/10)$
> <details>
>  <summary> 參考解法 </summary>
>
> 注意到 $n,(n+1),n(n+1)$ 為一種解，但是 $n=1$ 時無解
> </details>

> ### [Codeforces 1930B - Permutation Printing](https://codeforces.com/contest/1930/problem/B)
>
> 難度：*Easy* $(2/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> 注意到 $1,n,2,n-1,...$ 為一組顯然解
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int t;
>     cin >> t;
>     while(t--) {
>         int n;
>         cin >> n;
>         int left = 1, right = n;
>         for(int i=1;i<=n;i++) {
>             cout << (i&1? left++ : right--) << " \n"[i==n];
>         }
>     }
> }
> ```
> </details>
