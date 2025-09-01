# 第三章、模擬

---

## 簡介
**模擬**（Simulation），就是利用電腦來模擬題目中所要求的一連串操作。

模擬類的題目通常具有程式碼量大、操作步驟多、思路較繁瑣的特點。

由於程式碼龐雜，常常會出現難以發現錯誤的情況。如果在考試中因為小錯導致程式無法通過，往往會浪費相當多的時間。

因此，面對模擬題時，良好的程式規劃、清晰的思路，以及謹慎的實作就特別重要。

> 模擬題，俗稱實作題，就是看起來很簡單但做起來就很麻煩的題目
>
> 因為區賽中經常出現，也被稱為 ~~彰師大爛題~~
>
> APCS第二題基本上都是模擬題

---

## 技巧
在撰寫模擬題時，可以遵循以下建議來提升解題效率：

1. 動手寫程式前，先規劃流程
在草稿紙上盡可能完整地寫出要實現的步驟，避免邊想邊寫導致思路混亂。

2. 程式碼模組化
將每個部分盡量寫成獨立的函式、結構體或類別，讓程式結構更清晰，調整也更方便。

3. 統一處理重複概念
對於可能多次出現的概念，最好先轉換為統一格式，方便後續處理。
例如：若題目輸入為 "YY-MM-DD 時:分"，可以寫一個函式將其轉換成秒數，減少不同單位之間的混淆。

4. 分塊調試
模組化的好處之一就是可以單獨測試某個部分，方便快速定位錯誤。

5. 保持思路清晰
寫程式時要依照事先規劃好的步驟進行，而不是想到什麼就寫什麼，否則容易導致結構混亂、錯誤增加。

事實上，以上這些習慣不僅適用於模擬題，在解決其他類型的題目時同樣非常有幫助。

---

## 練習

> ### [UVA 255 - Correct Move](https://zerojudge.tw/ShowProblem?problemid=e601)
>
> 難度：*Easy* $(3/10)$
> 
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Time : 2024 / 10 / 15
> // Problem : https://zerojudge.tw/ShowProblem?problemid=e601
> #include <bits/stdc++.h>
> #define row first
> #define column second
> using namespace std;
> pair<int,int> crdn(int x) {
>     // get coordinate
>     return make_pair((int)x/8,x%8);
> }
> bool is_near(int a,int b) {
>     pair<int,int> A = crdn(a), B = crdn(b);
>     return abs(A.row-B.row) + abs(A.column-B.column) == 1;
> }
> int main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int King, queen, go;
>     // string ans = "";
>     while(cin >> King >> queen >> go) {
>         pair<int,int> now = crdn(queen), after = crdn(go), king = crdn(King);
>         if(king.row == now.row && king.column == now.column) {
>             cout << "Illegal state\n";
>             continue;
>         }
>         if (now.row != after.row && now.column != after.column) {
>             cout << "Illegal move\n";
>             continue;
>         }
>         if(queen == go || King == go) {
>             cout << "Illegal move\n";
>             continue;
>         }
>         if(now.row == after.row && now.row == king.row) {
>             // q1-k-q2 or q2-k-q1
>             if(now.column < king.column && king.column < after.column) {
>                 cout << "Illegal move\n";
>                 continue;
>             }
>             if(now.column > king.column && king.column > after.column) {
>                 cout << "Illegal move\n";
>                 continue;
>             }
>         }
>         if(now.column == after.column && now.column == king.column) {
>             // q1-k-q2 or q2-k-q1
>             if(now.row < king.row && king.row < after.row) {
>                 cout << "Illegal move\n";
>                 continue;
>             }
>             if(now.row > king.row && king.row > after.row) {
>                 cout << "Illegal move\n";
>                 continue;
>             }
>         }
>         if(is_near(go,King)) 
>             cout << "Move not allowed\n";
>         else {
>             bool check = false;
>             if(King >= 8 && !is_near(King-8,go)) check = true;
>             if(King <= 55 && !is_near(King+8,go)) check = true;
>             if(king.column != 0 && !is_near(King-1,go)) check = true;
>             if(king.column != 7 && !is_near(King+1,go)) check = true;
>             cout << (check? "Continue\n" : "Stop\n");
>         }
>     }
> }
> ```
> </details>

> ### [2019北市賽 B - 地圖編修(Map)](https://tioj.ck.tp.edu.tw/problems/2170)
>
> 難度： *Medium* $(5/10)$
> 
> 先備知識： `模逆元`, `快速冪`
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Time : 2024 / 11 / 19
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> static constexpr int MAXN = 1005;
> int n,mod,mat[MAXN][MAXN];
> int fp(int a,int b) {
>     if(b == 0) return 1;
>     if(b & 1) return a*fp(a,b-1)%mod;
>     int h = fp(a,b>>1)%mod;
>     return h*h%mod;
> }
> #define inv(x) (fp(x,mod-2)%mod)
> void row_operation() {
>     int mainly = 0;
>     for(int col = 0;col<n;col++) {
>         /*
>             1. 選擇主元(pivot)，為值最大者
>             2. 主元歸一化(把主元變成 1)
>             3. 消去其他列的那一行
>         */
>         // step 1 -> select pivot
>         int pivot = mainly;
>         for(int row = mainly+1; row<n;row++) {
>             if(abs(mat[row][col]) > abs(mat[pivot][col]))
>                 pivot = row;
>         }
>         if(mat[pivot][col] == 0) continue;
>         swap(mat[mainly], mat[pivot]);
>         // step 2 -> allow mainly element to 1
>         int iv = inv(mat[mainly][col]);
>         for(int j=col;j<=n;j++) {
>             mat[mainly][j] = mat[mainly][j] * iv % mod;
>         }
>         // step 3 -> do the row operation
>         for(int i=0;i<n;i++) {
>             int factor = mat[i][col];
>             for(int j=col;j<=n;j++) {
>                 if(i == mainly) continue;
>                 mat[i][j] = mat[i][j] - mat[mainly][j] * factor % mod + mod;
>                 mat[i][j] %= mod; 
>             }
>         }
>         mainly += 1;
>     }
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     cin >> n >> mod;
>     for(int i=0;i<n;i++) {
>         cin >> mat[i][n];
>     }
>     for(int i=0;i<n;i++) {
>         for(int j=0;j<n;j++) {
>             cin >> mat[j][i];
>         }
>     }
>     row_operation();
>     for(int i=0;i<n;i++) {
>         cout << mat[i][n] << " \n"[i==n-1];
>     }
> }
> ```
> </details>

