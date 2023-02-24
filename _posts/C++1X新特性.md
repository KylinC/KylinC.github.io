---
dlayout:    post
title:      C++1X新特性
subtitle:   C++1X新特性
date:       2023-2-24
author:     Kylin
header-img: img/bg-cpp.jpg
catalog: true
tags:
    - C++
---



# A Guidebook for C++1X

### 语言可用性强化



#### 类型推导 

- auto

```
for (auto it = magicFoo.vec.begin(); it != magicFoo.vec.end(); ++it) {
	std::cout << *it << ", ";    // 迭代器使用auto
}                        
auto i = 5;              // i 被推导为 int
auto arr = new auto(10); // arr 被推导为 int *

```

**注意：** `auto` 不能用于函数传参，因此下面的做法是无法通过编译的（主要是以为保留重载）

```
int add(auto x, auto y);

2.6.auto.cpp:16:9: error: 'auto' not allowed in function prototype
int add(auto x, auto y) {
     ^~~~
```

**注意：** `auto` 还不能用于推导数组类型

```
auto auto_arr2[10] = {arr};   // 错误,  无法推导数组元素类型

2.6.auto.cpp:30:19: error: 'auto_arr2' declared as array of 'auto'
 auto auto_arr2[10] = {arr};
```

- decltype 

> 类似 typeof

```
decltype(表达式)

std::is_same<T, U> 用于判断 T 和 U 这两个类型是否相等
```

```
// 判断 变量 x, y, z 是否是同一类型。

int main() {

    auto x = 1;
    auto y = 2;
    decltype(x+y) z;  //声明一个x+y同类型的变量z

    if (std::is_same<decltype(x), int>::value)
        std::cout << "type x == int" << std::endl;
    if (std::is_same<decltype(x), float>::value)
        std::cout << "type x == float" << std::endl;
    if (std::is_same<decltype(x), decltype(z)>::value)
        std::cout << "type z == type x" << std::endl;

    return 0;
}
```

- 尾返回类型推导

以下函数定义了返回x+y类型的值

```
template<typename T, typename U>
auto add2(T x, U y) -> decltype(x+y){
    return x + y;
}
```

```c++
#include <iostream>

template<typename T, typename U>
auto add2(T x, U y) -> decltype(x+y){
    return x + y;
}

template<typename T, typename U>
auto add3(T x, U y){
    return x + y;
}

int main(){
    // after c++11
    auto w = add2<int, double>(1, 2.0);
    if (std::is_same<decltype(w), double>::value) {
        std::cout << "w is double: ";
    }
    std::cout << w << std::endl;

    // after c++14
    auto q = add3<double, int>(1.0, 2);
        if (std::is_same<decltype(q), double>::value) {
        std::cout << "q is double: ";
    }
    std::cout << q << std::endl;
}
```



#### std::initializer_list 统一初始化

```
vector<int> a {1,2,3,4};
int arr[3] {1,2,3};
class c = {1,2,3}
```

```c++
#include <initializer_list>
#include <iostream>
#include <vector>
class MagicFoo {
public:
    std::vector<int> vec;
    MagicFoo(std::initializer_list<int> list) {
        for (std::initializer_list<int>::iterator it = list.begin();
             it != list.end(); ++it)
            vec.push_back(*it);
    }
  	// 列表初始化也可以作为普通函数参数
    void foo(std::initializer_list<int> list) {
            for (std::initializer_list<int>::iterator it = list.begin(); it != list.end(); ++it) vec.push_back(*it);
    }
};
int main() {
    // after C++11
    MagicFoo magicFoo = {1, 2, 3, 4, 5};

    std::cout << "magicFoo: ";
    for (std::vector<int>::iterator it = magicFoo.vec.begin(); it != magicFoo.vec.end(); ++it) std::cout << *it << std::endl;
}
```



#### 结构化绑定（允许多返回值）

```c++
#include <iostream>
#include <tuple>

std::tuple<int, double, std::string> f() {
    return std::make_tuple(1, 2.3, "456");
}

int main() {
    auto [x, y, z] = f();
    std::cout << x << ", " << y << ", " << z << std::endl;
    return 0;
}
```



#### 区间for迭代

```c++
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {1, 2, 3, 4};
    for (auto element : vec)
        std::cout << element << std::endl; // read only
    for (auto &element : vec) {
        element += 1;                      // writeable
    }
}
```



#### using 取代 typedef

```c++
typedef int MyInt;
using MyInt = int;
using TrueDarkMagic = MagicType<int, int>;

int main() {
    TrueDarkMagic<bool> you;
}
```



#### 默认模版参数

```
template<typename T = int, typename U = int>
auto add(T x, U y) -> decltype(x+y) {
    return x+y;
}

// Call add function
auto ret = add(1,3);
```



#### 变长参数模版



### 运行时强化

#### lambda表达式

> Lambda 表达式，实际上就是提供了一个类似匿名函数的特性，而匿名函数则是在需要一个函数，但是又不想费力去命名一个函数的情况下去使用的。这样的场景其实有很多很多，所以匿名函数几乎是现代编程语言的标配。

基本语法：

```
[捕获列表](参数列表) mutable(可选) 异常属性 -> 返回类型 {
// 函数体
}
```

- 捕获

所谓捕获列表，其实可以理解为参数的一种类型，Lambda 表达式内部函数体在默认情况下是不能够使用函数体外部的变量的，这时候捕获列表可以起到传递外部数据的作用。

> - [] 空捕获列表
> - [name1, name2, ...] 捕获一系列变量
> - [&] 引用捕获, 让编译器自行推导引用列表
> - [=] 值捕获, 让编译器自行推导值捕获列表

```c++
#include <iostream>

void lambda_value_capture() {
    int value = 1;
    auto copy_value = [value] {
        return value;
    };
    value = 100;
    auto stored_value = copy_value();
    std::cout << "stored_value = " << stored_value << std::endl;
    // 这时, stored_value == 1, 而 value == 100.
    // 因为 copy_value 在创建时就保存了一份 value 的拷贝
}

void lambda_reference_capture() {
    int value = 1;
    auto copy_value = [&value] {
        return value;
    };
    value = 100;
    auto stored_value = copy_value();
    std::cout << "stored_value = " << stored_value << std::endl;
    // 这时, stored_value == 100, value == 100.
    // 因为 copy_value 保存的是引用
}

int main(){
    lambda_value_capture();
    lambda_reference_capture();
    return 0;
}
```

- 泛型 Lambda

> auto类型在c++14中可以变成参数类型

```
auto add = [](auto x, auto y) {
    return x+y;
};

add(1, 2);
add(1.1, 2.2);
```



#### 函数对象包装器

C++11 `std::function` 是一种通用、多态的函数封装，它的实例可以对任何可以调用的目标实体进行存储、复制和调用操作，它也是对 C++ 中现有的可调用实体的一种类型安全的包裹（相对来说，函数指针的调用不是类型安全的），换句话说，就是函数的容器。

```
#include <functional>
#include <iostream>

int foo(int para) {
    return para;
}

int main() {
    // std::function 包装了一个返回值为 int, 参数为 int 的函数
    std::function<int(int)> func = foo;
		// function<返回类型列表(参数类型列表)>
    int important = 10;
    std::function<int(int)> func2 = [&](int value) -> int {
        return 1+value+important;
    };
    std::cout << func(10) << std::endl;
    std::cout << func2(10) << std::endl;
    return 0;
}
```

```
std::function<double(int, int)> func = [](int a, int b) -> double {
    // 在这里编写函数体
    return a * b * 0.5;
};

```













