# 第四章、遞迴
---
## 什麼是遞迴?
> 數學應該學過 **遞迴數列**
> 
> 遞迴如同數學上遞迴數列的概念
> 1. 要有終止條件(數列第1項)
> 2. 會有與前一項的關係(自我定義 / 調用)

- 狹義的遞迴：在函式中呼叫自己的過程
- 廣義的遞迴：分治的過程也算
```cpp
void say_hi(){
    print("HI");
    say_hi();
}
```
> 遞迴的定義：
>
> 在數學與計算機科學中，**遞迴（Recursion）**是指在函數的定義中使用函數自身的方法。也就是說，一個函數在定義過程中會呼叫自己來完成部分計算。
> 在計算機科學領域，遞迴除了上述概念外，還特別指一種將問題不斷分解為相似子問題來解決整個問題的方法。透過將大問題拆解成結構相似、規模更小的子問題，並在子問題中再次使用相同的解法，最終合併子問題的結果，就能得到原問題的解答。
> 簡單來說，遞迴是一種自我參照的解題策略，既能簡化程式邏輯，也能有效處理許多自然分解的問題，例如階乘計算、費氏數列、樹或圖的遍歷等。

---

## 遞迴有什麼用？
- 可以用來解決複雜的問題
### 階乘
$$n! = n\times(n-1)!,\ and\ 0!=1$$
```cpp
int factorial(int n){
    if(n == 0)return 1;
    else return factorial(n-1)*n;
}
```
### 費氏數列
$$F_1 = F_2=1,\ F_n = F_{n-1}+F_{n-2}$$
```cpp
int F(int n){
    if(n == 1||n == 2){
        return 1;
    }
    return F(n-1)+F(n-2);
}
```

---

