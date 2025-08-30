# 第四章、vector
---
> ***Translation by Cambridge***
> 
> vector (n)向量
> 
> something physical such as a force that hassize and direction

---
## 靜態陣列(Array)的缺點
- 一塊連續儲存空間的資料
- 這個空間一開始就要決定好，不能更改
> 溫馨提示：
> `array` 宣告在 `global` 時要用 `const` 當作 `size`（常數）
```cpp
const int N = 2e5+5;
int arr[N];
```
---
## 為何要使用 vector
在競賽程式中，對程式效率的要求往往遠高於對工程層級穩定性的考量。而由於 `vector` 需要動態分配與管理記憶體，在某些情況下，其執行效率不如靜態陣列；若再加上 OJ 伺服器未必開啟完整最佳化，情況可能更為不利。

因此，在一般單純的資料儲存情境下，通常不會優先選擇 `vector`。

不過，`vector` 仍具備一些相當優秀的特性，在需要運用這些特性的場合，vector 能夠發揮極大的作用。
### 動態分配記憶體
很多時候，我們無法事先開出那麼大的空間（例如：題目的 $n$ 給到 $10^6$）。雖然整體資料量可能仍在記憶體可承受的範圍內，但單筆資料的大小卻可能非常龐大。
這種情況下，我們就需要使用 `vector`，以便將記憶體使用量控制在更合適的範圍。
此外，`vector` 具備動態擴容的能力，在記憶體資源相當緊張的情境下，這個特性便能發揮重要作用。

---

## 標頭檔
- 要確保引用 `std` 命名空間
```cpp
#include <vector>
#include <bits/stdc++.h>
```

---
## 使用方式

### 宣告
```cpp
//vector<Type> Name;
vector<int> v;
```
## 指定初值
### 直接指定元素
```cpp
vector<int> v1 = {1,2,3,4};
```
### 複製(都是左閉右開區間)
```cpp
vector<int> copy_v(v1);
```
```cpp
vector<int> copy_v2(v1.begin(),v.end()-1);
//1,2,3
```
```cpp
int a = {1,2,3,4};
vector<int> copy_v3(a,a+4);
//1,2,3,4
```
### 預先設定長度
- 每個空間的值會先預設為0（如果是 **int**）
```cpp
vector<int> pre_v(5);
//0,0,0,0,0
```
### 指定長度＋初值
```cpp
vector<int> pre_v2(6,2);
//2,2,2,2,2,2
```
### vector 夾 vector
```cpp
vector<vector<int>> g(3,vector<int>(4));
// 相當於 3 x 4 的二維陣列
```

--- 

## 各類 Method
> 都以這個 `vector` 操作
```cpp
vector<int> v = {1,1,0};
```
### `at()`取值
- 跟 `array` 的 `[]` 是一樣的，當然也支援 `[]` 操作
> 比賽小技巧：
> `at(-1)`：如果取超出範圍會回傳 <font color="red"> `std::out_of_range` </font>
> `[-1]`：取超出範圍你只會看到 <font color="red"> `Segmeantation fault` </font>（記憶區段錯誤）
```cpp
int a = v.at(0);
int b = v[0];
```

### 取值操作 `front()`,`back()`
- `front()` 回傳容器第一位元素值
- `back()` 回傳容器最後一位元素值
```cpp
// v => 1 , 1 , 0
cout << v.front(); // 1
cout << v.back(); // 0
```

### `push_back()` 尾端加入元素
- 用 `emplace_back()` 也可以（但要該資料型態有構造函數）
- 複雜度 $O(1)$
```cpp
v.push_back(2);
//1,1,0,2
```
```cpp
v.emplace_back(4);
//1,1,0,2,4
```
### `pop_back()` 刪除尾端元素
- 不能有參數，不然會出錯
- 複雜度 $O(1)$
- 想刪除特定元素要用 `erase`，但複雜度很差( $O(n)$ )
```cpp
for(int i=0;i<3;i++){
    v.pop_back();
}
//1,1
```
### `size`,`empty`,`clear`
- `size` 回傳陣列長度
- `empty` 回傳陣列是否為空（**boolean**）
- `clear` 清空整個陣列
```cpp
cout << v.size(); //2
cout << v.empty(); //0 -> false
v.clear(); //nothing
cout << v.empty(); //1 -> true
cout << v.size(); //0
```
### `resize()` 重新設定 `vector`
> resize(size,value)
```cpp
v.resize(5); //0,0,0,0,0
v.resize(3); //0,0,0
v.resize(2,1); //1,1
```
---

## 迭代器(iterator)
- 會指向一個位址，而非一個值
![](https://i.cmpnet.com/ddj/cuj/images/cuj0106smeyers/diagram2.gif)
- 開頭加上$r$就是倒轉 (reverse 的意思)
- `end()`是陣列最後一個的下一個
- 整個陣列的範圍是 `begin()` ~ `end()`，為左閉右開區間(也就是不含 `end()`)
- 運算就直接使用 `+` , `-` 就好了

---

## std::array
`std::array` 實際上是 STL 對原生陣列的一層封裝。與 `vector` 相比，它放棄了動態擴容的特性，但因此換得了與原生陣列幾乎一致的效能（在開啟完整最佳化的情況下）。
因此，在能夠使用 C++11 特性的情況下，幾乎所有需要定長陣列的地方都可以直接以 `array` 取代，而若需要動態分配陣列，則可以使用 `vector`。
```cpp
vector<array<int,5>> arr;
```
