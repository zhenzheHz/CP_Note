# 第八章、映射map 
## 概念
- ~~絕對不是地圖~~
- **key-value pair**
- 類似 Python 的 `dict`

--- 

## 映射的概念

> 映射（英語：map，mapping）或稱射影、寫像，在數學及相關的領域經常等同於函數。基於此，部分映射就相當於部分函數，而完全映射相當於完全函數。在很多特定的數學領域中，這個術語用來描述具有與該領域相關聯的特定性質函數，例如，在拓撲學中的連續函數，線性代數中的線性變換等等。
>
> by Wiki

將一組「鍵（key）」和「值（value）」配對起來。每個鍵對應一個值，通過鍵就能快速找到對應的值
類似現實生活中的字典或對照表。使用映射時，可以方便地進行新增、查找或刪除操作
而且映射通常會自動管理鍵的順序，使資料能有效率地存取。
在程式中，映射非常適合用於統計、計數、索引或任何需要根據某個關鍵資訊快速找到對應結果的情況。

> **溫馨提示**
> 可以把映射 map 的概念想成 `index` 不是數字的 `array`
> 也就是說可能 map 的第 "Zhenzhe" 項等於 8 這樣
> 也就是
> ```cpp
> arr["Zhenzhe"] = 8
> ```

---

## 使用方式
### 標頭檔
```cpp
#include <map>
#include <bits/stdc++.h>
```
```cpp
map<int,int> mp;
```

### map 常用方法與時間複雜度

| 方法           | 功能說明                                   | 時間複雜度 |
|:--------------:|:-----------------------------------------|:----------:|
| `insert({k,v})` | 插入鍵值對 (key, value)，若 key 已存在則無效 | $$O(\log n)$$ |
| `operator[k]`   | 取得或設定 key 對應的 value，如果 key 不存在會自動插入 | $$O(\log n)$$ |
| `erase(k)`      | 移除 key 對應的元素                          | $$O(\log n)$$ |
| `find(k)`       | 查找 key，回傳 iterator 或 `end()`           | $$O(\log n)$$ |
| `count(k)`      | 計算 key 是否存在（0 或 1）                  | $$O(\log n)$$ |
| `begin()`       | 取得最小 key 的 iterator                     | $$O(1)$$     |
| `end()`         | 取得尾端 iterator                            | $$O(1)$$     |
| `empty()`       | 判斷 map 是否為空                            | $$O(1)$$     |
| `size()`        | 回傳 map 中元素個數                          | $$O(1)$$     |

> 當然也有`unordered_map`

---

## 練習題
### [ZJ-a135,Language Detection](https://zerojudge.tw/ShowProblem?problemid=a135)
### [ZJ-b515,摩斯電碼-商競103](https://zerojudge.tw/ShowProblem?problemid=b515)
### [ZJ-e641,Soundex](https://zerojudge.tw/ShowProblem?problemid=e641)
### [ZJ-d267,Letter Frequency](https://zerojudge.tw/ShowProblem?problemid=d267)

