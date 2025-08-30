# 第七章、set
---
## 概念
- 就是數學上的 **集合**
- 元素並不會重複
- 會自動小到大排序（字典排序）

## 使用方式
### 標頭檔
```cpp
#include <set>
#include <bits/stdc++.h>
```
```cpp
set<int> s;
```

### set 常用方法與時間複雜度
- `set` 

|   Method       | 功能說明                          | 時間複雜度 |
|:--------------:|:----------------------------------|:----------:|
| `insert(x)`     | 插入元素 x（若已存在則無效）       | $$O(\log n)$$ |
| `erase(x)`      | 移除元素 x（若不存在則無效）       | $$O(\log n)$$ |
| `find(x)`       | 查找元素 x（返回 iterator 或 end） | $$O(\log n)$$ |
| `count(x)`      | 計算元素 x 是否存在（0 或 1）     | $$O(\log n)$$ |
| `begin()`       | 取得最小元素的 iterator           | $$O(1)$$     |
| `end()`         | 取得尾端 iterator                 | $$O(1)$$     |
| `empty()`       | 判斷是否為空                       | $$O(1)$$     |
| `size()`        | 回傳元素個數                        | $$O(1)$$     |



## ==multiset==
- 可支援重複元素的set
- erase(數字)的話會全刪，只要刪一個就要刪除他的iterator
- 當然也有`unordered_set`,`unordered_multiset`
```cpp=
multiset<int> ms;
```

## ==練習題==
### [ZJ-f607,切割費用](https://zerojudge.tw/ShowProblem?problemid=f607)
### [ZJ-d123,B2-Sequence](https://zerojudge.tw/ShowProblem?problemid=d123)
### [ZJ-d442,Happy Number](https://zerojudge.tw/ShowProblem?problemid=d442)
