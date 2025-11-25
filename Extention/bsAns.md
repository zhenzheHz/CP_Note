# 第一章、對答案二分搜
---

## Introduction

我們常遇到這類問題：

* 找到「最小的 X 使條件成立」
* 找到「最大的 X 使條件仍成立」
* 最小值最大化問題 / 最大值最小化問題

如果我們可以判斷「給定一個候選答案 $k$，它是否可行（YES/NO）」，那麼整個問題就能透過二分搜尋快速解決。這就是 **答案二分搜尋** 的核心概念

---

## 一、為什麼可以對「答案」二分搜？

二分搜尋的基本前提是：**答案的可行性必須具備單調性（Monotonicity）。** 也就是說，答案空間呈現以下其中一種結構（`1` 代表可行，`0` 代表不可行）

* **00000000001111111111111111**
* **11111100000000000000000000**

一旦找到這樣的性質，我們就能把答案視為「排序後的整數」，進而對答案做二分。比如說當 $k$ 可行時，$k+1,k+2,...,k+n$ 必定可行這種形式

舉例來說：當我們知道雞蛋從 $3$ 樓掉下來一定會碎，那麼我們可以知道從 $4,5,6,7,...,3+n$ 樓掉下來都會碎

---

## 二、解題流程

答案二分具有固定的思考模板，大致寫法如下，二分搜的寫法可以參考[](// 左開右開寫法，可參考[二分搜尋法](https://zhenzhehz.github.io/CP_Note/#Basic/Binary_search.md))

- **(1) 定義答案的範圍** $[L,R]$
- **(2) 設計一個 check function**
- **(3) 利用二分搜尋得到答案**（你要會寫二分搜）

```cpp
int l,r;
while(l+1 != r) {
  int mid = (l+r) / 2;
  // 視情況調整左右邊界
  if(check(mid)) r = mid;
  else l = mid;
}
// 答案是 l 或 r 其中一個（看題目）
```

如果 **check的時間複雜度是** $O(f(n))$，整體的時間複雜度會是 $O(f(n)\times (R-L))$

---

## 三、典型使用場景

對答案二分搜算是一個間單好懂的小技巧，，APCS有機會考，如果出了要好好把握，而在一般正式的比賽中，通常困難的點是 `cheek` 很難寫或者單調性不容易發現

### **1.滿足條件的最大值 / 最小值**

> ### [2024.1月 TOI練習賽潛力組 - 遊戲升等](https://zerojudge.tw/ShowProblem?problemid=f815)

這題就是很典型的找符合條件的最大最小值問題，可以發現如果可以升到 $x$ 等，那麼升到 $x-1,x-2,x-3$ 必定也可以，因此必定存在一個臨界值（最大值）是符合條件的，這就是我們要的單調性，因此我們可以對答案（等級）二分搜

```cpp
#include <bits/stdc++.h>
#define int int64_t
using namespace std;
int n,m;
bool check(vector<int> &s,int level){
    int cost = 0;
    for(int &i : s){
        if(i < level){
            cost += (level-i)*(level-i);
        }
    }
    return (cost<=m);
}
int32_t main(){
    ios_base::sync_with_stdio(0);
    cin.tie(0),cout.tie(0);
    cin>>n>>m;
    vector<int> a(n);
    for(int &i : a)cin >> i;
    int l = -1,r = 1e7+1;
    while(l+1 != r){
        int m = (l+r)>>1;
        if(check(a,m))l = m;
        else r = m;
    }
    cout << l;
}
```

### **2. 最大值最小化 ／ 最小值最大化問題**

> ### [CSES - Array Division](https://cses.fi/problemset/task/1085)

我們要最小化「最大子陣列和」。假設我們猜一個值 **M**，問：「能否把陣列劃分為 $\leq k$ 段，使得每一段的和 $\leq M$？」如果可以，那麼我們定義「最大子陣列和 $\leq M$」是可行的。反過來，如果不可以，那麼「最大子陣列和 $\leq M$」是不可行的。由於 $M$ 越大，限制就越鬆；$M$ 越小，限制越嚴。這樣就具備單調性：若某 $M$ 可行，那所有 $> M$ 的也可行；若某 $M$ 不可行，那所有 $< M$ 的也不可行。於是我們可以在答案空間上做二分搜尋。

- 設定答案範圍

最小可能值：因為至少一段會含有最大的一個元素，所以最小 $M$ 至少是 $max(x_i)$。
最大可能值：當 $k=1$ 時，整個陣列是一段，其和為 $sum(x_i)$。所以最大 $M$ 最多是 $sum(x_i)$。

因此範圍是 $[max(x_i),\sum_{i=0}^{n-1]x_i]$

- 定義 check(M)：能否在每段和 $\leq M$ 的限制下，使用 $\leq k$ 段把陣列劃分？

方法：使用 `greedy` + `sliding window` 即可

- 用二分搜尋找最小可行 $M$

- 時間與空間複雜度分析：每次 check 需要 $O(n)$ 時間，$log(R-L) ≲ log(n·10^9) \leq 40$，因此整體複雜度為 $O(nlog(R-L))$

```cpp
#include <bits/stdc++.h>
#define int int64_t
#define fastio ios_base::sync_with_stdio(0);cin.tie(0),cout.tie(0)
#define endl '\n'
using namespace std;
int n,k;
int check(vector<int>&arr,int x){
    int cnt = 0;
    int save = 0;
    for(int i=0;i<n;i++){
        if(save + arr[i] > x && save){
            save = arr[i];
            cnt += 1;
        }else save += arr[i];
    }
    if(save)cnt++;
    return cnt;
}
int32_t main(){
    fastio;
    cin >> n >> k;
    vector<int> arr(n);
    int mx = -1,sum = 0;
    for(int i=0;i<n;i++){
        cin >> arr[i];
        mx = max(arr[i],mx);
        sum += arr[i];
    }
    int l = mx-1,r = sum+1;
    while(l+1!=r){
        int m = (l+r)>>1;
        if(check(arr,m)<=k)r = m;
        else l = m;
    }
    cout << r;
}
```




