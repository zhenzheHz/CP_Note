# 第十章、bitset
---
> 嚴格來說 `bitset` 並不是 STL 的一種

---

## 布林陣列
```cpp
bool check[1000];
vector<bool> check(1000);
```

由於記憶體的位址是以 位元組（byte） 為單位進行尋址，而不是以 位元（bit），所以一個 `bool` 型別的變數，雖然只能表示 `0` 或 `1`，但實際上仍會佔用 `1` 個位元組的空間。

`std::bitset` 則透過固定的優化機制，讓一個位元組中的 8 個位元可以分別儲存 8 個 `0/1`。

以一個 4 位元組（4 byte）的 `int` 為例，如果只是單純用來存放 `0/1` 的資訊，那麼使用 `bitset` 的**空間和時間**佔用僅為它的 $\frac{1}{32}$，也就是 $O(\frac{N}{32})$

### 資料型別與空間效率比較  

| 型別      | 可表達的範圍 | 實際儲存方式         | 空間使用量      | 儲存 0/1 的效率       |
|-----------|--------------|----------------------|-----------------|-----------------------|
| `bool`    | 0 或 1       | 以 **1 byte** 儲存   | 1 byte = 8 bits | 只能存 1 個 0/1       |
| `int`     | 約 ±21 億    | 4 bytes = 32 bits    | 4 bytes         | 只能存 1 個 0/1（浪費）|
| `bitset`  | 0 或 1       | **壓縮在 bit 裡**    | 1 bit           | 1 byte 可存 8 個 0/1  |

---

## 使用
### 標頭檔
```cpp
#include <bitset>
#include <bits/stdc++.h>
```
### 宣告
- `size` 當然也必須是 `constant`
```cpp
bitset<1000> bs;
```
### 構造
- 可以設為 `val` 的二進制形式的 `0/1`
```cpp
int val = 12; // 1100
bitset(val);
```
### 各類 Method
- `count()` 回傳 `1` 的個數
- `test(pos)` 相當於 `vector` 中的 `at(pos)`
- `set()` 全部設為 `1`
- `reset()` 全部設為 `0`
- `flip()` 全部翻轉（`0/1` 顛倒）

---

## 應用
- 有時候可以用一些奇怪的解法在極限邊緣壓過去，畢竟速度快 $32$ 倍
> [例題 : Triangle](https://atcoder.jp/contests/abc258/tasks/abc258_g)
> 
> 作法：開 $N$ 個 `bitset` 暴力解
```cpp
#include <bits/stdc++.h>
#define int int64_t
using namespace std;
int32_t main(){
	ios_base::sync_with_stdio(0);
	cin.tie(0),cout.tie(0);
	int n;cin>>n;
	static constexpr int N = 3001;
	bitset<N> g[N];
	for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++){
			char x;cin>>x;
			g[i][j] = (x=='1');
		}
	}
	int ans = 0;
	for(int i=1;i<=n;i++){
		for(int j=1;j<=i;j++){
			if(g[i][j])ans += (g[i] & g[j]).count();
		}
	}
	cout << ans/3 << '\n';
}
```
