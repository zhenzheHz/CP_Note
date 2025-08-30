# 第四章、Lambda

---

> 可以把 function 寫在 `main` 裡面，~~真是方便~~
> *by Zhenzhe*

---

## 變量捕獲方式

### Capture by value
只引入數值，不影響變數本身
```cpp
int add_by_value(int x,int y) {
    x++, y++;
    return x+y;
}
```

### Capture by reference
會實際影響傳入的參數本身，也就是從傳入的變數直接去修改，會加上 `&` 符號
也可以理解為因為加上了 `&` 所以是直接從那個傳入的參數位址去調用
```cpp
int add_by_reference(int &x,int &y) {
    x++, y++;
    return x+y;
}
```

```cpp
int a = 1,b = 2;
cout << add_by_value(a,b); // 5
cout << a << ' ' << b; // 1 2
cout << add_by_reference(a,b); // 5
cout << a << ' ' << b; // 2 3
```

---

## Lambda 使用格式
> \[capture](parameters) -> return_type { body }

- 中括號內就放變量捕獲方式
- 小括號內放參數（跟普通 function 相同）
- 用 `->` 可以指定回傳類型
- 結尾要分號

```cpp
auto say_hello = []() {
    cout << "hello, world.";
    return;
};
```
捕獲外部變數 x,y，並且是 captue by value
```cpp
int x,y;
auto add = [x,y]() {
    x++, y++;
    return x+y;
};
```
捕獲外部變數 $x,y$，而 $x$ 要 `capture by reference`，而 $y$ 則是 `by value`
```cpp
int x = 1,y = 2;
auto add = [&x,y]() {
    x++, y++;
    return x+y;
};
cout << x << '  ' << y; // 1 2
cout << add(x,y); // 5
cout << x << ' ' << y; // 2 2
```
全局變量捕獲皆為 `by reference`（常用的寫法）
```cpp
auto add = [&](int x,int y) -> int{
    return x+y;
};
```
---

## <functional> 使用格式
來自 `<functional>` 中 `std::function` 的另一種寫法，~~感覺比較美觀~~
跟前面的 `lambda` 寫法基本上差不多，看以下範例對比一下就好
> 全域函式
```cpp
double divide(int x,int y) {
    if(y == 0) return -1;
    else return x / (double) y;
}
```
> Lambda
```cpp
auto divide = [&](int x,int y) -> double {
    if(y == 0) return -1;
    else return x / (double) y;
}
```
> Lambda by <functional>
```cpp
function<double(int,int)> divide = [&](int x,int y) {
    if(y == 0) return -1;
    else return x / (double) y;
};
```

---

## 注意事項
1. 寫在 `main` 裡面有機會 stack overflow
2. 這樣寫的好處是可以在 `main` 或其他 function 中定義新的 function
3. 最好用的時機就是在 `sort` 裡面放 `comparator` 的時候
```cpp
sort(arr.begin(),arr.end(),[&](int a,int b) {
    return a*a > b*b;
})
```
