# Pointer (指標)
### ~~學術倫理~~
- [c語言-超好懂的指標，初學者請進](https://kopu.chat/c語言-超好懂的指標，初學者請進～/)

---

## 概念
![sky](https://hackmd.io/_uploads/S1mcjgzop.png)
- **指標**：相當於網址，會指向資料
- 可以透過指標（網址）去取的資料（圖片）
### 記憶體

- 當宣告一個變數時，會在記憶體中申請一個位置來存放
- 舉例來說，一個$int$會佔 $4\ Byte$ 的空間
```cpp
int b = 2;
```
![int](https://hackmd.io/_uploads/S1lzTefia.png)
- 可以用`sizeof()`去看一個變數的大小
```cpp
int a = 2;
double b = 3.14;
long long c = 100;
cout << sizeof(a) << endl; // 4
cout << sizeof(b) << endl; // 8
cout << sizeof(c) << endl; // 8
```
### 變數
- 可以把照片的關係對應到變數上
- 網址就是**變數位址**（指向資料）

![example](https://hackmd.io/_uploads/B1eYAxMj6.png)

---

## 實作
### 取址符號 `&`
- 透過這個符號可以取得該**資料的位址**
- 相當於 `&圖片` 可以得到 `網址`
```cpp
int a = 1;
cout << "a : " << a;
cout << "address : " << &a;
//a : 1
//address : 0x7ffee1b5ef44
```
### 取值符號 `*`
- 透過這個符號可以取得一個位址的值
- 相當於 `*網址` 可以得到 `圖片`

```cpp
int a = 1;
cout << a;
cout << &a;
cout << *(&a);
//1,0x7ffee1b5ef44,1
```
> `&` 和`*`是反運算

---

## 指標變數
![pointer](https://hackmd.io/_uploads/HyDLbZMia.png)
(圖片來源：[頁首那裡](https://kopu.chat/c語言-超好懂的指標，初學者請進～/))

### 如何宣告

```cpp
int *pointer;
int* pointer;
//兩種都可以
```

### 賦值

```cpp
int a = 2;
pointer = &a;
```

**注意：**
`*`只是代表他是一個型態為指標的變數
所以不能寫成`pointer = a`

---

## Thinking
> 想想看輸出結果是什麼

```cpp
int a =1;
int *pointer = &a;

cout << "a的值: " << a << endl;
cout << "a的位址: " << &a << endl;
cout << "pointer的值: " << pointer << endl;
cout << "-------------------------------";
cout << endl;
//做一點改變
*pointer = 999;

cout << "*pointer的值: " << *pointer << endl;
cout << "a的值: " << a << endl;
cout << "pointer的位址: " << &pointer << endl;
```
### Result

```
a的值: 1
a的位址: 0x7ffdcf30302c
pointer的值: 0x7ffdcf30302c
-------------------------------
*pointer的值: 999
a的值: 999
pointer的位址: 0x7ffdcf303030
```
* a的值: 1
* a的位址: 0x7ffffd37d60c
> pointer的值就是a的位址
* pointer的值: 0x7ffffd37d60c
> 改變*pointer就是改變pointer位址上的資料的值
> 因為*pointer就是存放a的位址
> 所以*pointer其實就是a
* *pointer的值: 999
* a的值: 999
> 指標變數也是變數，所以有一個存放他的位址合情合理吧
* pointer的位址: 0x7ffffd37d610



