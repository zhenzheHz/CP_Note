# 前置處理器
---
## 前置處理是什麼？
- $C$++是一種編譯語言，需要編譯才能執行
- 前置處理就是在編譯前的階段要做處理動作
- `#`是前置處理的指令
- 常用的就是`#include`,`#define`
---
## include
- `#include`會打開後面那個東西打開然後複製貼上
```text
This is content in file.txt
```
```cpp
cout << #include "file.txt" << '\n';
```
- 當然通常只是拿來用函式庫而已
```cpp
#include <bits/stdc++.h>
```
---
## define
- `#define`會把程式碼中的字元替換掉（用來偷懶）
- 僅僅只是替換，不會運算
```cpp
#define a+b 1+7
```
```cpp
int a = a+b*10;
```
- 這樣結果是71，而非80，因為替換後變成1+7*10
- 所以通常要運算會加括號
```cpp
#define mid (left+(right-left)/2)
```
- 常用大概就以下幾種
```cpp
#define endl '\n'
#define F first
#define S second
#define pb push_back
#define pii pair<int,int>
```
- 也可以用來當作函式
```cpp
#define plus(a,b) (a+b)
```
- 圖論常用
```cpp
#define inRange(i,j) (i>=0&&i<row&&j>=0&&j<column)
```
- ~~邪教~~
```cpp
#define int long long
signed main(){
    //code...
}
```
```cpp
#define int int64_t
int32_t main(){
    //code...
}
```
---
## 參考資料
- [[C 語言] 程式設計教學：如何使用巨集 (macro) 或前置處理器 (Preprocessor)](https://opensourcedoc.com/c-programming/preprocessor/)
- [WiwiHo的競程筆記](https://cp.wiwiho.me/preprocessor/)


