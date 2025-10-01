# 第二章、暴力枚舉

---

## 簡介

**枚舉** (Enumerate)，顧名思義，也叫做窮舉，即考慮所有可能的答案去取需要的結果

舉例來說：
假設你今天要決定出門要不要帶：
- 雨傘
- 外套

每個東西都有「要帶」或「不帶」兩種選擇。  
- 如果只有雨傘 $\rightarrow 2$ 種情況  
- 如果有雨傘 + 外套 $\rightarrow 2 × 2 = 4$ 種情況  
- 如果還有帽子 $\rightarrow 2 × 2 × 2 = 8$ 種情況  

這就是窮舉：**全部列出來**

---

## 迴圈枚舉

利用 `for` 迴圈枚舉所有可能情況，是 **比賽中經常會出現的子題模式**，我們只需要考慮所有子樣本空間即可求出最後答案

> 複雜度 $O(n^k)$ $(k$ 層迴圈 $)$

先看個簡單的例題

> ### [Atcoder ABC389 B - tcaF](https://atcoder.jp/contests/abc389/tasks/abc389_b)
>
> 題目大意：給定一個值 $X$，求 $N$ 滿足 $N! = X$
>
> 限制：$2\leq X\leq 3\times 10^{18}$

很顯然，我們只要窮舉 $[1,20]$ 中所有可能直接比對就能知道答案

不過這題太簡單大家都會

> ### [Atcoder ABC393 B - A..B..C](https://atcoder.jp/contests/abc393/tasks/abc393_b)
>
> 題目大意：給定字串 $S$，問有幾個點對 $(i,j,k)$ 滿足 $i<j<k$ ， $2j = k + i$，並且 $(S_i,S_j,S_k) = (A,B,C)$
>
> 限制：$3\leq |S|\leq 100$

因為 $S$ 的長度很小，因此直接枚舉所有可能即可

不過注意到因為 $2j = k + i$，因此我們只需要枚舉 $i,j$ 兩個可能， $k$ 就會順便被決定好

```cpp
int ans = 0;
for(int i=0;i<s.size();i++) {
    for(int j=i+1;j<s.size();j++) {
        int k = 2 * j - i;
        if(s[i] == 'A' && s[j] == 'B' && s[k] == 'C') {
            ans += 1;
        }
    }
}
```

這樣複雜度 $O(|S|^2)$

---

## Bitmask 枚舉子集

Bitmask 可以想像是一個 **用 0 和 1 表示選擇的密碼**。

- `0` 代表「不選」
- `1` 代表「選」

舉例：
還是雨傘 + 外套 + 帽子：
- `000` → 三個都不帶
- `001` → 只帶帽子
- `010` → 只帶外套
- `011` → 帶外套和帽子
- `100` → 只帶雨傘
- `101` → 帶雨傘和帽子
- `110` → 帶雨傘和外套
- `111` → 三個都帶

總共 8 種情況，剛好對應二進位的數字 `0 ~ 7`。

如果我們用「巢狀 for 迴圈」去列舉會很麻煩：

```cpp
for (雨傘的選擇) {
    for (外套的選擇) {
        for (帽子的選擇) {
            // 做事情
        }
    }
}
```

一旦物品很多，巢狀迴圈就會變得超長。
但是用 bitmask，我們只需要一個迴圈：

```cpp
for (int mask = 0; mask < (1 << n); mask++) {
    // 用 mask 來表示一種選擇
}
```

這樣就能列出 所有 $2^n$ 種可能。

### 實作概念

> 先確保自己已經熟練位元運算 `&`,`|`,`^`,`>>`,`<<`...

如何判斷某個東西有沒有被選？

假設我們有一個數字 $mask$，它的第 $i$ 個 bit 表示第 $i$ 個東西有沒有被選。

要檢查第 $i$ 個東西就是利用 `&` 運算判斷是否為 $1$

```cpp
if (mask & (1 << i)) {
    // 也可以寫成 (mask >> i) & 1
    // 第 i 個東西有被選
} else {
    // 第 i 個東西沒有被選
}
```

### 範例程式

> 假設有一個長度為 $n$ 的序列 $A_n$，則以下程式為列舉其所有可能選擇方法的寫法

```cpp
vector<int> A(n);
for(int status = 0; status < (1<<n); status++) {
    for(int i = 0; i < n; i++) {
        if((status >> i) & 1) {
            // 1的情況，即有選擇
        }
        else {
            // 0 的情況，即不選擇
        }
    }
}
```

---

## 枚舉排列情形

排列（Permutation）指的是把一組元素依照不同的順序重新排好。

例如：$\{1, 2, 3\}$ 的所有排列有以下六種

> 123,132,213,231,312,321

`next_permutation()` 可以在字典序下生成下一個排列。

如果目前排列是「字典序中某一個」，它會改變容器中的元素為下一個排列，並回傳 `true`。

如果目前排列已經是「最大字典序」，它會將容器改回最小字典序，並回傳 `false`。

`prev_permutation()` 則是反過來

搭配 `do...while()` 可以枚舉子狀態的排列情況

以八皇后問題為例：

> ### [八皇后問題](https://zerojudge.tw/ShowProblem?problemid=i644)
>
> 有幾種放置方法使得在 $8\times 8$ 的西洋棋盤上，放置 $8$ 個皇后，使她們之間互不攻擊(皇后會攻擊 同一列、同一行、同一斜線)

八皇后有三個「互不攻擊」的限制：

- 同一行 不能有兩個皇后

- 同一列 不能有兩個皇后

- 同一斜線 不能有兩個皇后

直觀思路會想：我是不是要在 $8\times 8$ 棋盤上做回溯，一種一種試？

這樣就有 $64$ 格可選，可能性很多，計算量很大。

