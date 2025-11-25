# 第一章、對答案二分搜
---

## Introduction

我們常遇到這類問題：

* 找到「最小的 X 使條件成立」
* 找到「最大的 X 使條件仍成立」
* 最小值最大化問題 / 最大值最小化問題

如果我們可以判斷「給定一個候選答案 $k$，它是否可行（YES/NO）」，那麼整個問題就能透過二分搜尋快速解決。這就是 **答案二分搜尋** 的核心概念

---

## 一、為什麼可以對「答案」二分搜？

二分搜尋的基本前提是：**答案的可行性必須具備單調性（Monotonicity）。** 也就是說，答案空間呈現以下其中一種結構（`1` 代表可行，`0` 代表不可行）

* **00000000001111111111111111**
* **11111100000000000000000000**

一旦找到這樣的性質，我們就能把答案視為「排序後的整數」，進而對答案做二分。比如說當 $k$ 可行時，$k+1,k+2,...,k+n$ 必定可行這種形式

舉例來說：當我們知道雞蛋從 $3$ 樓掉下來一定會碎，那麼我們可以知道從 $4,5,6,7,...,3+n$ 樓掉下來都會碎

---

## 二、解題流程

答案二分具有固定的思考模板，大致寫法如下，二分搜的寫法可以參考[](// 左開右開寫法，可參考[二分搜尋法](https://zhenzhehz.github.io/CP_Note/#Basic/Binary_search.md))

- **(1) 定義答案的範圍** $[L,R]$
- **(2) 設計一個 check function**
- **(3) 利用二分搜尋得到答案**（你要會寫二分搜）

```cpp
int l,r;
while(l+1 != r) {
  int mid = (l+r) / 2;
  // 視情況調整左右邊界
  if(check(mid)) r = mid;
  else l = mid;
}
// 答案是 l 或 r 其中一個（看題目）
```

如果 **check的時間複雜度是** $O(f(n))$，整體的時間複雜度會是 $O(f(n)\times (R-L))$

---

## 三、典型使用場景

對答案二分搜算是一個間單好懂的小技巧，，APCS有機會考，如果出了要好好把握，而在一般正式的比賽中，通常困難的點是 `cheek` 很難寫或者單調性不容易發現

### **1.滿足條件的最大值 / 最小值**

> ### [2024.1月 TOI練習賽潛力組 - 遊戲升等](https://zerojudge.tw/ShowProblem?problemid=f815)

這題就是很典型的找符合條件的最大最小值問題，可以發現如果可以升到 $x$ 等，那麼升到 $x-1,x-2,x-3$ 必定也可以，因此必定存在一個臨界值（最大值）是符合條件的，這就是我們要的單調性，因此我們可以對答案（等級）二分搜

```cpp
#include <bits/stdc++.h>
#define int int64_t
using namespace std;
int n,m;
bool check(vector<int> &s,int level){
    int cost = 0;
    for(int &i : s){
        if(i < level){
            cost += (level-i)*(level-i);
        }
    }
    return (cost<=m);
}
int32_t main(){
    ios_base::sync_with_stdio(0);
    cin.tie(0),cout.tie(0);
    cin>>n>>m;
    vector<int> a(n);
    for(int &i : a)cin >> i;
    int l = -1,r = 1e7+1;
    while(l+1 != r){
        int m = (l+r)>>1;
        if(check(a,m))l = m;
        else r = m;
    }
    cout << l;
}
```

### **2. 最大值最小化 ／ 最小值最大化問題**

> ### [CSES - Array Division](https://cses.fi/problemset/task/1085)

我們要最小化「最大子陣列和」。假設我們猜一個值 **M**，問：「能否把陣列劃分為 $\leq k$ 段，使得每一段的和 $\leq M$？」如果可以，那麼我們定義「最大子陣列和 $\leq M$」是可行的。反過來，如果不可以，那麼「最大子陣列和 $\leq M$」是不可行的。由於 $M$ 越大，限制就越鬆；$M$ 越小，限制越嚴。這樣就具備單調性：若某 $M$ 可行，那所有 $> M$ 的也可行；若某 $M$ 不可行，那所有 $< M$ 的也不可行。於是我們可以在答案空間上做二分搜尋。

- 設定答案範圍

最小可能值：因為至少一段會含有最大的一個元素，所以最小 $M$ 至少是 $max(x_i)$。
最大可能值：當 $k=1$ 時，整個陣列是一段，其和為 $sum(x_i)$。所以最大 $M$ 最多是 $sum(x_i)$。

因此範圍是 $[max(x_i),\sum_{i=0}^{n-1} x_i]$

- 定義 check(M)：能否在每段和 $\leq M$ 的限制下，使用 $\leq k$ 段把陣列劃分？

方法：使用 `greedy` + `sliding window` 即可

- 用二分搜尋找最小可行 $M$

- 時間與空間複雜度分析

每次 check 需要 $O(n)$ 時間，$log(R-L) ≲ log(n·10^9) \leq 40$，因此整體複雜度為 $O(nlog(R-L))$

```cpp
#include <bits/stdc++.h>
#define int int64_t
#define fastio ios_base::sync_with_stdio(0);cin.tie(0),cout.tie(0)
#define endl '\n'
using namespace std;
int n,k;
int check(vector<int>&arr,int x){
    int cnt = 0;
    int save = 0;
    for(int i=0;i<n;i++){
        if(save + arr[i] > x && save){
            save = arr[i];
            cnt += 1;
        }else save += arr[i];
    }
    if(save)cnt++;
    return cnt;
}
int32_t main(){
    fastio;
    cin >> n >> k;
    vector<int> arr(n);
    int mx = -1,sum = 0;
    for(int i=0;i<n;i++){
        cin >> arr[i];
        mx = max(arr[i],mx);
        sum += arr[i];
    }
    int l = mx-1,r = sum+1;
    while(l+1!=r){
        int m = (l+r)>>1;
        if(check(arr,m)<=k)r = m;
        else l = m;
    }
    cout << r;
}
```



---

## 練習

> ### [APCS 2017.03 第四題 - 基地台](https://zerojudge.tw/ShowProblem?problemid=c575)
>
> 考點：二分搜＋貪心演算法
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> #define int long long 
> int n,m;
> int arr[5000003];
> bool check(int r){
>     int c = 1,end = arr[0]+r;
>     for(int i = 0;i<n;i++){
>         if(arr[i] <= end)continue;
>         end = arr[i]+r;
>         c++;
>     }
>     return c <= m;
> }
> signed main(){
>     cin>>n >> m;
>     for(int i = 0;i<n;i++){
>         cin >> arr[i];
>     }
>     sort(arr,arr+n);
>     int left = 0,right =1e9+1;
>     while(left +1 != right){
>         int mid = (right-left)/2 + left;
>         if(check(mid)){
>             right = mid;
>         }else left = mid;
>     }
>     cout << right;
> }
> ```
> </details>

> ### [APCS 2022.01 第四題 - 牆上海報](https://zerojudge.tw/ShowProblem?problemid=h084)
>
> 考點：二分搜＋貪心演算法
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> int n,q;
> vector<int> height,paste;
> bool check(int h){
>     int sum = 0,index = 0;
>     for(int i=0;i<n;i++){
>         if(index == q)return true;
>         if(height[i] >= h)sum += 1;
>         else sum = 0;
>         // cerr << sum << '\n';
>         if(sum >= paste[index]){
>             index += 1;
>             sum = 0;
>         }
>     }
>     // cerr << index << '\n';
>     return (index == q);
> }
> int32_t main(){
>     ios_base::sync_with_stdio(0);
>     cin.tie(0);
>     cin>>n>>q;
>     height.resize(n);paste.resize(q);
>     for(int &hs : height)cin >> hs;
>     for(int &i : paste)cin >> i;
>     int l=0,r=1e9+1;
>     while(l+1 != r){
>         int m = (l+r)>>1;
>         // cerr << "m = " << m << '\n';
>         if(check(m))l = m;
>         else r = m;
>     }
>     cout << l;
> }
> ```
> </details>

> ### [APCS 2022.10 第四題 - 蓋步道](https://zerojudge.tw/ShowProblem?problemid=j125)
>
> 考點：二分搜＋BFS＋最短路
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> #define int long long
> #define inside(i) (i>=0&&i<n)
> int v[305][305],n,xdir[4] = {-1,1,0,0},ydir[4] = {0,0,-1,1};
> int bfs(int h){
>     queue<tuple<int,int,int>>q;
>     int vis[n][n];
>     for(int i = 0;i<n;i++)
>         for(int j = 0;j<n;j++)
>             vis[i][j] = 0;
>     q.push({0,0,0});
>     vis[0][0] = 1;
>     while(!q.empty()){
>         int i = get<0>(q.front());
>         int j = get<1>(q.front());
>         int step = get<2>(q.front());
>         q.pop();
>         if(i == n-1 and j == n-1)return step;
>         //check route
>         for(int c = 0;c<4;c++){
>             int x = i + xdir[c];
>             int y = j + ydir[c];
>             if(inside(x) and inside(y)){
>                 if(!vis[x][y] and abs(v[i][j] - v[x][y])<=h){
>                     vis[x][y] = 1;
>                     q.push({x,y,step+1});
>                 }
>             }
>         }
>     }
>     return -1;
> }
> signed main(){
>     ios_base::sync_with_stdio(0);
>     cin.tie(0),cout.tie(0);
>     cin>>n;
>     for(int i = 0;i<n;i++)
>         for(int j = 0;j<n;j++)
>             cin>>v[i][j];
>     int l = -1,r = 1e6+1;
>     while(l+1 != r){
>         int mid = (r-l)/2+l;
>         if(bfs(mid) != -1)r = mid;
>         else l = mid;
>     }
>     cout << r << '\n' << bfs(r);
> }
> ```
> </details>

> ### [CITRC Judge - 控分大師](https://judge.citrc.tw/contest/19/problem/8)
>
> 考點：貪心演算法＋二分搜
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> int n,m,q;
> vector<int>a;
> bool check(int k){
>     int TIMES = 0;
>     for(int score : a){
>         int x = (score+k)/q;
>         if((score+k)%q != 0)x += 1;
>         TIMES += (k>=x);
>     }
>     return (TIMES<=m);
> }
> int32_t main(){
>     ios_base::sync_with_stdio(false);cin.tie(NULL);
>     cin >> n >> m >> q;
>     q += 1;
>     for(int i = 0;i<n;i++){
>         int x;cin>>x;
>         a.push_back(x);
>     }
>     int l = -1,r = 1e9+1;
>     while(l+1!=r){
>         int m = (l+r)>>1;
>         if(check(m))l = m;
>         else r = m;
>     }
>     cout << l;
> }
> ```
> </details>

> ### [Codeforces Div.3 - Problem D.Jumping Through Segments](https://codeforces.com/contest/1907/problem/D)
>
> 考點：區間問題＋二分搜
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
> template<typename... T>
> void trace(T&&... args) { ((cerr << args << " "), ...);cerr<<'\n';}
> void solve(){
>     int n;cin>>n;
>     vector<pair<int,int>> seg(n);
>     for(auto &i : seg){
>         cin >> i.first >> i.second;
>     }
>     int l = -1,r = 1e9+1;
>     function<bool(int)>check=[&](int k)->bool{
>         int L = 0,R = 0;
>         for(auto &[l,r] : seg){
>             L = max(l,L-k);
>             R = min(r,R+k);
>             if(R<L)return false;
>             //if(debug)trace(L,R);
>         }
>         return true;
>     };
>     while(l+1!=r){
>         int m = (l+r)>>1;
>         if(check(m))r = m;
>         else l = m;
>     }
>     cout << r << '\n';
>     //trace(r,"-----");
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


> ### [Codeforces Div.4 - Problem F.Final Boss](https://codeforces.com/contest/1985/problem/F)
>
> 考點：貪心演算法＋二分搜
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
>     int h,n;
>     cin>>h>>n;
>     int max_cd = 0;
>     vector<pair<int,int>> skills(n);
>     for(int i=0;i<n;i++)cin>>skills[i].first;
>     for(int i=0;i<n;i++){
>         cin>>skills[i].second;
>         max_cd = max(max_cd,skills[i].second);
>     }
>     //check function
>     auto defeat = [&](int turns){
>         int total_damage = 0;
>         for(auto [damage,cd] : skills){
>             total_damage += ((turns-1)/cd + 1)*damage;
>             if(total_damage>=h)return true;
>         }
>         return false;
>     };
>     //binary search
>     int l=0,r=max_cd*(h+10);
>     while(l+1 != r){
>         int m = (l+r)>>1;
>         if(defeat(m))r = m;
>         else l = m;
>     }
>     cout << r << '\n';
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

> ### [2018 北市賽 - 勇者冒險(Adventure)](https://tioj.ck.tp.edu.tw/problems/2180)
>
> 考點：圖論＋二分搜
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> #define x first
> #define y second
> using namespace std;
> static constexpr int MAXN = 1005;
> const pair<int,int> dxy[] = {{0,1},{0,-1},{1,0},{-1,0}};
> int R,C,N,sx,sy,ex,ey,grid[MAXN][MAXN];
> bool vis[MAXN][MAXN];
> bool inRange(int i,int j)  {
>     return (i >=0 && i < R && j >= 0 && j < C);
> }
> bool check(int level) {
>     memset(vis, 0, sizeof(vis));
>     vis[sx][sy] = 1;
>     queue<pair<int,int>> q;
>     q.push({sx,sy});
>     while(!q.empty()) {
>         auto cur = q.front();
>         q.pop();
>         if(cur.x == ex && cur.y == ey) return 1;
>         for(auto [dx,dy] : dxy) {
>             int tx = cur.x + dx, ty = cur.y + dy;
>             if(!inRange(tx,ty) || grid[tx][ty] == -1) continue;
>             if(vis[tx][ty] || grid[tx][ty] > level) continue;
>             q.push({tx,ty});
>             vis[tx][ty] = 1;
>         }
>     }
>     return 0;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     cin >> R >> C;
>     cin >> sx >> sy >> ex >> ey;
>     cin >> N;
>     for(int i = 0; i < R; i++) for(int j = 0; j < C; j++) grid[i][j] = -1;
>     grid[sx][sy] = grid[ex][ey] = 0;
>     int l = -1, r = 0;
>     for(int i = 0; i < N; i++) {
>         int x,y,lv;
>         cin >> x >> y >> lv;
>         grid[x][y] = lv;
>         r = max(r, lv+1);
>     }
>     while(l+1 != r) {
>         int mid = (l+r) >> 1;
>         if(check(mid)) r = mid;
>         else l = mid;
>     }
>     return cout << r << '\n', 0;
> }
> ```
> </details>

> ### [2023 TOI 初選 第二題 - 裁員風暴(storm)](https://tioj.ck.tp.edu.tw/problems/2331)
>
> 考點：雙指針＋二分搜
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> // Problem : https://tioj.ck.tp.edu.tw/problems/2331
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> int check(vector<int> &w, int val) {
>     int ans = 0;
>     for(int l=0,r=w.size()-1;l < w.size()&&l<=r;l++) {
>         while(w[l] + w[r] > val && l<=r) --r;
>         ans += r-l+1;
>     }
>     return ans;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int n,k;
>     cin >> n >> k;
>     vector<int> w(n);
>     for(int &i : w) cin >> i;
>     sort(w.begin(),w.end());
>     k = n*(n+1)/2 - k + 1;
>     int l = 2*w.front()-1, r = 2*w.back()+1;
>     while(l+1!=r) {
>         int m = (l+r)>>1;
>         if(check(w,m) < k) l = m;
>         else r = m;
>     }
>     if(r & 1) cout << r << '\n' << 2;
>     else cout << r/2 << '\n' << 1 << '\n';
> }
> ```
> </details>

> ### [2021 全國資訊學科能力競賽 Problem D. 汽車不再繞圈圈](https://tioj.ck.tp.edu.tw/problems/2254)
>
> 考點：圖論＋二分搜＋DAG＋構造
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> // Problem : https://tioj.ck.tp.edu.tw/problems/2254
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> static constexpr int MAXN = 1e5+5;
> struct Edge {int u,v,w,id;};
> int n,m;
> vector<Edge> lst;
> vector<int> g[MAXN], change;
> bool check(int thr) {
>     for(int i=1;i<=n;i++) g[i].clear();
>     vector<int> degree(MAXN,0), pos(MAXN);
>     for(auto &[u,v,w,id] : lst) {
>         if(w > thr) {
>             g[u].push_back(v);
>             degree[v]++;
>         }
>     }
>     queue<int> q;
>     int cur_id = 0;
>     for(int i=1;i<=n;i++) {
>         if(degree[i] == 0) {
>             q.push(i);
>             pos[i] = cur_id++;
>         }
>     }
>     while(!q.empty()) {
>         auto cur = q.front();
>         q.pop();
>         for(auto nxt : g[cur]) {
>             if(--degree[nxt] == 0) {
>                 q.push(nxt);
>                 pos[nxt] = cur_id++;
>             }
>         }
>     }
>     if(cur_id != n) return false;
>     change.clear();
>     for(auto &[u,v,w,id] : lst) {
>         if(w <= thr && pos[u] > pos[v]) {
>             change.push_back(id);
>         }
>     }
>     return true;
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     cin >> n >> m;
>     vector<int> c = {0};
>     for(int i=0;i<m;i++) {
>         int a,b,x;
>         cin >> a >> b >> x;
>         c.push_back(x);
>         lst.push_back({a,b,x,i+1});
>     }
>     sort(c.begin(), c.end());
>     c.resize(unique(c.begin(),c.end()) - c.begin());
>     int l = -1, r = c.size();
>     while(l+1 != r) {
>         int mid = (l+r)>>1;
>         if(check(c[mid])) r = mid;
>         else l = mid;
>     }
>     check(c[r]);
>     cout << c[r] << ' ' << change.size() << '\n';
>     for(auto id : change) cout << id << '\n';
>     return 0;
> }
> ```
> </details>

> ### [Codeforces Div.2 - Problem C.  Tree Cutting](https://codeforces.com/contest/1946/problem/C)
>
> 考點：貪心演算法＋樹論＋二分搜
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> #define int int64_t
> using namespace std;
> void solve() {
>     int n,k;
>     cin >> n >> k;
>     vector<int> g[n+1];
>     for(int i=1;i<n;i++) {
>         int a,b;
>         cin >> a >> b;
>         g[a].push_back(b);
>         g[b].push_back(a);
>     }
>     function<bool(int)> check = [&](int x) {
>         int total_cut = 0;
>         function<int(int,int)> dfs = [&](int cur, int from) {
>             int SIZE = 1;
>             for(auto nxt : g[cur]) {
>                 if(nxt == from) continue;
>                 SIZE += dfs(nxt, cur);
>             }
>             if(SIZE >= x) total_cut += 1, SIZE = 0;
>             return SIZE;
>         };
>         int root = dfs(1,-1);
>         return total_cut > k || (total_cut == k && root >= x);
>     };
>     int l = -1, r = n+1;
>     while(l+1!=r) {
>         int m = (l+r)>>1;
>         if(check(m)) l = m;
>         else r = m;
>     }
>     return cout<<l<<'\n',void();
> }
> signed main() {
>     cin.tie(nullptr)->ios_base::sync_with_stdio(0);
>     int t;
>     cin >> t;
>     while(t--) solve();
> }
> ```
> </details>
