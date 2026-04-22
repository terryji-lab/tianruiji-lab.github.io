# 不同形状面积计算(考察函数重载)
## Description
设计重载函数Area，分别实现圆、长方形和三角形面积的求解。

其中：

double Area(double r)用于圆的面积求解(PI=3.1416)；

double Area(double len, double wid)用于长方形面积的求解；

double Area(double a, double b, double c)用于三角形面积的求解(注意采用海伦公式求解)。
## Input
输入圆的半径，长方形的长与宽，三角形的三条边长。
## Output
圆、长方形和三角形的面积。
## Sample
```cpp
2
3.2 4.5
3 4 5
```

```cpp
12.5664
14.4
6
```
## Code
```cpp
#include <iostream>
#include <cmath>
using namespace std;
const double PI=3.1416;
double Area(double r)
{
	return PI*r*r;
}
double Area(double len,double wid)
{
	return len*wid;
}
double Area(double a,double b,double c)
{
	double p=(a+b+c)/2;
	return sqrt(p*(p-a)*(p-b)*(p-c));
}
int main()
{
	double r,len,wid,a,b,c;
	cin>>r>>len>>wid>>a>>b>>c;
	cout<<Area(r)<<endl<<Area(len,wid)<<endl<<Area(a,b,c);
	return 0;
}
```
## 默认参数易错点
以下均演示错误代码
1. 仅返回值不同，绝对不是重载
```cpp
void fun(int a);
int fun(int a);   //  重定义报错，不是重载
```
2. 形参变量名不同 ， 不算重载
```cpp
void fun(int x);
void fun(int y);   //  重复定义，不是重载
```
3. 值传递的 const 不算重载；引用 / 指针的 const 算重载
```cpp
void fun(int a);
void fun(const int a);  //  不是重载，重定义

void fun(int &a);
void fun(const int &a); //  是重载

void fun(int *p);
void fun(const int *p); //  是重载
```
4. 默认参数引发二义性歧义，编译报错
```cpp
void fun(int a = 10);
void fun();

fun();  //  二义性，编译器不知道调用哪一个
```
5. 参数顺序交错不同 ，属于重载
```cpp
void fun(int a, double b);
void fun(double a, int b); //  重载
```
