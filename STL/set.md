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
- `set` 的底層實作是用 **紅黑樹**，所以複雜度有 `log`

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

---

## multiset
- 可支援重複元素的 `set`
- `erase(num)` 的話會把所有數值為 `num` 的全部刪調
- 若只要刪除一個就要刪除他的 `iterator`
- 當然也有 `unordered_set`,`unordered_multiset`（基本上沒什麼用就是了）
```cpp
multiset<int> ms;
for(int i=0;i<10;i++) ms.insert(3); // 有 10 個 3
ms.erase(ms.find(3)); // 刪掉1個，剩下9個
ms.erase(3); // 全部刪掉
```

---

## 注意事項
- `set` 的 `iterator` 不能加減
- 有一個算兩個 `iterator` 距離的函式 `std::distance(it1,it2)`，複雜度 $(n)$ ，請不要使用
- `erase()` 是會回傳刪掉之後下一個元素的 `iterator`，因此可以用迴圈歷遍不會有問題
- `set` 的操作請務必熟悉 `iterator`
- 使用 `find()` 若沒找到該元素則會返回 `end()`（練習題第三題中會用到）
```cpp
set<int> s;
for(auto it = s.begin(); it != s.end(); ) {
  if(*it < 100) it = s.erase(it);
  else it = next(it);
}
```
---

## 練習題
> ### [Zerojudge F607 - 切割費用](https://zerojudge.tw/ShowProblem?problemid=f607)
>
> 難度：*Easy* $(2/10)$
> 
> <details>
>   <summary> 參考解法 </summary>
> ```cpp
> #include <iostream>
> #include <set>
> #include <vector>
> using namespace std;
> #define int long long
> signed main(){
>     int n,L;cin>>n>>L;
>     int a,b,ans = 0;
>     vector<int>v(n);
>     set<int>s;
>     for(int i =0;i<n;i++){
>         cin>>a>>b;
>         v[b-1] = a;
>     }
>     s.insert(0);s.insert(L);
>     for(int i = 0;i<n;i++){
>         auto it = s.lower_bound(v[i]);
>         int cost = *it;
>         it--;
>         cost -= *it;
>         ans += cost;
>         s.insert(v[i]);
>     }
>     cout << ans;
> }
> ```
> </details>

> ### [Zerojudge D123 - B2-Sequence](https://zerojudge.tw/ShowProblem?problemid=d123)
>
> 難度：*Easy* $(2.5/10)$
> 
> <details>
>   <summary> 參考解法 </summary>
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> #define Nline int t;cin>>t;for(int i=0;i<t;i++)
> #define EOF(i) int i;while(cin >> i)
> #define declare int x;cin>>x;
> #define blank ' '
> #define endl '\n'
> int main(){
>     int Case = 1;
>     EOF(t){
>         int n;
>         bool flag = true;
>         int appear[20001],arr[t];
>         for(int i = 0;i<20002;i++)appear[i]=0;
>         for(int i = 0;i<t;i++){
>             cin >> arr[i];
>         }
>         if(arr[0] < 1){
>             flag = false;
>         }
>         for(int i = 0;i<t-1;i++){
>             if(arr[i+1] <= arr[i]){
>                 flag = false;
>                 break;
>             }
>         }
>         for(int i = 0;i<t;i++){
>             for(int j = 0;j<t;j++){
>                 if(i <= j){
>                     if(appear[arr[i]+arr[j]] > 0){
>                         flag = false;
>                         break;
>                     }
>                     appear[arr[i]+arr[j]]++;
>                 }
>             }
>         }
>         cout << "Case #" << Case;
>         cout << ": It is ";
>         if(!flag)cout << "not ";
>         cout << "a B2-Sequence.";
>         Case++;
>         cout << endl << endl;
>     }
> }
> ```
> </details>

> ### [Zerojudge D442 - Happy Number](https://zerojudge.tw/ShowProblem?problemid=d442)
>
> 難度：*Easy* $(2/10)$
> 
> <details>
>   <summary> 參考解法 </summary>
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> int Case = 1;
> void solve() {
>     int x, k;
>     cin >> x;
>     k = x;
>     unordered_set<int> pre;
>     pre.insert(x);
>     cout << "Case #" << Case++ << ": ";
>     for( ; ;) {
>         if(x == 1) return cout<< k <<" is a Happy number.\n",void();
>         int tmp = x, sum = 0;
>         while(tmp) {
>             sum += (tmp % 10) * (tmp % 10);
>             tmp /= 10;
>         }
>         x = sum;
>         if(pre.find(x) == pre.end()) pre.insert(x);
>         else return cout<< k << " is an Unhappy number.\n",void();
>     }
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int test = 1;
>     cin >> test;
>     while(test--) solve();
> }
> ```
> </details>


