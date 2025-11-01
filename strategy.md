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

### 一、實作題型

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

---

### 二、字串處理題型

這類的題目或多或少也算是實作題的一種，不過要注意的事情就比較多了，有時候（通常）還需要搭配一些資料結構來處理

所以務必熟悉 [STL](https://zhenzhehz.github.io/CP_Note/#STL/map.md) （尤其是 `map`）以及 [string](https://zhenzhehz.github.io/CP_Note/#STL/string.md) 的基本語法

除此之外還需要知道 `getline()` 以及 [stringstream](https://zhenzhehz.github.io/CP_Note/#Syntax/optimize.md) 的用法

也需要知道 `getline()` 和 `cin` 最好不要同時使用

也要避免使用 `cin.ignore()`

若 `getline()` 和 `cin` 同時使用，會造成 `getline` 會先讀到 `cin` 那行的 `\n`

比如說輸入長這樣

```
2
hello, world.
this is line 2.
```

你這樣輸入

```cpp
cin >> n;
getline(cin,line1); // 會抓到第一行2後面的 '\n'
getline(cin,line2); // hello, world
```

以下是正確範例，使用 `stoi()` 可以將 `string` 轉成 `int`

```cpp
string N,line1,line2;
getline(cin,N);
int n = stoi(N);
getline(cin,line1);
getline(cin,line2);
```

另一點需要注意的就是行末可能會有 `'\r'` 需要注意，避免被作業系統搞到

解決辦法如下：
```cpp
void inspect(string &str) {
    while(str.back() == '\r' || str.back() == '\n') {
        str.pop_back();
    }
}
```

詳細可以參考：[淺談 CRLF 對程式競賽的影響](https://hackmd.io/@MelonHiker/H1CVcl5Dgl)

寫這種題目的小技巧就是善用 `isalpha()`、`isdigit()`、`islower()`、`isupper()`

他們都會回傳 `true` 或 `false`，參數則是一個 `char`，簡單來說就是可以判斷一個 `char` 是字母、數字、大小寫

```cpp
bool upupercase = isupper('A'); // 判斷大寫，true
bool lowercase = islower('A'); // 判斷小寫，false
bool alphabet = isalpha('A'); // 判斷是否為字母（大小寫皆可），true
bool digit = isdigit('0'); // 判斷是否為數字（char 的 0~9），true
```

以下是一題練習題可以練習上述函式的用法

> ### [UVA 00245 - Uncompress](https://zerojudge.tw/ShowProblem?problemid=e569)
>
> <details>
>   <summary> 參考解法 </summary>
>
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=e569
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> void inspect(string &str) {
>     while(str.back() == '\r' || str.back() == '\n') {
>         str.pop_back();
>     }
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     string line, word;
>     int index;
>     vector<string> lst;
>     while(getline(cin, line)) {
>         // init
>         inspect(line);
>         if(line == "0") break;
>         word.clear();
>         index = 0;
>         line.push_back('\n');
>         // handle
>         for(auto &ch : line) {
>             if(isalpha(ch)) word.push_back(ch);
>             else if(isdigit(ch)) index *= 10, index += ch - '0';
>             else {
>                 // check word
>                 if(!word.empty()) {
>                     cout << word;
>                     lst.push_back(word);
>                     word.clear();
>                 }
>                 // check number to substitute
>                 if(index != 0) {
>                     int sz = lst.size();
>                     cout << lst[sz - index];
>                     string tmp = lst[sz-index];
>                     lst.erase(lst.end() - index);
>                     lst.push_back(tmp);
>                     index = 0;
>                 }
>                 cout << ch;
>             }
>         }
>     }
>     return 0;
> }
> ```
> </details>

---

### 三、大數處理

就是經典的大數題目，因為 `long long` 的最大值只到 $2^{63}-1$，所以當題目的值可以到 $10^{10^6}$ 這種等級就要用大數

$P.S.$ 如果題目根本沒說可能會多大，就最好用一下比較保險，畢竟除了除法之外都算好寫

實作上的話我個人喜歡用 `vector<int16_t>` 來寫，`index = 0` 是個位數（反過來存）

```cpp
using Integar = vector<int16_t>;
Integar operator+(const Integar &A, const Integar &B) {
    Integar C;
    int16_t carry = 0;
    for(size_t i = 0; i < max(A.size(), B.size()); i++) {
        int16_t a = (i < A.size()? A[i] : 0);
        int16_t b = (i < B.size()? B[i] : 0);
        int sum = a + b + carry;
        C.push_back(sum % 10);
        carry = sum / 10;
    }
    if(carry) C.push_back(carry);
    return C;
}
```

```cpp
Integar operator*(const Integar &A, const Integar &B) {
    Integar C(A.size() + B.size(), 0);
    for(size_t i = 0; i < A.size(); i++) {
        int16_t carry = 0;
        for(size_t j = 0; j < B.size() || carry; j++) {
            int a = A[i], b = (j < B.size()? B[j] : 0);
            int product = C[i+j] + a * b + carry;
            C[i+j] = product % 10;
            carry = product / 10;
        }
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```


