# 彰雲嘉區賽攻略

---

> by. zhenzhe 2025.10.28

這篇主要分享我參加了兩年由彰師大負責出題的區賽的經驗和準備方式

彰雲嘉區賽的題目長得跟其他區的實在是滿不一樣的，品質也很不穩定

有時候會出現題序不清、不給測資範圍、沒有specail judge的情形

不過就只能努力克服

不過有趣的是他每一題都是 $5$ 筆測資，每筆 $2$ 分

---

## 題目類型

### 實作題型

這種題目類型其實就是在 [基礎演算法 - 模擬](https://zhenzhehz.github.io/CP_Note/#Basic/Implement.md) 中提過的那種

這種題目通常就是題目簡單明瞭，但感覺就很難寫或者很容易寫爛

不過區賽的話如果錯了可以檢查看看邊界測資或極端案例

或是直接猜答案

像是 2024 年的那場，有一題算出來的答案是 $0.00$ ~ $1.00$ 的其中一個數字

聽說輸出 $1.00$ 可以得到 $2$ 分

這裡提供一個練習題，還滿不錯的，基本上完美詮釋了何謂實作題

> ### [YTP 2025 高中組全國決賽第9題 - 農田劃分](https://oj.ntucpc.org/problems/959)
>
> 解法說明：[高中組 決賽 題序9 農田分割 Farmland Partition](https://www.youtube.com/watch?v=VMnR6JhXkQg)
>
> 難度： $4/10$
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Problem : https://oj.ntucpc.org/problems/959
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> int H,W,arrange[15][15];
> struct Special {int x,y,area;};
> vector<Special> place;
> bool valid(int x, int y, int h, int w) {
>     if(x + h - 1 > H || y + w - 1 > W) return false;
>     for(int i = 0; i < h; i++) {
>         for(int j = 0; j < w; j++) {
>             if(arrange[x+i][y+j] > 0) return false; 
>         }
>     } 
>     return true;
> }
> void apply(int x, int y, int h, int w, int id) {
>     for(int i = 0; i < h; i++) {
>         for(int j = 0; j < w; j++) {
>             arrange[x+i][y+j] = id;
>         }
>     }
> }
> bool dfs(int idx) {
>     if(idx == (int)place.size()) return true;
>     auto &[x,y,area] = place[idx];
>     for(int h=1; h<=area; h++) {
>         if(area % h != 0) continue;
>         int w = area / h;
>         for(int sx = max((int) 1,x-h+1); sx <= x; sx++) {
>             for(int sy = max((int) 1,y-w+1); sy <= y; sy++) {
>                 if(!valid(sx, sy, h, w)) continue;
>                 apply(sx, sy, h, w, idx+1);
>                 if(dfs(idx+1)) return true;
>                 apply(sx, sy, h, w, 0);
>             }
>         }
>     }
>     return false;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     cin >> H >> W;
>     int x;
>     for(int i = 1; i <= H; i++) {
>         for(int j = 1; j <= W; j++) {
>             cin >> x;
>             if(x > 0) place.push_back({i,j,x});
>         }
>     }
>     dfs(0);
>     for(int i = 1; i <= H; i++) {
>         for(int j = 1; j <= W; j++) {
>             cout << arrange[i][j] << " \n"[j==W];
>         }
>     }
>     return 0;
> }
> ```
> </details>

