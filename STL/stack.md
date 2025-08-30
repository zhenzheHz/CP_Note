# 第六章、stack
---
## 概念
![stack](https://hackmd.io/_uploads/Sy0e-Cqv0.png)
- 中文為 **棧**
- 一種先進後出(`LIFO`, *Last In First Out* ) 的資料結構
- 一個桶子（底部封閉），放東西進去後又放東西則就需要把後放的先拿出來，先放的才出的來
---
## 使用方式
### 標頭檔
```cpp
#include <stack>
#include <bits/stdc++.h>
```
```cpp
stack<int> stk;
```

### stack 常用方法與時間複雜度
|   Method   | 功能說明                   | 時間複雜度 |
|:----------:|:---------------------------|:----------:|
| `push(x)`     | 將元素加入堆疊頂端          | $O(1)$  |
| `pop()`      | 移除堆疊頂端的元素          | $O(1)$  |
| `top()`      | 取得堆疊頂端元素            | $O(1)$  |
| `empty()`    | 回傳堆疊是否為空            | $O(1)$  |
| `size()`     | 回傳堆疊元素個數            | $O(1)$  |

> 注意 : 如果stack為空，使用pop會出錯

---

## vector 其實更好用？
- 基本上 `stack` 的操作都可以用 `vector` 取代
- 而且別看操作都是 $O(1)$ ，實際上他常數很大
| stack | vector |
|:------:| :------:|
| push() | push_back() |
| top() | back() |
| pop() | pop_back() |
| empty() | empty() |
| size() | size() |

---

## 練習

> ### [Zerojudge E924 - 括號配對](https://zerojudge.tw/ShowProblem?problemid=e924)
>
> 難度：*Easy* $(2/10)$
> 
> 重要的經典題，**必須學會**
> <details>
>   <summary> 參考解法 </summary>
> 把各種括號用 $0$ ~ $7$ 代替，同一種就差 $4$ 比較好實作
> 
> ```cpp
> #include <iostream>
> #include <vector>
> using namespace std;
> int transfer(char c){
>     string dict = "([{<)]}>";
>     for(int i = 0;i<8;i++){
>         if(c == dict[i]){
>             return i;
>         }
>     }
> }
> int main(){
>     int n;cin>>n;
>     for(int i = 0;i<n;i++){
>         string a;cin>>a;
>         vector<int>v;
>         bool closed = true;
>         for(char i : a){
>             int t = transfer(i);
>             if(t < 4)v.push_back(t);
>             else{
>                 if(v.empty() and t > 3){
>                     closed = false;
>                     break;
>                 }
>                 if(t - v.back() == 4){
>                     v.pop_back();
>                 }else{
>                     closed = false;
>                     break;
>                 }
>             }
>         }
>         if(!v.empty())closed = false;
>         cout << ((closed)? "Y\n" : "N\n");
>     }
> }    
> ```
> </details>

