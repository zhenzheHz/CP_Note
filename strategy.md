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


而下面這種就是宜看就不想寫的題目

> ### [UVA 392 - Polynomial Showdown](https://zerojudge.tw/ShowProblem?problemid=c060)
>
> <details>
>     <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=c060
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     array<int,9> c;
>     int sign = 0;
>     while(cin >> c[8]) {
>         sign = 0;
>         for(int i = 7; i >= 0; i--) cin >> c[i];
>         for(int i = 0; i < 9; i++) {
>             if(c[i] < 0) {
>                 sign |= (1 << i);
>                 c[i] = abs(c[i]);
>             }
>         }
>         bool leader = false;
>         for(int i = 8; i >= 0; i--) {
>             if(c[i] == 0) continue;
>             if(leader) {
>                 cout << ' ';
>                 cout << ((sign & (1<<i))? '-' : '+');
>                 cout << ' ';
>             }
>             else {
>                 leader = true;
>                 if(sign & (1<<i)) cout << '-';
>             }
>             if(i == 0) cout << c[i];
>             else {
>                 if(c[i] != 1) cout << c[i];
>                 cout << 'x';
>                 if(i > 1) cout << '^' << i;
>             }
>         }
>         if(!leader) cout << 0;
>         cout << '\n';
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

> ### [UVA 245 - Uncompress](https://zerojudge.tw/ShowProblem?problemid=e569)
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
```

```cpp
ostream& operator<<(ostream &o, integar &x) {
    for(int i = 0; i < (int) x.size(); i++) {
        o << x[i];
    }
    return o;
}
```
```cpp
bool geq(const integar &A, const integar &B) {
    if(A.size() != B.size()) return A.size() > B.size();
    for(int i = 0; i < (int) A.size(); i++) {
        if(A[i] == B[i]) continue;
        return A[i] > B[i];
    }
    return 1;
}
```

加減乘法中 $A, B$ 的 `index` 皆為低位為 $0$

```cpp
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
Integar operator-(const Integar &A, const Integar &B) {
    Integar C;
    int16_t borrow = 0;
    for(int i = 0; i < (int)A.size(); i++) {
        int16_t a = A[i] - borrow;
        int16_t b = (i < (int)B.size()? B[i] : 0);
        if(a < b) a += 10, borrow = 1;
        else borrow = 0;
        C.push_back(a-b);
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back();
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

除法比較不一樣（$A, B$ 的 `index` 皆為高位為 $0$）

```cpp
integar operator/(integar A,integar B) {
    integar quotient;
    if(!geq(A,B)) return {0};
    int k = A.size() - B.size();
    while(B.size() < A.size()) B.push_back(0);
    for(int i = 0; i <= k; i++) {
        int16_t l = -1, r = 10;
        while(l+1!=r) {
            integar m = {(l+r)/2};
            if(geq(A, B * m)) l = (l+r)/2;
            else r = (l+r)/2;
        }
        integar tmp = {l};
        integar C = B * tmp;
        A = A - C;
        B.pop_back();
        if(quotient.size() || l != 0)quotient.push_back(l);
    }
    // integar remainer = A;
    return quotient;
}
```

> ### [2016 高雄市資訊學科能力競賽 第一題 - 畢氏定理](https://zerojudge.tw/ShowProblem?problemid=b898)
>
> 這題只有用大數比較大小
>
> <details>
>     <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=b898
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> bool cmp(const string &A, const string &B) {
>     return (A.size() == B.size()? A < B : A.size() < B.size());
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int x;
>     cin >> x;
>     string a,b,c;
>     for(int i = 0; i < 5; i++) {
>         cin >> a >> b >> c;
>         vector<string> t = {a,b,c};
>         stable_sort(t.begin(), t.end(), cmp);
>         cout << *t.rbegin() << '\n';
>     }
>     return 0;
> }
> ```
> </details>

> ### [UVA 495 - Fibonacci Freeze](https://zerojudge.tw/ShowProblem?problemid=c121)
>
> 這題只有大數加法
>
> <details>
>     <summary> 參考解法 </summary>
>
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=c121
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> using Integar = vector<int16_t>;
> Integar operator+(const Integar &A, const Integar &B) {
>     Integar C;
>     int16_t carry = 0;
>     for(size_t i = 0; i < max(A.size(), B.size()); i++) {
>         int16_t a = (i < A.size()? A[i] : 0);
>         int16_t b = (i < B.size()? B[i] : 0);
>         int sum = a + b + carry;
>         C.push_back(sum % 10);
>         carry = sum / 10;
>     }
>     if(carry) C.push_back(carry);
>     return C;
> }
> void operator<<(ostream &o, const Integar &P) {
>     for(int i = P.size()-1; i >= 0; i--) {
>         o << P[i];
>     }
>     o << '\n';
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int n;
>     Integar F[5005];
>     F[0] = {0}, F[1] = {1};
>     for(int i = 2; i <= 5000; i++) {
>         F[i] = F[i-1] + F[i-2];
>     }
>     while(cin >> n) {
>         cout << "The Fibonacci number for " << n << " is ";
>         cout << F[n];
>     }
>     return 0;
> }
> ```
> </details>

> ### [UVA 748 - Exponentiation](https://zerojudge.tw/ShowProblem?problemid=d394)
>
> 大數乘法 + 一些字串處理
>
> <details> 
>     <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=d394
> #include <bits/stdc++.h>
> #define int int64_t
> #define sz(x) ((int)(x.size()))
> using namespace std;
> using integar = vector<int16_t>;
> void trim(integar &x) {
>     while(x.size() > 1 && x.back() == 1) 
>         x.pop_back();
> }
> integar operator*(const integar &A, const integar &B) {
>     integar C(sz(A) + sz(B), 0);
>     for(int i = 0; i < sz(A); i++) {
>         int carry = 0;
>         for(int j = 0; j < sz(B) || carry; j++) {
>             int b = (j < sz(B)? B[j] : 0);
>             int sum = C[i+j] + A[i] * b + carry;
>             C[i+j] = sum % 10;
>             carry = sum / 10;
>         }
>     }
>     trim(C);
>     return C;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     string num;
>     int power;
>     while(cin >> num >> power) {
>         integar base;
>         for(int i = sz(num) - 1; i >= 0; i--) {
>             if(num[i] != '.') base.push_back(num[i] - '0');
>         }
>         auto f = num.find('.');
>         int dot = (f == string::npos? 0 : num.size() - f - 1);
>         integar result = {1};
>         for(int i = 0; i < power; i++) {
>             result = result * base;
>         }
>         while(sz(result) < dot * power) result.push_back(0);
>         string ans = "";
>         for(int i = 0; i < sz(result); i++) {
>             if(i == dot * power) ans.push_back('.');
>             ans.push_back('0' + result[i]);
>         }
>         if(sz(result) == dot * power) ans.push_back('.');
>         while(ans.back() == '0') ans.pop_back();
>         reverse(ans.begin(),ans.end());
>         while(ans.back() == '0') ans.pop_back();
>         if(ans.back() == '.') ans.pop_back();
>         cout << ans << '\n';
>     }
>     return 0;
> }
> ```
> </details>


> ### [Zerojudge - a021 大數運算](https://zerojudge.tw/ShowProblem?problemid=a021)
> 簡單來說就是會用到四種內容
>
> <details>
>     <summary> 參考解法 </summary>
>
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=a021
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> using integar = vector<int>;
> void trim(integar &x) {
>     while(x.size() > 1 && x.back() == 0) x.pop_back();
> }
> ostream& operator<<(ostream &o, integar &x) {
>     for(int i = 0; i < (int) x.size(); i++) {
>         o << x[i];
>     }
>     return o;
> }
> bool geq(const integar &A, const integar &B) {
>     if(A.size() != B.size()) return A.size() > B.size();
>     for(int i = 0; i < (int) A.size(); i++) {
>         if(A[i] == B[i]) continue;
>         return A[i] > B[i];
>     }
>     return 1;
> }
> integar operator+(integar &A,integar &B) {
>     reverse(A.begin(),A.end());
>     reverse(B.begin(),B.end());
>     integar C;
>     int16_t carry = 0;
>     for(int i = 0; i < (int)max(A.size(), B.size()); i++) {
>         int16_t a = (i < (int) A.size()? A[i] : 0);
>         int16_t b = (i < (int) B.size()? B[i] : 0);
>         int16_t sum = a + b + carry;
>         C.push_back(sum % 10);
>         carry = sum / 10;
>     }
>     C.push_back(carry);
>     trim(C);
>     reverse(A.begin(),A.end());
>     reverse(B.begin(),B.end());
>     reverse(C.begin(),C.end());
>     return C;
> }
> integar operator-(integar &A, integar &B) {
>     if(!geq(A,B)) swap(A,B);
>     reverse(A.begin(),A.end());
>     reverse(B.begin(),B.end());
>     integar C;
>     int16_t borrow = 0;
>     for(int i = 0; i < (int)A.size(); i++) {
>         int a = A[i] - borrow;
>         int b = (i < (int) B.size()? B[i] : 0);
>         if(a < b) a += 10, borrow = 1;
>         else borrow = 0;
>         C.push_back(a - b);
>     }
>     trim(C);
>     reverse(A.begin(),A.end());
>     reverse(B.begin(),B.end());
>     reverse(C.begin(),C.end());
>     return C;
> }
> integar operator*(integar &A, integar &B) {
>     reverse(A.begin(),A.end());
>     reverse(B.begin(),B.end());
>     integar C(A.size() + B.size(), 0);
>     for(int i = 0; i < (int) A.size(); i++) {
>         int carry = 0;
>         for(int j = 0; j < (int) B.size() || carry; j++) {
>             int a = A[i], b = (j < (int) B.size()? B[j] : 0);
>             int sum = C[i+j] + a * b + carry;
>             C[i+j] = sum % 10;
>             carry = sum / 10;
>         }
>     }
>     trim(C);
>     reverse(A.begin(),A.end());
>     reverse(B.begin(),B.end());
>     reverse(C.begin(),C.end());
>     return C;
> }
> integar operator/(integar A,integar B) {
>     integar quotient;
>     if(!geq(A,B)) return {0};
>     int k = A.size() - B.size();
>     while(B.size() < A.size()) B.push_back(0);
>     for(int i = 0; i <= k; i++) {
>         int16_t l = -1, r = 10;
>         while(l+1!=r) {
>             integar m = {(l+r)/2};
>             if(geq(A, B * m)) l = (l+r)/2;
>             else r = (l+r)/2;
>         }
>         integar tmp = {l};
>         integar C = B * tmp;
>         A = A - C;
>         B.pop_back();
>         if(quotient.size() || l != 0)quotient.push_back(l);
>     }
>     // integar remainer = A;
>     return quotient;
> }
> 
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     string a,b;
>     char op;
>     cin >> a >> op >> b;
>     integar x,y;
>     for(int i = 0; i < (int) a.size(); i++) x.push_back(a[i] - '0');
>     for(int i = 0; i < (int) b.size(); i++) y.push_back(b[i] - '0');
>     if(op == '+') {
>         integar ans = x + y;
>         cout << ans;
>     }
>     if(op == '-') {
>         if(!geq(x,y)) {
>             swap(x,y);
>             cout << '-';
>         }
>         integar ans = x - y;
>         cout << ans;
>     }
>     if(op == '*') {
>         integar ans = x * y;
>         cout << ans;
>     }
>     if(op == '/') {
>         integar ans = x / y;
>         cout << ans;
>     }
>     return 0;
> }
> ```
> </details>

> ### [YTP 2024 高中組全國決賽第1題 - No 1 can solve this problem](https://oj.ntucpc.org/problems/442)
>
> 要想一下怎麼解，滿多解法的
>
> <details>
>     <summary> 參考解法 </summary>
> 
> 解法是讓最左邊的 $1$ 進位成 $2$
> 
> ```cpp
> // Author : Zhenzhe
> // Problem : https://oj.ntucpc.org/problems/442
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> using Integar = vector<int16_t>;
> Integar operator-(const Integar &A, const Integar &B) {
>     Integar C;
>     int16_t borrow = 0;
>     for(int i = 0; i < (int)A.size(); i++) {
>         int16_t a = A[i] - borrow;
>         int16_t b = (i < (int)B.size()? B[i] : 0);
>         if(a < b) a += 10, borrow = 1;
>         else borrow = 0;
>         C.push_back(a-b);
>     }
>     while(C.size() > 1 && C.back() == 0) C.pop_back();
>     return C;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     string get;
>     cin >> get;
>     Integar x, minus;
>     bool haveOne = false;
>     for(int i = 0; i < (int) get.size(); i++) {
>         x.push_back(get[i] - '0');
>         if(haveOne) minus.push_back(0);
>         else if(x[i] == 1) haveOne = true, minus.push_back(2);
>         else minus.push_back(x[i]);
>     }
>     reverse(x.begin(), x.end());
>     reverse(minus.begin(), minus.end());
>     Integar ans = minus - x;
>     reverse(ans.begin(), ans.end());
>     for(int i = 0; i < (int)ans.size(); i++) {
>         cout << ans[i];
>     }
>     return 0;
> }
> ```
> </details>


---

## 四、基礎數論

這個地方有四個重點：質數篩、快速冪、因數分解、最大公因數

### 1. 質數篩

複雜度比較好的做法就是埃氏篩法，複雜度大概 $O(N\log \log N)$

簡單來說就是在窮舉質數的過程中把質數的倍數篩選掉

假設說求 $1$ ~ $100$ 中的所有質數，檢查完 $2$ 之後就把所有 $2$ 的倍數都刪掉

以下是模板

```cpp
vector<int> primes;
void handle(const int &MAXN) {
    vector<bool> is_prime(MAXN, 1);
    is_prime[0] = is_prime[1] = 0;
    for(int i = 2; i < MAXN; i++) {
        if(is_prime[i]) {
            primes.push_back(i);
            for(int j = i*i; j < MAXN; j += i) {
                is_prime[j] = 0;
            }
        }
    }
}
```

### 2. 快速冪

就是基本中的基本，但還要對費馬小定理有一定的瞭解

$if\ (a,p) = 1 \rightarrow a^{p-1}\equiv 1(mod\ p)$

以下是 $a^b$ 模板，有遞迴和迴圈兩種

```cpp
int fp(int a,int b) {
    if(b == 0) return 1;
    if(b & 1) return a * fp(a,b-1) % mod;
    int h = fp(a,b>>1);
    return h * h % mod;
}
```

```cpp
int ans = 1;
while(b) {
    if(b & 1) ans *= a;
    a *= a;
    b >>= 1;
}
```

### 3. 因數分解

通常會搭配質數篩做使用，以下是模板

```cpp
map<int,int> factors;
for(auto &prime : primes) {
    if(number < prime || number <= 1) break;
    while(number % prime == 0) {
        factors[prime] += 1;
        number /= prime;
    }
}
if(number > 1) factors.insert({number,1});
```

### 4. 最大公因數

最小公倍數也可以用一樣的求法，要知道 $ab = gcd(a,b)\times lcm(a,b)$

實作上不可能把兩個數字都因數分解玩找相同

所以可以使用 `輾轉相除法` 來實作

```cpp
int gcd(int a,int b) {
    if(b == 0) return a;
    return gcd(b,a%b);
}
```

> ### [UVA 516 - Prime Land](https://zerojudge.tw/ShowProblem?problemid=c088)
>
> 質數篩+因數分解
> <details>
>     <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Problem : https://zerojudge.tw/ShowProblem?problemid=c088
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> static constexpr int MAXN = 2e5+5;
> vector<int> primes;
> void handle() {
>     bitset<MAXN> is_prime;
>     is_prime.set();
>     is_prime[0] = is_prime[1] = 0;
>     for(int i = 2; i < MAXN; i++) {
>         if(is_prime[i]) {
>             primes.push_back(i);
>             for(int j = i*i; j < MAXN; j += i) {
>                 is_prime[j] = 0;
>             }
>         }
>     }
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     handle();
>     string line;
>     while(getline(cin,line)) {
>         if(line == "0") break;
>         stringstream ss;
>         ss << line;
>         string base, power;
>         int number = 1;
>         while(ss >> base >> power) {
>             int a = stoi(base), b = stoi(power);
>             number *= pow(a,b);
>         }
>         number--;
>         map<int,int> factors;
>         for(auto &prime : primes) {
>             if(number < prime || number <= 1) break;
>             while(number % prime == 0) {
>                 factors[prime] += 1;
>                 number /= prime;
>             }
>         }
>         if(number > 1) factors.insert({number,1});
>         for(auto it = factors.rbegin(); it != factors.rend(); it++) {
>             cout << it->first << ' ' << it->second << ' ';
>         }
>         cout << '\n';
>     }
>     return 0;
> }
> ```
> </details>
