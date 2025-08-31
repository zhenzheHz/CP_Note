# 第七章、前綴和與差分
---

## 前言
前綴和與差分是競賽中常用的技巧

- **前綴和**(Prefix Sum)可以快速求區間和

- **差分**(Difference)可以快速進行區間修改

- 為了方便討論，先定義序列 $<a_i>$ 是從 $i=1$ ~ $n$ 的，並且 $a_0 = 0$

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
