# 进制转换（考察带默认参数的函数）
## Description
输入一个正整数n，要求设计一个带默认参数的函数Print(int n, int base=10, bool bUpper=false)，分别实现其对应10进制、8进制和16进制大小写的输出。
## Input
正整数n
## Output
n对应的10进制数、8进制数和大小写16进制数。
## Sample
```cpp
15
```

```cpp
Dec: 15
Oct: 17
Hex: f
Hex: F
```
## Code
```cpp
#include <iostream>
#include <iomanip>//带有输出控制的头文件
using namespace std;
void Print(int n,int base=10,bool bUpper=false)
{
	if(bUpper)cout<<uppercase;//字母大写
	else cout<<nouppercase;//字母小写
	if(base==10)cout<<dec<<"Dec: ";//10进制
	else if(base==8)cout<<oct<<"Oct: ";//8进制
	else if(base==16)cout<<hex<<"Hex: ";//16进制
	cout<<n<<'\n';
}
int main()
{
	int x;
	cin>>x;
	Print(x);//带有默认参数
	Print(x,8);
	Print(x,16);
	Print(x,16,true);
	return 0;
}
```
## 默认参数易错点
以下均演示错误代码
1. 默认参数必须从右往左连续赋值，不允许左边参数有默认值，右边参数没有默认值。
```cpp
void f(int a = 1, int b, int c = 3);
void f(int a = 1, int b, int c);
```
2. 默认参数只能初始化一次,**函数声明**和**函数定义**二选一写默认值，不能两边都写。
```cpp
// 声明
void fun(int a = 10);
// 定义又写一遍默认值
void fun(int a = 10){}
```
3. 默认参数是编译期常量，默认值只能是常量、全局变量，不能用函数内局部变量做默认参数。
```cpp
void test()
{
    int x = 30;
    void fun(int a = x); // 局部变量不能当默认参数
}
```
4. 默认参数不区分函数重载，仅仅默认参数不同的两个函数，不算重载，调用时会直接产生二义性报错。
```cpp
void f(int a);
void f(int a = 10);

int main()
{
    f(); // 编译报错！二义性，编译器不知道调用哪个
}
```
5. 指针、空指针可以做默认参数，nullptr 默认参数。
```cpp
void fun(int *p = nullptr);//这是正确代码！
```
