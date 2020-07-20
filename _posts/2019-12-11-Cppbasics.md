---
layout: post
title: C++基础
date: '2019-12-11'
categories: [C++, Interview]
---

## const

- 修饰变量，说明该量不可改变
- 修饰指针，分为指向常量的指针（pointer to const），自身是常量的指针（const pointer）
- 修饰引用，指向常量的引用，既避免了拷贝，又避免了值被修改
- 修饰成员函数，说明该成员函数内不能修改成员变量

### const的指针与引用

- 指针
  - 指向常量的指针（pointer to const）
  - 自身是常量的指针（const pointer）
- 引用
  - 指向常量的引用（reference to const）
  - 没有const reference，因为引用本身就是const pointer

> （为了方便记忆可以想成）被const修饰（在const后面）的值不可以被改变

### 使用

```c++
//修饰函数
void fun1(const int var);	//修饰变量，形参在函数内不可改变
void fun2(const char* var);	//修饰指向常量的指针，参数指针所指向内容为常量
void fun3(char* const var);	//修饰常指针，参数指针为常量（常指针）
void fun4(const int& var);	//修饰引用，参数引用在函数内为常量
void fun5(void) const;		//修饰成员函数
//修饰返回值
const int fun6();		//返回一个常数
const int* fun7();		//返回一个指向常量的指针
const int* p = fun7();
int* const fun8();		//返回一个常指针
int* const p = fun8();
```



## static

- 修饰普通变量，修改变量的存储区域（静态区）和生命周期（程序整个生命周期）。在main函数运行前就分配了空间，如果有初始化值就用初始化值初始化它，如果没有，就是用默认初始化值
- 修饰普通函数，表名函数的作用范围（仅在该函数所在的文件内）。在多人开发项目中，为了防止与他人命名空间内的函数重名，可以将函数定义为static
- 修饰成员变量，修饰成员变量使所有的对象只保存一个该变量，并且不需要生成对象就可以访问该变量，使用`::`运算符
- 修饰成员函数，修饰成员函数使不需要生成对象就可以访问该函数，并且在static修饰的成员函数内不可以访问非静态成员变量

## this指针

- 是一个隐含于每一个非静态成员函数中的函数指针，指向**调用该成员函数的那个对象**
- 当对一个对象调用成员函数时，编译程序先将对象的地址赋给this指针，然后调用该成员函数，每次成员函数存取成员变量时，都**隐式**调用this指针
- 当一个成员函数被调用时，自动向他传递一个隐含的参数（this指针）
- this指针隐含的声明为：`classname* const this`，但是不能给this指针赋值，在类的const成员函数中，其类型为`const classname* const this`，这说明不能对这种对象的成员变量进行赋值操作
- this指针不是一个常规变量，而是一个右值，所以不能取得this的地址（不能`&this`）
- 在以下的场景中通常需要显示引用this指针：
  - 为实现对象的链式引用
  - 为避免对同一个对象进行赋值操作
  - 在实现一些数据结构的时候，比如list

## 宏

- 宏定义可以实现类似于函数的功能，但是不是真的函数，宏定义括弧中的”参数“也不是真的参数，在宏展开的时候对"宏参数"进行一对一替换

## inline内联函数

### 特征

- 相当于把内联函数里面的内容复制到调用内联函数处
- 相当于不用执行进入函数的步骤，直接进入函数体
- 相当于宏，但是比宏多了类型检查，真正具有函数特性
- 编译器一般不内联包含循环、递归、switch等复杂操作的内联函数
- 在**类声明中定义的成员函数**，除了虚函数的其他成员函数都会**自动隐式**的当成内联函数，在外部定义的成员函数需要显示内联
- 程序中的`inline`关键字只是给编译器内联建议，能否内联由编译器决定

### 编译器处理步骤

1. 将`inline`函数体复制到`inline`函数调用处
2. 为所有`inline`函数的局部变量分配内存空间
3. 将`inline`函数的输入参数和返回值映射到调用方法的局部变量空间中
4. 如果有多个返回点，将其转变为`inline`函数代码块末尾的分支（`GOTO`）

### 优点

- 内联函数同宏函数一样将在被调用处进行代码展开，省去了参数压栈、栈帧开辟和回收、结果返回等，从而提高了程序运行速度
- 相对于宏函数而言，在代码展开时会进行安全检查胡总和自动类型转换（同普通函数），而宏定义不会
- 在类中声明同时定义的成员函数，自动转换为内联函数，因此内联函数可以访问类的成员变量，而宏定义不能
- 内联函数可以调试，宏定义不能

