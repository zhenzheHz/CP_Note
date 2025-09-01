# 第七章、前綴和與差分
---

## 前言
前綴和與差分是競賽中常用的技巧

- **前綴和**(Prefix Sum)可以快速求區間和

- **差分**(Difference)可以快速進行區間修改

- 為了方便討論，先定義序列 $a_n$ 是從 $i=1$ ~ $n$ 的，並且 $a_0 = 0$

> ### 前置思考
>
> 給定序列 $X_n$，並且有 $Q$ 筆詢問，每次詢問要求回答在 $X$ 中 $[L,R]$ 的和
>
> 限制： $n \leq 10^5,Q\leq 10^5$
>
> 若暴力求解，顯然複雜度為 $O(n^2)$，肯定 `TLE`
> ```cpp
> for(int i=0;i<Q;i++) {
>     int sum = 0;
>     for(int i=l;i<=r;i++) {
>        sum += x[i];
>     }
>     cout << sum << '\n';
> }
> ```

---

## 前綴和
簡單來說就是 **數列的前 $n$ 項和**，也就是級數

$$S_i = \sum _{k=1}^i a_k$$

並且可以很直觀的發現

$$S_0 = 0, S_i = S_{i-1} + a_i$$

計算了前綴和，當我們想要知道一個區間的元素和，就能從 $O(n)$ 降到 $O(1)$

> 前綴和預處理 $O(n)$，查詢 $O(1)$

$$S[L,R] = S_R - S_{L-1}$$

```cpp
int S[N],X[N];
S[0] = X[0];
for(int i=1;i<N;i++) {
    S[i] = S[i-1] + X[i];
}
```

---

## 差分

了解了前綴和的概念之後，差分就相當簡單了

差分的運算就是前綴和的 **反運算**

差分序列 $D_n$ 指的是

$$D_i = a_i - a_{i-1},(a_0 = 0)$$

這個差分序列可以幹嘛呢，有點像分項對消的概念

差分序列前 $i$ 項的前綴和恰好是 $a_i$

$$\sum_{k=1}^{i} D_k = a_i$$

### 應用
對於區間修改操作十分的便利

常用於區間修改的 `Fenwick Tree / BIT`

或者維護樹上資訊的 `樹上差分`

---

## 練習

> ### [Zerojudge E339 - 前綴和練習](https://zerojudge.tw/ShowProblem?problemid=e339)
>
> 難度：*Easy* $(1/10)$

> ### [Zerojudge E340 - 差分練習](https://zerojudge.tw/ShowProblem?problemid=e340)
>
> 難度：*Easy* $(1/10)$


> ### [Luogu P1387 - 最大正方形](https://www.luogu.com.cn/problem/P1387)
>
> 難度：*Easy* $(2.5/10)$
>
> <details>
>     <summary> 參考解法 </summary>
> 
> 把前綴和的概念轉成二維的就好（左上到右下的所有元素總和）
> 
> 以下是 AC Code from OI Wiki
> 
> ```cpp
> #include <algorithm>
> #include <iostream>
> #include <vector>
> 
> int n, m;
> std::vector<std::vector<int>> a, ps;  // (n + 1) x (m + 1).
> 
> // Calculate the prefix sum of 2-d array.
> void prefix_sum() {
>   ps = a;
>   for (int i = 1; i <= n; ++i)
>     for (int j = 1; j <= m; ++j)
>       ps[i][j] += ps[i - 1][j] + ps[i][j - 1] - ps[i - 1][j - 1];
> }
> 
> // Find the sum of elements in submatrix [x1, y1] to [x2, y2].
> int query(int x1, int y1, int x2, int y2) {
>   return ps[x2][y2] - ps[x1 - 1][y2] - ps[x2][y1 - 1] + ps[x1 - 1][y1 - 1];
> }
> 
> int main() {
>   std::cin >> n >> m;
>   a.assign(n + 1, std::vector<int>(m + 1));
> 
>   for (int i = 1; i <= n; i++)
>     for (int j = 1; j <= m; j++) std::cin >> a[i][j];
> 
>   prefix_sum();
> 
>   int ans = 0;
>   for (int l = 1; l <= std::min(n, m); ++l)
>     for (int i = l; i <= n; i++)
>       for (int j = l; j <= m; j++)
>         if (query(i - l + 1, j - l + 1, i, j) == l * l) ans = std::max(ans, l);
> 
>   std::cout << ans << std::endl;
>   return 0;
> }
> ```
> </details>

> ### [CSES 1138 - Path Queries](https://cses.fi/problemset/task/1138)
> 
> 難度：*Medium* $(5.5/10)$
>
> 先備知識：`BIT / 線段樹`, `dfs`, `ETT`
> 
> <details>
>     <summary> 參考解法 </summary>
> 
> 解法一：前面提到的 **樹上差分**
> 
> 解法二：樹鏈剖分
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> static constexpr int N = 2e5+5;
> vector<vector<int>> g(N);
> vector<int> val(N),in(N),out(N),bit(N);
> int n,q;
> inline void update(int p,int v){
>     while(p<=n){
>         bit[p] += v;
>         p += (p&-p);
>     }
> }
> inline int get(int p){
>     int ans = 0;
>     while(p>0){
>         ans += bit[p];
>         p -= (p&-p);
>     }
>     return ans;
> }
> inline void dfs(int cur,int from){
>     static int timer = 0;
>     in[cur] = ++timer;
>     for(int &nxt : g[cur]){
>         if(nxt != from)
>             dfs(nxt,cur);
>     }
>     out[cur] = timer;
> }
>  
> int32_t main(){
>     ios_base::sync_with_stdio(0);
>     cin.tie(0), cout.tie(0);
>  
>     cin >> n >> q;
>     for(int i=1;i<=n;i++){
>         cin >> val[i];
>     }
>     for(int i=1;i<n;i++){
>         int a,b;
>         cin >> a >> b;
>         g[a].push_back(b);
>         g[b].push_back(a);
>     }
>     dfs(1,1);
>     for(int i=1;i<=n;i++){
>         update(in[i],val[i]);
>         update(out[i]+1,-val[i]);
>     }
>     while(q--){
>         int op,a,b;
>         cin >> op;
>         if(op == 1){
>             cin >> a >> b;
>             update(in[a],b - val[a]);
>             update(out[a]+1,val[a] - b);
>             val[a] = b;
>         }else{
>             cin >> a;
>             cout << get(in[a]) << '\n';
>         }
>     }
> }
> ```
> </details>

> ### [CSES 1650 - Range Xor Queries](https://cses.fi/problemset/task/1650)
>
> 難度：*Easy* $(3/10)$
>
> <details>
>     <summary> 參考解法 </summary>
> 
> 前綴和不要拘束於 "和"
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> static constexpr int MAXN = 2e5+5;
> int arr[MAXN], prefix[MAXN];
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int n,q;
>     cin >> n >> q;
>     for(int i=1;i<=n;i++) cin >> arr[i];
>     prefix[1] = arr[1];
>     for(int i=2;i<=n;i++) prefix[i] = arr[i] ^ prefix[i-1];
>     while(q--) {
>         int l,r;
>         cin >> l >> r;
>         cout << (prefix[r] ^ prefix[l-1]) << '\n';
>     }
> }
> ```
> </details>


