# 第八章、二分搜尋法
---
## 終極密碼
> 要從 $1$ ~ $1000$ 猜到指定數字，怎麼保證能盡快猜到

- 你當然可以從 $1$ 開始猜（就是線性搜,暴力搜）
  
- 很顯然大部分人都會是 $500,250,125...$ 一次除一半去猜
- 二分搜就是這個概念，每次都砍一半
- 雖然概念就是很簡單但是你比賽的時後就是有可能把邊界寫爛

|   方法   |    概念    | 限制 | 複雜度 |
|:--------:|:----------:|:----:|:------:|
| 線性搜 | 一個一個找 |   效率較低   |   $O(N)$     |
| 二分搜 | 一次找一半 |   序列要有單調性   |  $O(logN)$      |

--- 

## 實作
### 如何取中間項
- 一般來說（左＋右）除二就好

```cpp
int mid = (left + right)/2;
```

- 但是當 `left`,`right` 很大的時候，加起來可能就超過 `long long`範圍

```cpp
int mid = left + (right - left)/2;
```

### 怎麼二分搜
- 其實有很多種寫法，就是不同區間（以下示範$[L,R]$）

```cpp
int BinarySearch(int arr[],int size,int target){
    //定義左右邊界
    int left = 0;
    int right = size-1;
    
    while(left <= right){
        //中間項
        int mid = (left+right)/2;
        //開始找
        if(arr[mid] == target){
            //如果找到目標
            return mid;
        }else if(arr[mid] < target){
            //如果目標比中間項大就找右邊
            left = mid+1;
        }else{
            //如果目標比中間項小就找左邊
            right = mid-1;
        }
    }
    //找不到就回傳-1
    return -1;
}
```

### 倍增法(Binary Lifting)
```cpp
int JumpSearch(int arr[],int size,int target){
    //先檢查最左邊
    if(arr[0] >= target)return 0;
    //定義現在的位置為0
    int now = 0;
    //開始跳躍（每次跳一半）
    for(int jump = size/2;jump > 0;jump /= 2){
        //如果不會跳出去陣列
        //而且未發現目標（只要沒有大於等於就是沒發現）
        //就直接跳過去
        while(now+jump < size && arr[now+jump] < target){
            now += jump;
        }
    }
    //最後會停在目標的前一格（因為條件是小於）
    return now +1;
}
```

### 錯誤率最低又簡單的的 $(L,R)$ 寫法

> 歡迎參考以下影片：[二分查找为什么总是写错？](https://www.youtube.com/watch?v=JuDAqNyTG4g)

```cpp
int Search(int arr,int size,int target){
    int left = -1,right = size;
    while(left+1 != right){
        int mid = (left+right)/2;
        if(arr[mid] < target){
            left = mid;
        }else right = mid;
    }
    return right;
}
```

--- 

## 內建函式

| Function | Return |
|:--------:|:------:|
| `lower_bound(x)` | 第一個大於等於 $x$ 的元素的 $iterator$ |
| `upper_bound(x)` |  第一個大於 $x$ 的元素的 $iterator$ |

```cpp
int arr[8] = {1,5,3,7,8,3,5,2};
//要先排序!!!
sort(arr,arr+8);
// 1 2 3 3 5 5 7 8
int target = 4;
//要減掉初始位置才會是索引值
int index = lower_bound(arr,arr+8,target)-arr;
//找不到的話他的索引值就會是超出陣列的
if(index < 8)cout << index;
else cout << "Not in!";
```

- `vector` 也差不多

```cpp
vector<int>v = {1,5,3,7,8,3,5,2};
sort(v.begin(),v.end());
int target = 4;
int index = lower_bound(v.begin(),v.end(),target) - v.begin();
if(index < 8)cout << index;
else cout << "Not in!";
```

---

## 注意事項

- 當你確定要二分搜時，必須保證有 **單調性**（數字恆遞增或恆遞減）
- 二分搜最重要的是他的延伸技巧 - **對答案二分搜**


## ==練習題==

> ### [Zerojudge D732 - 二分搜尋法](https://zerojudge.tw/ShowProblem?problemid=d732)
> 
> 難度：*Template* $(2/10)$
> 
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> #define int long long 
> signed main(){
>     int n,m;cin>>n>>m;
>     int arr[n];
>     for(int i =0;i<n;i++){
>         cin >>arr[i];
>     }
>     sort(arr,arr+n);
>     for(int i =0;i<m;i++){
>         int a;cin>>a;
>         int l = -1,r = n;
>         while(l+1 != r){
>             int mid = (r-l)/2+l;
>             if(arr[mid] >= a){
>                 r = mid;
>             }else l = mid;
>         }
>         if(arr[r] == a)cout << r+1;
>         else cout << 0;
>         cout << '\n';
>     }
> }
> ```
> </details>

> ### [Zerojudge F607 - 切割費用](https://zerojudge.tw/ShowProblem?problemid=f607)
>
> 難度：*APCS* $(3/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
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


> ### [Zerojudge F581 - 圓環出口](https://zerojudge.tw/ShowProblem?problemid=f581)
>
> 難度：*APCS* $(4/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <iostream>
> #include <algorithm>
> using namespace std;
> int main(){
>     ios_base::sync_with_stdio(0);
>     cin.tie(0);
>     //input
>     int x,y;cin>>x>>y;
>     int p[2*x];
>     for(int i = 0;i<x;i++)cin>>p[i];
>     for(int i = 0;i<x;i++){
>         p[x+i] = p[i];
>     }
>     //prefix sum
>     for(int i = 1;i<2*x;i++){
>         p[i] += p[i-1];
>     }
>     int room = 0,q;
>     //each key
>     for(int i = 0;i<y;i++){
>         cin >> q;
>         if(room != 0) q += p[room-1];
>         room = lower_bound(p+room,p+2*x,q) - p;
>         room = (room+1)%x;
>     }
>     cout << room;
> }
> ```
> </details>




> ### [Codeforces 1955C - Inhabitant of the Deep Sea](https://codeforces.com/contest/1955/problem/C)
>
> 難度：$Easy$ $(4/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> #define FASTIO ios_base::sync_with_stdio(0);cin.tie(0)
> #define all(x) x.begin(),x.end()
> using namespace std;
> void solve(){
>     int n,k;
>     cin >> n >> k;
>     vector<int> arr(n),pre(n),suf(n);
>     for(int i=0;i<n;i++)cin>>arr[i];
>     pre[0] = arr[0];suf[n-1] = arr[n-1];
>     for(int i=1;i<n;i++)pre[i] = pre[i-1]+arr[i];
>     for(int i=n-2;i>=0;i--)suf[i]= suf[i+1] + arr[i];
>     reverse(all(suf));
>     int L = (k+1)/2,R = k/2;
>     int a = lower_bound(all(pre),L) - pre.begin();
>     int b = lower_bound(all(suf),R) - suf.begin();
>     b = n-b-1;
>     reverse(all(suf));
>     if(a > b){
>         cout << n << '\n';return;
>     }
>     int ans = 0;
>     if(a < b){
>         //check equal or greater
>         if(pre[a] > L)ans += a;
>         else ans += a+1;
>         if(suf[b] > R)ans += n-b-1;
>         else ans += n-b;
>     }else{//(a == b)
>         int total = pre[n-1];
>         if(total > k)ans = n-1;
>         else ans = n;
>     }
>     cout << ans << '\n';
> }
> int32_t main(){
>     FASTIO;
>     int test;
>     cin >> test;
>     while(test--){
>         solve();
>     }
> }
> ```
> </details>

