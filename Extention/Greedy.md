# 第四章、反悔貪心

---

## 反悔的概念

對於原本的貪心演算法來說，求解過程中總是做出最好的選擇

也就是說，我們每一個步驟都是當前情況下的最優解，是沒有反悔操作的

但是一直朝著最優解走可能導致我們只停留在局部最優解而非全局最優解

**反悔貪心** 的概念就是在我們發現該解不是全局最優解的情況下回退操作採取其他策略去達到全局最佳解

就像是爬到某座山的山頂後發現有其他山比這座山高

於是我們下山到另一座山再爬

---

## 死線問題

> ### [USACO 09 OPEN - Work Scheduling](https://zerojudge.tw/ShowProblem?problemid=a567)
> 題目概述：有 $n$ 個工作，每個都有截止時間 $D_i$ 和完成利潤 $P_i$ ，求最多可獲得多少利潤（每個工作所花時間為 $1$）

- 想法一

最直觀的貪心方法是從利潤高的開始做，剩下的不做

但是顯然這個策略是錯的，以下是反例

有兩個工作 $A = (3,3), B = (3,1)$

若依照此策略，我們會選擇 $A$ 而捨棄 $B$

然而我們可以在時間為 $2$ 時做 $B$，時間為 $3$ 時做 $A$

- 想法二

我們也可以很直覺地想到，如果先完成截止日期靠前的工作會更好

所以就利用截止日期小到大排序，若相同則依據價值排序

能做就做，不能做就算了

這樣好像很有道理

但實際上有可能在後期有許多高價值工作卻因為前期做太多低價值工作而做不了

舉例來說，有 $4$ 個工作 

$$A=(1,2), B = (1,1), C = (2,5), D = (2,6)$$

若依照原本的貪心策略，順序為 $ABDC$

然後會做 $AD$ 這兩個工作，利潤為 $8$

但實際上，如果把做 $A$ 的時間拿去做 $C$

最後的結果會是 $11$ ，比 $8$ 還要好

為何會這樣呢？

因為到了後期，有很多高利潤的工作卻沒時間做

因為前面做了利潤很低的工作

那我們就會希望可以反悔

不要做那些低利潤工作轉而做高利潤工作

如果當前的最優解比原本的還要好

我們就放棄原本的改成現在的

反之就不管這份工作了

我們可以利用 `priority_queue` 來紀錄原本最佳解中利潤最少的工作

```cpp
struct Task {
    int deadline, value;
    bool operator<(const Task &cmp) const {
        return value > cmp.value;
    }
};
void solve(int n) {
    vector<Task> tasks(n);
    for(auto &[d,p] : tasks) cin >> d >> p;
    sort(tasks.begin(), tasks.end(), [&](const Task &a, const Task &b) {
        return a.deadline < b.deadline;
    });
    priority_queue<Task> pq;
    int ans = 0;
    for(auto &task : tasks) {
        // 還有時間可以做這個工作就直接做
        if(task.deadline > pq.size()) {
            ans += task.value;
            pq.push(task);
        }
        else {
            // 已經沒時間做了，考慮是否反悔
            if(task.value > pq.top().value) {
                ans -= pq.top().value;
                pq.pop();
                pq.push(task);
                ans += task.value;
            }
        }
    }
    cout << ans << '\n';
}
```

---

## 練習