## 遞迴的理解
![y](https://hackmd.io/_uploads/Sk4c30gqge.png)
遞迴的基本思想是，某個函數直接或間接地呼叫自身，藉此將原問題的求解轉換為許多性質相同但規模更小的子問題。在實際求解過程中，我們只需要關注如何將原問題拆分成符合條件的子問題，而不必過分關心這些子問題是如何被解決的。

這種方法的核心在於分而治之：透過不斷將問題縮小，最終將問題簡化到容易處理的基本情況，再將子問題的結果組合回來，得到原問題的答案。遞迴既能讓程式邏輯簡潔，也能有效處理許多自然分解的問題，例如階乘計算、費氏數列、樹或圖的遍歷等。

> 舉例來說：你今年幾歲？比去年多一歲
>
> 遞迴又名TRTC，是The Recursive TRTC Creation的縮寫，裡面的TRTC是The Recursive TRTC Creation的縮寫，裡面的...

---

## 河內塔問題
### 規則：[應該都知道才對](https://www.youtube.com/watch?v=lDZUP-S0_Zw)

### 最佳解決方法

> 假設有 $N$ 片
> 1. 先把 $1$ ~ $N-1$片移出來
> 2. 把第$N$片移到終點
> 3. 重複以上步驟

### 如何思考
> **Q：如何把大象放進冰箱？**
> 1. **把冰箱打開**
> 2. **把大象放進去**
> 3. **把冰箱關上**

- 不要去管怎麼把大象放進去，你只管把他放進去就好
> **思考的轉換**

- 把遞迴操作當成<font color="#FF0000">**超級操作**</font>，不要管他怎麼做到（把大象塞進冰箱）
- 可以直接做到的事當成<font color="#00A600">**普通操作**</font>（開冰箱,關冰箱）
- 超級操作就是整疊拿起來移動
- 普通操作就是只拿最上面的

```text
function 超級移動(n層,起始柱,中轉柱,目標柱):
	if n = 1:
		用基本操作把第一片移到目標柱
		return
	else:
		用超級移動把n-1都從起始柱移到中轉柱上
		用基本操作把第n片從起始柱移到目標柱上
		用超級移動把n-1都從中轉柱移到目標柱上
```

> ### **追加問題：如何把長頸鹿放進冰箱**
> <details>
> <summary> 大家都懂的爛梗 </summary>
>
> 1. 打開冰箱
> 2. 把大象拿出來
> 3. 把長頸鹿放進去
> 4. 把冰箱關上
> </details>

### 結論
- 不要去過度思考地迴的過程，就當作遞迴會幫你處理掉大問題
- 把小問題寫好（包含終止條件），只考慮最上層

### 虛擬碼與實作
```terminal
function HANOI(n,source,auxiliary,target):
	if n = 1:
		MOVE_ONE_DISK(source,target)
		return
	else:
		HANOI(n-1,source,target,auxiliary)
		MOVE_ONE_DISK(source,target)
		HANOI(n-1,auxiliary,source,target)
```
- 實作參考 Code
```cpp
#include <iostream>
using namespace std;

void normal_MOVE(int disk,char start,char end){
	//因為移完就不用處理了，所以其實普通操作就是輸出
    cout <<"Move ring "<<disk;
    cout <<" from "<<start<<" to "<< end;
    cout << "\n";
}

void super_MOVE(int n,char A,char B,char C){
    if(n == 1){
        normal_MOVE(n,A,C);
        return;
    }
    super_MOVE(n-1,A,C,B);
    normal_MOVE(n,A,C);
    super_MOVE(n-1,B,A,C);
}

int main(){
    int n;
    while(cin>>n){
        super_MOVE(n,'A','B','C');
        cout << '\n';
    }

```

---

> ### [Zerojudge E357 - 遞迴語法練習](https://zerojudge.tw/ShowProblem?problemid=e357)
>
> 難度：*Easy* $(1/10)$
>
> <details>
>   <summary> 參考解答 </summary>
>
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> 
> int f(int x){
>     if(x == 1)return 1;
>     if(x%2 == 0) return f(x/2);
>     return f(x-1)+f(x+1);
> }
> int main(){
>     int i;cin >> i;
>     cout << f(i);
> }
> ```
> </details>

> ### [Zerojudge A227 - 河內塔](https://zerojudge.tw/ShowProblem?problemid=a227)
>
> 難度：*Easy* $(經典問題)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> 
> ```cpp
> #include <iostream>
> using namespace std;
> 
> void move(int ring,char start,char end){
>     cout <<"Move ring "<<ring;
>     cout <<" from "<<start<<" to "<< end;
>     cout << "\n";
> }
> 
> void s_move(int n,char A,char B,char C){
>     //(層數,起始柱,中轉柱,終點柱)
>     if(n == 1){
>         move(n,A,C);
>         return;
>     }
>     //把n-1移動到B
>     s_move(n-1,A,C,B);
>     //把第n片（最下面）移動到A
>     move(n,A,C);
>     //把n-1移動到C
>     s_move(n-1,B,A,C);
> }
> 
> int main(){
>     int n;
>     while(cin>>n){
>         s_move(n,'A','B','C');
>         cout << '\n';
>     }
> }
> ```
> </details>

> ### [Zerojudge I646 - 全組合](https://zerojudge.tw/ShowProblem?problemid=i646)
>
> 難度：*Easy* $(3/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> int number[15] ,n,m;
> void combination(int layer){
>     if(layer == m){
>         for(int i = 1;i<=m;i++){
>             cout << (char)('a' + number[i]);
>         }
>         cout << '\n';
>         return;
>     }
>     for(int i = 0;i<n;i++){
>         if(number[layer] >= i)continue;
>         number[layer+1] = i;
>         combination(layer+1);
>     }
> }
> 
> int main(){
>     number[0]=-1;
>     while(cin>>n>>m){
>         if(!n and !m)break;
>         combination(0);
>     }
> }
> ```
> </details>

> ### [Zerojudge F640 - 合成函數](https://zerojudge.tw/ShowProblem?problemid=f640)
>
> 難度：*APCS* $(4/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> - 只要目前那項是函數不是數字就往後找參數
> 
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> 
> int input(){
>     string a;cin>>a;
>     if(a[0] == 'f'){
>         //function 'f' has one parameter
>         int x = input();
>         return 2*x-3;
>     }else if(a[0] == 'g'){
>         //function 'g' has two parameters
>         int x = input();
>         int y = input();
>         return 2*x+y-7;
>     }else if(a[0] == 'h'){
>         //function 'h' has three parameters
>         int x = input();
>         int y = input();
>         int z = input();
>         return 3*x-2*y+z;
>     }else{
>         //while input is a number
>         return stoi(a);
>     }
> }
> 
> int main(){
>     cout << input();
> }
> 
> ```
> </details>

> ### [Zerojudge F638 - 支點切割](https://zerojudge.tw/ShowProblem?problemid=f638)
>
> 難度：*APCS* $(5/10)$
>
> <details>
>   <summary> 參考解法 </summary>
> 
> - 此題必須用前綴和優化才能過
> 
> ```cpp
> #include <iostream>
> #include <cmath>
> #include <climits>
> using namespace std;
> #define int long long
> int n,p[50005],k,prefix[50005],suffix[50005];
> 
> inline int cut(int left,int right,int times){
>     if(right - left < 2)return 0;
>     if(times >= k)return 0;
>     //prefix sum
>     int stack = 0;
>     prefix[left] = 0;
>     for(int i =left+1;i<=right;i++){
>         stack += p[i-1];
>         prefix[i] = prefix[i-1]+stack;
>     }
>     //suffix sum
>     stack = 0;
>     suffix[right] = 0;
>     for(int i = right-1;i>=left;i--){
>         stack += p[i+1];
>         suffix[i] = suffix[i+1]+stack;
>     }
>     //find the best
>     int best = LONG_MAX,ans;
>     for(int i = left+1;i<right;i++){
>         int cost = abs(suffix[i]-prefix[i]);
>         if(cost < best){
>             best = cost;
>             ans = i;
>         }
>     }
>     //return
>     return p[ans] + cut(left,ans-1,times+1) + cut(ans+1,right,times+1);
> }
> 
> signed main(){
>     ios_base::sync_with_stdio(0);
>     cin.tie(0);
>     cin>>n>>k;
>     for(int i = 1;i<=n;i++){
>         cin >> p[i];
>     }
>     cout << cut(1,n,0);
> }
> ```
> </details>

---

## Reference
- 以下影片超推薦觀看
- [滑水影片](https://www.youtube.com/watch?v=KlY-A8gk5sA)
- [三件套-1](https://www.youtube.com/watch?v=q4xLypEFToQ)
- [三件套-2](https://www.youtube.com/watch?v=kEWQj2Hb8kc&t=469s)
- [三件套-3](https://www.youtube.com/watch?v=e9fEQDQ_JpQ)
- [遞迴只會費氏數列?](https://www.youtube.com/playlist?list=PLd95SS_-Um6-JqY3HlJZAdBScnR0INWxO)
