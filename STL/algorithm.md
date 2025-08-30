# 第十一章、演算法模板函數

---

## 簡介
STL 在 `<algorithm>`,`numeric`,`functional` 中提供了相當多的模板函數

因此在比賽中我們不需要再自己書寫常見的演算法

善用這些函式才能在比賽中取得好成績

---

> ### 重要概念
>
> 1. 基本上所有的區間參數都是左閉右開，也就是不包含右界
>
> 2. `string` 也是算是 `char` 的陣列，因此只要陣列能做的操作 `string` 也能做

---

## 排序演算法
- 主要就 `sort()`,`stable_sort()` 兩種，用法相同，複雜度皆為 $O(n\log n)$
- 但其實 `sort()` 會根據陣列大小不同而使用不同的排序演算法而達到最佳複雜度
- `sort(L,R,cmp)` 代表在 $[L,R)$ 區間內使用 `cmp` 的比較方式來排序（默認為`less<int>`）
- `stable_sort()` 為穩定排序，相等元素排序後的相對順序會保持不變
- 對 `string` 排序的話會**按照字典序**


```cpp
int arr[N];
vector<int> vec(N);
// 以下兩種方法皆為對整個陣列排序
// 默認為小排到大
sort(arr,arr+N);
sort(vec.begin(),vec.end());
```

### 由大排到小的方式
- 利用 `rbegin()` 及 `rend()`
```cpp
sort(vec.rbegin(),vec.rend());
```
- 用 `greater<int>`
```cpp
sort(vec.begin(),vec.end(),greater<int>);
```
- 自定義比較方式函式(可以想像第一個參數就是排序前左邊的)
- `comparator` 的寫法務必熟練
```cpp
bool cmp(int a,int b) {
    return a > b;
}
sort(vec.begin(),vec.end(),cmp);
```
- 也可以用 `lambda`
```cpp
sort(vec.begin(),vec.end(),[&](int a,int b){
    return a > b;
});
```

---

## 翻轉
- `reverse(L,R)` 會將 $[L,R)$ 區間的順序調換（e.g. $[1,2,3]\rightarrow [3,2,1]$）
- `string` 當然也能用
```cpp
string str = "ABCDE";
reverse(str.begin(),str.end());
cout << str; // "EDCBA"
```

---

## 搜尋函數

| Function | Functionality | Time Complexity | Notes |
|:--------:|:-------------|:---------------|:-----|
| `binary_search(v.begin(), v.end(), x)` | 判斷容器中是否存在元素 $x$ | $O(\log n)$ | 只返回 true/false，不返回位置 |
| `lower_bound(v.begin(), v.end(), x)` | 回傳第一個 $\geq x$ 的 iterator | $O(\log n)$ | 若不存在，返回 `end()` |
| `upper_bound(v.begin(), v.end(), x)` | 回傳第一個 $> x$ 的 iterator | $O(\log n)$ | 若不存在，返回 `end()` |

---

## 全排列函數
- `next_permutation(L,R)`：將目前的排列更改為 全排列中的下一個排列。如果目前的排列已經是全排列中的最後一個排列（元素完全由大到小排列），函數會回傳 `false`，並將排列更改為全排列中的第一個排列（元素完全由小到大排列；否則，函數回傳`true`
- `prev_permutation(L,R)`：`next_permutation` 的反向操作
> 經常搭配 `do while` 使用
```cpp
int N = 9, a[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
do {
  for (int i = 0; i < N; i++) cout << a[i] << " ";
  cout << endl;
} while (next_permutation(a, a + N));
```

> ### [Luogu P1706 - 全排列問題](https://www.luogu.com.cn/problem/P1706)
>
> 題目難度：*Easy* $(0/10)$

---

## 去重函數
- `unique(L,R)`：去除相鄰的重複元素移動到容器結尾，回傳去重後結尾的 `iterator`
- 舉例：$[1,1,1,2,2,3,4,4,5]\rightarrow [1,2,3,4,5,1,1,2,4]$ ，回傳的 `5` 的位置的迭代器
- 基本上會用在 `離散化` 上

