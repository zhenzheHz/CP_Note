# 第五章、queue
--- 
## 概念
- 中文名稱為 **佇列**
- 先進先出的資料結構(`FIFO`,*First in first out*)
- 就像排隊，先排的（先進）就可以先接受服務（先出）

## 使用方式
### 標頭檔
```cpp
#include <queue>
#include <bits/stdc++.h>
```
### 宣告
```cpp
queue<int> q;
```
---
### queue 常用方法與時間複雜度

|   Method   |        功能說明             | 時間複雜度 |
|:----------:|:---------------------------:|:----------:|
| `push(x)`     | 往隊尾加入一個元素          | $$O(1)$$  |
| `pop()`      | 移除隊首元素                | $$O(1)$$  |
| `front()`    | 取得隊首元素                | $$O(1)$$  |
| `back()`     | 取得隊尾元素                | $$O(1)$$  |
| `empty()`    | 回傳是否為空                | $$O(1)$$  |
| `size()`     | 回傳隊列大小                | $$O(1)$$  |


> **注意**:queue沒有clear功能
---
## deque 雙向佇列

* 全名 `Double-Ended Queue`
* ~~他是念`"deck"`而不是`底Q`~~
* 從前面從後面都有的`queue`
    
### 標頭檔
```cpp=1
#include <deque>
#include <bits/stdc++.h>
```
```cpp
deque<int> dq;
```
### deque 常用方法與時間複雜度

|   Method    |        功能說明         | 時間複雜度 |
|:-----------:|:-----------------------:|:----------:|
| `push_front(x)`| 從最前端加入一個元素    |   $O(1)$     |
| `pop_front(x)` | 從最前端移除一個元素    |   $O(1)$     |
| `push_back()` | 從最後端加入一個元素    |   $O(1)$     |
| `pop_back()`  | 從最後端移除一個元素    |   $O(1)$     |
| `empty()`     | 回傳是否為空            |   $O(1)$     |
| `size()`      | 回傳 `deque` 的大小     |   $O(1)$     |
| `front()`     | 回傳最前端的元素        |  $O(1)$     |
| `back()`      | 回傳最末端的元素        |   $O(1)$     |
| `begin()`     | 回傳指向最前端的迭代器  |   $O(1)$     |
| `end()`       | 回傳指向尾端的迭代器    |   $O(1)$     |

> 看起來好像不錯但常數好像滿大的

---
## Priority Queue 優先佇列
- 有優先順序的佇列
- 以 `int` 為例就是取值時會先以最大的優先(又稱 **最大堆**)
- 也可以自定義優先順序(用 `struct`)
### 標頭檔
```cpp=
#include <queue>
#include <bits/stdc++.h>
```
```cpp=
priority_queue<int> pq;
```
### priority_queue 常用方法與時間複雜度
- 因為底層是 `heap` 所以複雜度有 `log`

|   Method   |        功能說明             | 時間複雜度 |
|:----------:|:---------------------------:|:----------:|
| `push`     | 加入一個元素                | $O(log\ n)$  |
| `pop`      | 移除當前最優先的元素        | $O(log\ n)$  |
| `top`      | 取得最優先的值              | $O(1)$      |
| `empty`    | 回傳是否為空                | $O(1)$      |


### 自訂義優先順序
- 用 `sturct` 寫運算符號
- 以下示範最小堆寫法（`top` 是最小的元素）
```cpp=
struct min_pq{
    int val;
    //就是把原本的'<'改成'>'
    bool operator< (const min_pq &cmp){
        return val > cmp.val;
    }
};
```
---
## 練習
> ### [Zerojudge E447 - queue練習](https://zerojudge.tw/ShowProblem?problemid=e447)
>
> 難度：*Easy* $(1/10)$
> 
> <details>
> <summary>參考解法</summary>
>
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> int main() {
>     queue<int>q;
>     int n;cin>>n;
>     for(int i =0;i<n;i++){
>         int a;cin>>a;
>         if(a == 1){
>             cin >> a;
>             q.push(a);
>         }else if(a == 2){
>             if(q.empty())cout << -1 << '\n';
>             else cout << q.front() << '\n';
>         }else{
>             if(!q.empty()){
>                 q.pop();
>             }
>         }
>     }
>     return 0;
> }
> ```
> 
> </details>

> ### [Zerojudge E155 - Throwing cards away I](https://zerojudge.tw/ShowProblem?problemid=e155)
>
> 難度：*Easy* $(1.5/10)$
> 
> <details>
> <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> int main(){
>     int n;
>     while(cin>>n){
>         if(!n)break;
>         queue<int> q;
>         for(int i = 1;i<=n;i++){
>             q.push(i);
>         }
>         cout << "Discarded cards:";
>         if(n!=1){
>             cout << ' ';
>         }else cout <<endl <<"Remaining card: " << 1 << "\n";
>         while(q.size()!=1){
>             cout << q.front();
>             q.pop();
>             q.push(q.front());
>             q.pop();
>             if(q.size() == 1){
>                 cout << "\n";
>                 cout << "Remaining card: " << q.front();
>                 cout << "\n";
>             }else cout << ", ";
>         }
>     }    
> }
> ```
>  
> </details>


> ### [TIOJ 1489 - 核心字串](https://tioj.ck.tp.edu.tw/problems/1489)
>
> 難度：*Medium* $(3/10)$
> 
> <details>
> <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> 
> inline void solve(int n){
> 	string s;cin>>s;
> 	vector<queue<int>> ap(26);
> 	for(int i=0;i<n;i++){
> 		ap[s[i]-'a'].push(i);
> 	}
> 	for(int i=0;i<26;i++){
> 		if(ap[i].empty()){
> 			cout << "not found\n";
> 			return;
> 		}
> 	}
> 	int l=0,r = 0,segment;
> 	pair<int,int> ans;
> 	for(int i=0;i<26;i++){
> 		r = max(r,ap[i].front());
> 	}
> 	ans = {l,r};
> 	segment = r-l+1;
> 	bool check = false;
> 	while(r < n) {
> 		l += 1;
> 		for(int i=0;i<26;i++){
> 			while(ap[i].size() && ap[i].front() < l){
> 				ap[i].pop();
> 			}
> 			if(ap[i].empty()){
> 				check = true;
> 				break;
> 			}
> 			r = max(r,ap[i].front());
> 		}
> 		if(check)break;
> 		if(r-l+1 < segment){
> 			segment = r-l+1;
> 			ans = {l,r};
> 		}
> 	}
> 	for(int i=ans.first;i<=ans.second;i++){
> 		cout << s[i];
> 	}
> 	cout << '\n';
> }
> int32_t main(){
> 	ios_base::sync_with_stdio(0);
> 	cin.tie(0),cout.tie(0);
> 	int n;
> 	while(cin>>n){
> 		if(n)solve(n);
> 		else break;
> 	}
> }
> ``` 
> </details>






