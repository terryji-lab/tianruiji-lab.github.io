# 动态内存分配(new和delete的使用)
## Description
输入n个整型数，包含<algorithm>，采用sort函数对其按从小到大顺序排序后输出。
## Input
采用cin输入整型数个数n及其对应的n个整数。
## Output
输出排序后的数据序列。
## Sample
```cpp
5
5 4 1 3 2
```

```cpp
1 2 3 4 5
```
## Code
```cpp
#include <iostream>
#include <algorithm>//算法库，包含sort函数
using namespace std;
int main()
{
	int n;
	cin>>n;
	int *arr=new int[n];//new开辟动态内存
	for(int i=0;i<n;i++)
	{
		cin>>arr[i];
	}
	sort(arr,arr+n);//左闭有开，快速排序
	for(int i=0;i<n;i++)
	{
		cout<<arr[i]<<' ';
	}
	delete[] arr;//释放内存
	return 0;
}
```
## 默认参数易错点
以下均演示错误代码
1. new[] 数组开辟，必须配 delete[]，绝对不能混用。
```cpp
//  严重错误
int *p = new int[5];
delete p;
```
2. 重复 delete 同一块内存。
```cpp
int *p = new int;
delete p;
delete p;   //未定义行为，程序直接崩溃
```
3. delete 空指针 是安全的，很容易记错。
```cpp
int *p = nullptr;
delete p;   //完全合法，不会报错
```
4. 默只 new 不 delete 造成内存泄漏。
```cpp
void fun()
{
    int *p = new int;
    // 没有 delete p;
}
```
5. 指针重复赋值，原内存隐秘泄漏。
```cpp
int *p = new int;
p = new int;    //第一块内存直接丢失，永久泄漏
```
6. 栈变量绝对不能用 delete
```cpp
int a = 10;
int *p = &a;
delete p;   //致命错误，程序崩溃
```
delete 只能释放堆内存,栈内存由系统自动分配、自动回收，禁止手动 delete。

7. delete 之后，指针不会自动置空，变成野指针,需要手动置空为``nullptr``。
```cpp
int *p = new int;
delete p;
// 内存已释放，但p的地址没变！
*p = 10;    //野指针访问，程序崩溃
```
8. void* 接收 new 的结果，无法 delete。delete 需要知道对象类型来计算大小、调用析构，void* 无类型信息。
```cpp
void *p = new int;
delete p;   //编译报错
```
9. 指针值传递，函数内 new 造成泄漏。
```cpp
void fun(int *p)
{
    p = new int; // 只修改形参副本
}
int main()
{
    int *q = nullptr;
    fun(q);
    // q依旧为空，fun里开辟的内存完全泄漏
}
```
在这里``q``和``p``虽然刚开始指向同一地址，但本质上是不同的变量，``new``相当于擦除原来地址，重新分配一段带有动态内存空间的地址，所以相当于给临时变量``p``分配了内存。