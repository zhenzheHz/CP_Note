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
