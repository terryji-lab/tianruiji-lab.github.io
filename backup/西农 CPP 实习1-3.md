# 用函数模板实现数据交换(考察引用和函数模板)
## Description
设计函数模板Swap，能够实现整型数与整型数、浮点型数与浮点型数、字符串与字符串的交换。
## Input
```cpp
int main()
{
    int a, b;
    double x, y;
    string str1, str2;
    cin >> a >> b;
    cin >> x >> y;
    cin >> str1 >> str2;
    Swap(a, b);
    Swap(x, y);
    Swap(str1, str2);
    cout << a << " " << b << endl;
    cout << x << " " << y << endl;
    cout << str1 << " " << str2 << endl;
    return 0;
}
```
## Output
分别输出交换后的整型数、浮点型数和字符串。
## Sample
```cpp
100 200
21.5 31.4
asdfqw iyjl
```

```cpp
200 100
31.4 21.5
iyjl asdfqw
```
## Code
```cpp
#include <iostream>
using namespace std;
template<typename T>
void Swap(T&a,T&b)
{
	T c=a;
	a=b;
	b=c;
}
int main()
{
    int a, b;
    double x, y;
    string str1, str2;
    cin >> a >> b;
    cin >> x >> y;
    cin >> str1 >> str2;
    Swap(a, b);
    Swap(x, y);
    Swap(str1, str2);
    cout << a << " " << b << endl;
    cout << x << " " << y << endl;
    cout << str1 << " " << str2 << endl;
    return 0;
}
```
## 默认参数易错点
以下均演示错误代码
1. 模板参数绝不隐式类型转换
2. 可以给模板本身的默认类型参数
```cpp
template<typename T = int>
void fun(T a){}//正确
```
3. 仅模板参数名字不同，编译器认为是重复定义，不是重载
```cpp
template<typename T>
void fun(T a)

template<typename S>
void fun(S a);//两次重复定义
```
4. 普通函数 ＞ 完全匹配的模板函数 ＞ 隐式转换，普通函数不匹配，才会去实例化函数模板
```cpp
template<typename T>
void fun(T a){ cout<<"模板"; }

void fun(int a){ cout<<"普通函数"; }

int main()
{
    fun(10);    // 输出：普通函数
    fun(3.14);  // 没有普通double函数，调用模板
}
```
5. 模板内静态变量，每种类型独立一份
```cpp
template<typename T>
void fun()
{
    static int cnt = 0;
    cnt++;
    cout << cnt;
}

int main()
{
    fun<int>();  // 1
    fun<int>();  // 2
    fun<double>();// 1
}
```
6. 数组传参，模板参数自动退化为指针