### 缺点

- 代码膨胀，以代码膨胀为代价，消除函数调用的开销
- 无法随着函数库升级而升级
- 是否内联，由编译器决定，`inline`关键字只是建议

> 虚函数可以是内联函数吗？
>
> - 可以，但是当虚函数表现出多态的时候不能内联
> - 内联是在编译期决定的，而虚函数的多态性表现在运行期，编译器无法知道运行期调用哪个代码，所以虚函数表现出多态时（运行期）无法内联
> - `inline virtual`唯一可以内联的时候是：编译器知道所调用的对象是哪个类，这只有编译器具有实际对象而不是对象的指针或者引用时才会发生

## volatile

```c++
volatile int i = 10;
```

- `volatile`关键字是一种类型修饰符，用它声明的类型变量可以被某些编译器未知的因素改变（操作系统，硬件等）。所以使用该关键字告诉编译器不应对这样的对象进行优化
- `volatile`关键字声明的变量，每次访问都必须从内存中取值（没有被`volatile`修饰的变量，可能由于被编译器优化，从CPU寄存器中取值）
- `const`（**编译期保证代码中没有被修改的地方，运行的时候不受限制**）可以是`volatile`（编译期告诉编译器不优化该变量，在运行期每次都从内存中取值）：表示一个变量在程序编译期不能被修改并且不能被优化，在**程序运行期变量值可能会被修改**，每次使用到该变量值都需要从内存中读取，防止意外错误
- 指针可以是`volatile`的

## assert()

- 断言，宏，非函数，在<cassert>头文件中，作用就是如果他的条件返回false，则终止程序执行，可以定义NDEBUG关闭`assert`，但是定义要在头文件前



## pragram pack(n)

- 设定`struct`，`union`以及类成员变量以n字节方式对齐，也就是让数据在内存中连续存储，同时以n字节对齐

### 使用

```c++
#pragram pack(push)	//保持对齐状态
#pragram pack(4)	//设定为4字节对齐
struct test{
	char m1;
	double m4;
	int m3;
};
#pragram pack(pop)	//恢复对齐状态
```



## 位域

```c++
Bit mode: 2;
```

- 类可以将其（非静态）成员变量定义为位域（bit-field），在一个位域中含有一定数量的二进制位
- 当一个程序需要向其他程序或者硬件设备传递二进制数据时，通常会用到位域
- 位域在内存中的分布和机器有关
- 位域的类型必须是整型或者枚举类型的，带符号类型中的位域的行为将因具体实现而定
- 取地址运算符`&`不能作用于位域，任何指针都无法指向类的位域

## extern

- 被`extern`限定的函数或变量是`extern`类型的
- 被`extern "C"`修饰的变量或者函数是按照C语言的方式编译的链接的，可以避免C++因为符号修饰导致代码不能和C语言库中的符号进行链接的问题

## struct和typedef struct

### C

```c++
typedef struct student{
	int age;
}s;
// 等价于
struct student{
	int age;
};
typedef struct student s;
```

- 上述的`s`等价于`struct student`，但是两个标识符名称空间不同
- 另外还可以定义于`struct student`不会冲突的`void student()`

### C++

- C++中的编译器定位符号的规则（搜索规则）于C中的不同，会导致：
  - 如果在类标识符空间定义了`struct student{...};`，使用`student me;`时，编译器将会搜索全局标识符表，`student`未找到时，则在类标识符中搜索；也即表现为可以使用`student`也可以使用`struct student`
  - 如果顶一个同名函数，则`student`只表示函数，结构体智能使用`struct student me;`表示，也即`s me;`

## struct and class in C++

- 总的来说，struct 更适合看成是一个数据结构的实现体，class 更适合看成是一个对象的实现体。

### 区别

- 最本质的一个区别就是默认的访问控制
  - 默认的继承访问权限。struct 是 public 的，class 是 private 的。
  - struct 作为数据结构的实现体，它默认的数据访问控制是 public 的，而 class 作为对象的实现体，它默认的成员变量访问控制是 private 的。



## union

