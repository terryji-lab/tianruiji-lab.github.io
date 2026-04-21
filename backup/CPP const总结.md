# C++ `const` 用法完整总结
---

## 1. 基本用法：const 变量（常量）

```cpp
const int MAX = 100;        // 必须初始化，不能再修改
int const MAX2 = 200;       // 等价写法，推荐 const 放在类型前面

// MAX = 101;               // 编译错误
```
---
## 2.const与指针

写法 | 含义 | 指针本身是否可改 | 指向的内容是否可改 | 记忆口诀
-- | -- | -- | -- | --
const int* p | 指向 const 的指针 | 可以 | 不可以 | 指针指向常量
int const* p | 同上 | 可以 | 不可以 | 同上
int* const p | const 指针 | 不可以 | 可以 | 指针本身是常量
const int* const p | const 指针指向 const | 不可以 | 不可以 | 两者都是常量


代码示例：
```cpp
int a = 10, b = 20;

// 1. 指向常量的指针
const int* p1 = &a;     // *p1 = 11;  // 错误
p1 = &b;                // 正确

// 2. 常量指针
int* const p2 = &a;     // p2 = &b;   // 错误
*p2 = 11;               // 正确

// 3. 常量指针指向常量
const int* const p3 = &a;  // 两者都不能改
```

## 3.const引用
```cpp
void func(const int& x) {   // 防止函数内部修改参数
    // x = 100;             // 错误
    std::cout << x;
}

int main() {
    int val = 42;
    func(val);              // 自动绑定，无拷贝
}
```

## 4.const在函数中
```cpp
// 参数
void print(const std::string& s);        // 推荐写法

// 返回值
const std::vector<int>& getData();       // 防止调用者修改

// const 成员函数（类中最重要！）
class MyClass {
public:
    int getValue() const {          // const 成员函数
        return value;               // 只能读，不能写
    }

    void setValue(int v) {          // 非 const
        value = v;
    }

private:
    int value = 0;
};
```
**const 成员函数规则：**
- 只能调用其他``const``成员函数
- 不能修改任何``mutable``成员

## 5.const 对象 + mutable
```cpp
const MyClass obj;           // const 对象只能调用 const 成员函数

class Cache {
    mutable int cacheHit = 0;   // 突破 const 限制
public:
    int get() const {
        cacheHit++;             // 可以修改
        return value;
    }
};
```

## 常见错误

1. ``const int* p`` 和 ``int* const p`` 搞反
2. const 对象只能调用 const 成员函数
3. **临时对象只能绑定到 const 引用**