但是我們可以 **觀察到**

棋盤是 $8$ 行，我們必須放 $8$ 個皇后 $\rightarrow$ 每行必放 $1$ 個皇后。

如果在第 $i$ 行放在第 $row[i]$ 列，就能把「每行」的限制自然滿足。

這樣我們馬上消掉了第一個限制（每行唯一）。

再來，同一列不能有兩個皇后 $\rightarrow$ 如果 $row$ 是一個排列，那每個數字 $1$ ~ $8$ 剛好出現一次，也就保證了每列唯一。

因此我們可以用 `next_permutation()` 來完成此種枚舉，也就是檢查所有 $1$ ~ $8$ 的排列是否合法

這樣一來我們只需要檢查斜角是否會攻擊到彼此就行

棋盤上的斜線有一個特徵

兩個點 $(i, row[i])$ 和 $(j, arr[j])$ 在同一斜線上 $\rightarrow$ $|i - j| == |arr[i] - arr[j]|$

所以我們只要在排列中檢查這個條件即可

當我們廣泛討論在 $n\times n$ 的棋盤上放置 $n$ 個皇后時

檢查斜角合法性的複雜度為 $O(n)$

枚舉所有排列情形複雜度為 $O(n!)$

故複雜度為 $O(n\times n!)$

```cpp
int solutions = 0;
do {
    bool ok = true;
    for (int i = 0; i < 8; i++) {
        for (int j = i+1; j < 8; j++) {
            if (abs(i - j) == abs(row[i] - row[j])) {
                ok = false;
                break;
            }
        }
        if (!ok) break;
    }
    if (ok) {
        solutions++;
        for (int i = 0; i < 8; i++) {
            for (int j = 0; j < 8; j++) {
                cout << (row[i] == j ? "Q " : ". ");
            }
            cout << "\n";
        }
        cout << "------------------\n";
    }
} while (next_permutation(row.begin(), row.end()));
```

---
## 練習

> ### [Bocchi's Setlist](https://codeforces.com/gym/634550/problem/A)
>
> 難度： $3.5/10$
> 
> 題目位於 [Codeforces 上的題單](https://codeforces.com/contestInvitation/e60938670c2d65c4bb70f162d34e0f640dc900e8)中

> ### [YTP 2024程式挑戰營Problem. 2 能源危機](https://oj.ntucpc.org/problems/443)
>
> 難度： $4.5/10$
> 
> <details>
>     <summary> 參考解法 </summary>
> 
> 作法：枚舉操作三合併前的兩個子集合，並枚舉所有可能乘法情況
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> int popcnt(int x) {
>     int ret = 0;
>     while(x) {
>         ret += x & 1;
>         x >>= 1;
>     }
>     return ret;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int n,k,x,y,z;
>     cin >> n >> k >> x >> y >> z;
>     vector<int> A(n),factors;
>     for(int &i : A) cin >> i;
>     for(int i = 1; i*i <= k; i++) {
>         if(k % i == 0) factors.push_back(i);
>     }
>     int ans = LLONG_MAX;
>     for(int s1 = 1; s1 < (1<<n); s1++) {
>         for(int s2 = 1; s2 < (1<<n); s2++) {
>             if(s1 & s2) continue;
>             int a = 0, b = 0;
>             for(int i = 0; i < n; i++) {
>                 if((s1 >> i) & 1) a += A[i];
>                 if((s2 >> i) & 1) b += A[i];
>             }
>             for(auto factor : factors) {
>                 int cost = z * (popcnt(s1) - 1 + popcnt(s2) - 1);
>                 cost += (a > factor ? y : x) * llabs(a - factor);
>                 cost += (k/factor > b ? x : y) * llabs((int)k/factor - b);
>                 ans = min(ans,cost);
>             }
>         }
>     }
>     return cout<<ans,0;
> }
> ```
> </details>


> ### [UVA 10098 - Generating Fast, Sorted Permutation](https://zerojudge.tw/ShowProblem?problemid=d436)
>
> 難度： $3/10$
>
> <details>
>     <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Time : 2024 / 11 / 07
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int test;string str;
>     cin >> test;
>     while(test--) {
>         cin >> str;
>         sort(str.begin(),str.end());
>         do {
>             cout << str << '\n';
>         } while(next_permutation(str.begin(),str.end()));
>         cout << '\n';
>     }
> }
> ```
> </details>

> ### [UVA 10344 - https://zerojudge.tw/ShowProblem?problemid=d762](https://zerojudge.tw/ShowProblem?problemid=d762)
>
> 難度： $3/10$
>
> <details> 
>     <summary> 參考解法 </summary>
> 
> ```cpp
> // Author : Zhenzhe
> // Time : 2024 / 11 / 02
> // Problem : https://zerojudge.tw/ShowProblem?problemid=d762
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> char oper[3] = {'+','-','*'};
> int Array[5];
> bool dfs(int index,int cur) {
>     if(index == 5) return cur==23;
>     bool check = 0;
>     if(dfs(index+1,cur+Array[index])) check = 1;
>     if(dfs(index+1,cur-Array[index])) check = 1;
>     if(dfs(index+1,cur*Array[index])) check = 1;
>     return check;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     while(cin >> Array[0] >> Array[1] >> Array[2] >> Array[3] >> Array[4]) {
>         if(Array[0]==0 && Array[1]==0 && Array[2]==0 && Array[3]==0 && Array[4]==0)
>             break;
>         sort(Array,Array+5);
>         bool capalbe = 0;
>         do {
>             if(dfs(1,Array[0])) capalbe = 1;
>         }while(next_permutation(Array,Array+5));
>         if(capalbe) cout << "Possible\n";
>         else cout << "Impossible\n";
>     }
> }
> ```
> </details>

