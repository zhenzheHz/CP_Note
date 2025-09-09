# 第一章、輸入與輸出 (Input / Output) 

---

## Stream 的概念

> *Stream* (n.) 流動、流  
> A continuous of things or people  
> 就如同他的名字一樣，一個 stream 會有流進來的地方、流出去的地方和流向

可以想像成一個可以儲存資料的管子

- istream : 輸入流，輸入的資料會先進入輸入流，再進到其他地方
- ostream : 輸出流，輸出的內容會先進入輸出流，再流到其他地方
- iostram : 雙向的，同時有 istream, ostream

---

## 標準流
### 流向
用 `<<` 和 `>>` 來表示往左或往右

### 標準輸入流 (cin)
一個儲存輸入資料的地方，使用時從 istream 流到變數，故為 istream >> variable
```cpp
cin >> input;
```

### 標準錯誤流 (cout)
一個儲存輸出資料的地方，使用時從變數流到 ostream ，故為 ostream << variable
```cpp
cout << output;
```

### 標準錯誤流 (cerr)
用來儲存錯誤訊息的地方，使用時會從 cerr 讀取錯誤訊息(同cout用法)
```cpp
double divide(){
    if(b == 0) {
        cerr << "Error : Divide by zero!";
    }
    else return a/b;
}
```

---

## 緩衝區 (buffer)
- 緩衝區：流都有緩衝區，資料會先暫時停在那裡，之後才會流走
- cin 讀取到的資料會先儲存在緩衝區等你讀取
- cout 則是先儲存在緩衝區，等到flush才會真正輸出
- cerr 不緩衝，因此會直接輸出
```cpp
cout << flush; // 手動輸出flush
```

---

## 以 EOF 結束
當題目告訴你以EOF(End of file)結束時，你就要使用
```cpp
while(cin >> ...) {
    ...
}
```

---

## Stringstream
- 一種 iostream，也就是說，可以流東西進去，也可以流東西出來(也可以想像成一個容器)
- 與 cin 相同會以空格分段，且變數型態必為 string
```cpp
stringstream ss;
ss << 5 << ' ' << 7; // input > 5,7
int tmp;
ss >> tmp; // tmp = 5
```

---

## Input/Output 優化
### stdio 與 iostream
- `stdio` 用 `scanf` 和 `printf`
```cpp
int a;
scanf("%d",&a);
printf("%d",a);
```
- `iostream` 用 `cin` 和 `cout`
```cpp 
int a;
cin >> a;
cout << a;
```
### stdio比較快？

- 前面說過，cout 有緩衝區，不會直接輸出出去
- **cin** 會自動 **flush**，**scanf**,**printf** 則不會
### 強制取消 **flush**
```cpp
cin.tie(0);
```
### 換行的 **flush**
- 請避免使用 `endl`，因為他其實不只是換行
```cpp
cout << flush << '\n';
```
- 取消 `flush`，用單純的換行就好
```cpp
cout << '\n';
```
```cpp
#define endl '\n'
```
### 解除同步
- 這兩種輸入輸出系統為了避免混用所造成的結果，會讓這兩個同步，但是我們基本上不會混用
```cpp
ios_base::sync_with_stdio(false);
```
### 邪教
```cpp
bool rit(auto& x) {
	x = 0; char c = cin.rdbuf()->sbumpc(); bool neg = 0;
	while (!isdigit(c)) {
		if (c == EOF) return 0;
		if (c == '-') neg = 1;
		c = cin.rdbuf()->sbumpc();
	}
	while (isdigit(c))
		x = x * 10 + c - '0', c = cin.rdbuf()->sbumpc();
	return x = neg ? -x : x, 1;
}
void wit(auto x) {
	if (x < 0) cout.rdbuf()->sputc('-'), x = -x;
	char s[20], len = 0;
	do s[len++] = x % 10 + '0'; while (x /= 10);
	while (len) cout.rdbuf()->sputc(s[--len]);
}
```

## Reference
- [WiwiHo的競程筆記](https://cp.wiwiho.me/io-optimize/)
- [Stream](https://cplusplus.com/reference/ios/)
