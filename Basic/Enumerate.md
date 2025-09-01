# 第二章、暴力枚舉

---

## 簡介

**枚舉** (Enumerate)，顧名思義，也叫做窮舉，即考慮所有可能的答案去取需要的結果

---

## 迴圈枚舉

利用 `for` 迴圈枚舉所有可能情況，是 **比賽中經常會出現的子題模式**，我們只需要考慮所有子樣本空間即可求出最後答案

> 複雜度 $O(n^k)$ $(k$ 層迴圈 $)$

先看個簡單的例題

> ### [Atcoder ABC389 B - tcaF](https://atcoder.jp/contests/abc389/tasks/abc389_b)
>
> 題目大意：給定一個值 $X$，求 $N$ 滿足 $N! = X$
>
> 限制：$2\leq X\leq 3\times 10^{18}$

很顯然，我們只要窮舉 $[1,20]$ 中所有可能直接比對就能知道答案

不過這題太簡單大家都會

> ### [Atcoder ABC393 B - A..B..C](https://atcoder.jp/contests/abc393/tasks/abc393_b)
>
> 題目大意：給定字串 $S$，問有幾個點對 $(i,j,k)$ 滿足 $i<j<k$ ， $2j = k + i$，並且 $(S_i,S_j,S_k) = (A,B,C)$
>
> 限制：$3\leq |S|\leq 100$

因為 $S$ 的長度很小，因此直接枚舉所有可能即可

不過注意到因為 $2j = k + i$，因此我們只需要枚舉 $i,j$ 兩個可能， $k$ 就會順便被決定好

```cpp
int ans = 0;
for(int i=0;i<s.size();i++) {
    for(int j=i+1;j<s.size();j++) {
        int k = 2 * j - i;
        if(s[i] == 'A' && s[j] == 'B' && s[k] == 'C') {
            ans += 1;
        }
    }
}
```

這樣複雜度 $O(|S|^2)$

---

## Bitmask 枚舉子集


