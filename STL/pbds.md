# 第九章、pbds
---

## 平板電視
全名 *Policy-Based Data Structures*

`pb_ds` 俗稱 **平板電視**，其中包含很多種資料結構，不過真正在比賽中有機會用到的只有 `tree`

基本上可以把它當成跑很快的 `set`

當然他確實有其他像是 優先隊列、字點樹等結構

---

## 使用方法

### 標頭檔、命名空間

> 溫馨提示：不是打了這個就不用 `#include <bits/stdc++.h>` 和 `std`

```cpp
#include <bits/extc++.h>
using __gnu_pbds;
```

### 宣告方式

> 問就是背起來就對了
```cpp
tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> rbt;
```

### pbds 常用方法與時間複雜度
- `set` 有的他都有，主要是多了最下面兩個好用的功能
| Method | Function | Time Complexity |
|:------:|:---------|:----------------|
| `insert(x)` | 插入元素 $$x$$ | $$O(\log n)$$ |
| `erase(x)` | 刪除元素 $$x$$（若存在） | $$O(\log n)$$ |
| `find(x)` | 查找元素 $$x$$，回傳 iterator | $$O(\log n)$$ |
| `lower_bound(x)` | 回傳第一個 $$\geq x$$ 的 iterator | $$O(\log n)$$ |
| `upper_bound(x)` | 回傳第一個 $$> x$$ 的 iterator | $$O(\log n)$$ |
| `order_of_key(x)` | 回傳集合中小於 $$x$$ 的元素個數（排名功能） | $$O(\log n)$$ |
| `find_by_order(k)` | 回傳集合中第 $$k$$ 小的元素（0-indexed） | $$O(\log n)$$ |

---

## 練習

> ###[Zerojudge D794](https://zerojudge.tw/ShowProblem?problemid=d794)
>
> 難度：*Medium* $(5/10)$
>
> <details>
>   <summary> 參考解法 1 </summary>
> 
> ```cpp
> #include<bits/stdc++.h>
> #include<bits/extc++.h>
> using namespace std;
> using namespace __gnu_pbds;
> int main() {
>     ios::sync_with_stdio(false);
>     cin.tie(0);
>     int n;
>     while(cin>>n) {
>         tree<pair<int,int>,null_type,greater<pair<int,int>>,rb_tree_tag,tree_order_statistics_node_update>t;
>         int tt = 1;
>         while(n--) {
>             int tmp;cin>>tmp;
>             t.insert({tmp,tt++});
>             cout<<t.order_of_key(*t.upper_bound({tmp + 1,0})) + 1<<"\n";
>         }
>     }
>     return 0;
> }
> ```
> </details>
>
> <details>
>   <summary> 參考解法 2 </summary>
> 
> 此解乃為線段樹
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> #define m ((l+r)>>1)
> using namespace std;
> struct Segment_Tree {
>     size_t sz;
>     vector<int> sg;
>     void init(int k) {
>         sz = k;
>         sg.resize((k+1)<<2,0);
>     }
>     void update(int l,int r,int p,int idx,int x) {
>         if(l == r) return sg[p] += x,void();
>         if(idx <= m) update(l,m,p<<1,idx,x);
>         else update(m+1,r,p<<1|1,idx,x);
>         sg[p] = sg[p<<1] + sg[p<<1|1];
>     }
>     int query(int l,int r,int p,int ql,int qr) {
>         if(ql<=l && r<=qr) return sg[p];
>         if(r < ql || qr < l) return 0;
>         int ans = 0;
>         if(ql <= m) ans += query(l,m,p<<1,ql,qr);
>         if(m < qr) ans += query(m+1,r,p<<1|1,ql,qr);
>         return ans;
>     }
> };
> void discretization(vector<int> &origin, size_t len) {
>     vector<int> copy(origin.begin(),origin.end());
>     sort(copy.begin(), copy.end());
>     copy.resize(unique(copy.begin(),copy.end()) - copy.begin());
>     for(int i=0;i<(int)len;i++) {
>         origin[i] = lower_bound(copy.begin(),copy.end(),origin[i]) - copy.begin() + 1;
>     }
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int n;
>     while(cin >> n) {
>         Segment_Tree Tree;
>         Tree.init(n);
>         vector<int> arr(n);
>         for(int i=0;i<n;i++) cin >> arr[i];
>         discretization(arr, n);
>         for(int i=0;i<n;i++) {
>             Tree.update(1,n,1,arr[i],1);
>             cout << Tree.query(1,n,1,arr[i],n) << '\n';
>         }
>     }
> }
> ```
> </details>
