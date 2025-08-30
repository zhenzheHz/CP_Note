# 第二章、string
---

## 何謂`string`
`std::string` 是在 `<string>` 中的一個型態（並非 C 中的`<string.h>`）
本質上其實是字元陣列，也就是`std::basic_string<char>`
```cpp
const *char[_size];
```
---

## 為何使用`string`
1. `string` 能夠動態分配空間，所以可以直接用`std::cin`來輸入
2. `string` 有 `+` 和 比較運算，也就是說可以用 `sort()` 使其照字典序排序
```cpp
string str = "edcba";
sort(str.begin(),str.end()); // abcde
```

---

## 使用方法
> 直接當作 `vector` 用就好
### 獲取長度
- `strlen(str)`，但是複雜度 $O(|S|)$
- `str.length()`，複雜度 $O(1)$
- `str.size()`，複雜度 $O(1)$，
- 三者回傳的型態皆為 `size_t` (非負整數)

### 尋找第一次出現的位置
- `find(str,pos)` 可以回傳在 `pos` 之後首次出現 `str` 的位置（`pos`預設為0）
- 若沒有出現會回傳 `string::npos`，其值為 `-1`（`size_t`類型）
```cpp
string name = "Zhenzhe";
if(name.find("zhe") == string.npos) cout << "No\n";
else cout << name.find("zhe");
```

### 截取子字串
- `substr(pos,len)` 回傳從原字串中 $[pos:pos+len]$ 的字串

### 末端加入
- `push_back(char)` 在末端加入字元
- `append(string)` 在末端加入字串
```cpp
str.push_back('k');
str.append("kkk");
```