- 联合（union）是一种节省空间的特殊的类，一个 union 可以有多个数据成员，但是在**任意时刻只有一个数据成员可以有值**。当某个成员被赋值后其他成员变为未定义状态。联合有如下特点：
  - 默认访问控制符为 public
  - 可以含有构造函数、析构函数
  - 不能含有引用类型的成员
  - 不能继承自其他类，不能作为基类
  - 不能含有虚函数
  - 匿名 union 在定义所在作用域可直接访问 union 成员
  - 匿名 union 不能包含 protected 成员或 private 成员
  - 全局匿名联合必须是静态（static）的

## 使用C实现C++的类

- 使用C实现C++的对象特性（封装、继承、多态）
  - 封装：使用函数指针把属性和方法封装到结构体中
  - 继承：结构体嵌套
  - 多态：父类和子类方法的函数指针不同

## explicit关键字

- `explicit`修饰构造函数时，可以防止隐式转换或者复制初始化
- `explicit`修饰转换函数时，可以防止隐式转换，但按语境转换除外

### 使用

```c++
struct A{
	A(int){}
	operator bool() const {return true;}
};
struct B{
	explicit B(int){}
	explicit operator bool() const {return true;}
};
int main{
    B b1(1);	//OK,直接初始化
    B b2 = 1;	//Error,被explict修饰的构造函数不能复制初始化
}
```

## friend友元类和友元函数

- 能访问私有成员
- 破环封装性
- 友元关系不能传递
- 友元关系的单向性
- 友元声明的形式和数量不受限制

## using

- 一条`using`声明语句一次只引入命名空间的一个成员，这使得我们可以清楚知道程序所引用的到底是哪个命名空间里的函数，比如

```
using namespace_name::name;
```

- 构造函数的`using`声明：在C++11中，可以直接重用其直接基类定义的构造函数，比如

```
class Derived:Base{
public:
	using Base:Base;
};
```

- 如上，对于基类的每个构造函数，编译器都生成一个与之对应（形参列表完全相同）的派生类构造函数，生成如下类型构造函数：

```
Derived(params):Base(args){}
```

- `using`指示使得某个特定命名空间中所有名字都可见，这样就无需再为他们都加上任何前缀了，比如

```
using namespace std;
```

> 应当尽量少使用`using`指示污染命名空间，尽量多使用`using`声明

## ::范围解析运算符

- 分类
  - 全局作用域符（`::name`）：用于类型名称（类，类成员，成员函数，变量等）前，表示作用域为全局命名空间
  - 类作用域符（`class::name`）：用于表示指定类型的作用域范围是具体某个类的
  - 命名空间作用域符（`namespace::name`）：用于表示指定类型的作用域范围是具体某个命名空间的

## enum枚举类型

- 限定作用域的枚举类型

```c++
enum class open_modes{input, output, append};
```

- 不限定作用域的枚举类型

```c++
enum color {red, yellow, green};
enum {floatPrec=6, doublePrec=10};
```

## decltype

- 用于检查实体的声明类型，表达式的类型以及值分类，使用

```c++
decltype ( expression )
```

- 使用说明

```c++
//尾置返回允许再参数列表之后声明返回类型
template<typename T> auto fun1(T beg, T end) -> decltype(*beg){
	//...
	return *beg;
}
//为了使用模板参数成员，必须用typename
template<typename T> auto fun2(T beg, T end) -> typename remove_reference<decltype(*beg)>::type{
	//...
	return *beg;
}
```

## 引用

### 左值引用

- 常规引用，一般表示对象的身份

### 右值引用

- 右值引用就是必须绑定到右值（一个临时对象，将要销毁的对象）的引用，一般表示对象的值
- 右值引用可以实现转移语义（Move Sementics）和精确传递（Perfect Forwarding），他的主要目的有两个：
  - 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率
  - 能够更简洁明确地定义泛型函数

### 引用折叠

- `X& &`、`X& &&`、`X&& &` 可折叠成 `X&`
- `X&& &&` 可折叠成 `X&&`

## 初始化列表

- C++构造函数的初始化列表可以使得代码更加简洁，并且少了一次调用默认构造函数的过程，可以用于全部成员变量，也可以只用于部分成员变量
- **成员变量的初始化顺序和初始化列表中列出的变量的顺序无关，只和成员变量再类中声明的顺序有关**
- 还有一个重要的功能 就是初始化`const`成员变量。**初始化`const`成员变量的唯一方法就是使用初始化列表**
- 引用类型，引用必须再定义的时候初始化，并且不能重新赋值，所以也要写在初始化列表里面

### initializer_list列表初始化

- 使用花括号初始化器列表初始化一个对象，其中对应构造函数接受一个`std::initializer_list`参数


