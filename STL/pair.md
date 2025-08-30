# 第三章、pair 與 tuple
---

## pair 簡介
`std::pair` 是 STL 中的一個**類模板**，可以一次將兩個變數結合形成一個 "對(pair)"
並且兩種變數的型態可以不同

---

## 使用
### 訪問
- 有 `first` 和 `second`  分別代表第一個和第二個
### 初始化
- 定義時指定
```cpp
pair<int,string> zz(17,"Zhenzhe);
```
- 定義後賦值
```cpp
pair<int,double> P;
P.first = 1;
P.second = 3.14
```
- `make_pair()` 函數
```cpp
pair<int,int> P = make_pair(3,4);
```
- 真方便
```cpp
pair<int,int> P = {3,4};
```
### 比較
`std::pair` 預設的比較方式是先比第一個，若一樣再比第二個
```cpp
pair<int,int> a = {1,2};
pair<int,int> b = {1,3};
// b > a
```

---

## tuple 的應用
若只有兩個還是不夠用要怎麼辦，這時候~~你可以使用 `tuple`~~
> 說是這樣說，但沒叫你這樣做
> 這時候使出 `struct` 自己定義就好

### 用法介紹
`std::tuple` 是 `std::pair` 的推廣，可以儲存多種變數
用法跟 `std::pair` 差不多，`make_pair()` 改成 `make_tuple()`
```cpp
tuple<int,int,double,string,pair<int,int>> tup;
tup = make_tuple(1,2,3.14,"zz",make_pair(2,3));
```
訪問時使用 `get<index>(tuple)` 即可
```cpp
int tmp = get<1>(tup);
```