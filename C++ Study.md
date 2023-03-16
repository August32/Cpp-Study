# C++ Study

## 1. 文字和数值

- 字符常量：

  八进制：' \101'  --> A

  十六进制：' \x61' --> a

- 字符串常量

- 数值常量：

  八进制：010

  十六进制：0x10

- 逻辑常量



## 2. 内联函数

​	内联函数一般写在**头文件**中，inline 是C++关键字，函数定义返回类型前加上 inline，即可以把函数指定为内联函数。

​	inline **必须与函数定义放在一起才能使函数成为内联函数，仅仅将inline放在函数声明前面不起任何作用**。

​	inline是一种“用于实现”的关键字，而不是一种“用于声明”的关键字。

### 语法

```c++
inline int add(int x,int y){int a = x+y;return a;}
```

### 为什么要使用内联函数？

​	当程序执行函数调用时，系统要**建立栈空间，保护现场，传递参数以及控制程序执行的转移**等等，这些工作需要系统时间和空间的开销。

​	当函数功能简单，使用频率很高时，为了提高效率，可以使用内联函数，**在编译期间编译器能够在调用时内联展开该函数**，这样可以**解决一些频繁调用的函数大量消耗栈空间（栈内存）**的问题。

### 对比：inline与static的区别

不同点：

- 内联函数**没有开栈清栈的开销**，而static函数有；
- inline编译阶段代码展开导致函数本文件可见；而static是因为符号属性为local本文件可见；

### 对比：inline和宏的区别

不同点：

- inline编译时处理**有类型检查和安全检查**，宏预编译时处理无类型检查和安全检查；
- 宏无法调试，而内联**可以进行调试**；
- 内联比宏更加安全，是一种更加安全的宏；



## 3. 函数模板

​	C++提供了模板(template)编程的概念。所谓模板，实际上是建立一个**通用函数或类**，其类内部的类型和函数的形参类型不具体指定，用一个虚拟的类型来代表。这种通用的方式称为模板。**函数模板不允许自动类型转化。**

​	模板是泛型编程的基础,泛型编程即以一种独立于任何特定类型的方式编写代码。

### 语法

```c++
//template 关键字告诉C++编译器 要开始泛型编程了
//T - 参数化数据类型
template <typename T, typename T2>
T Max(T a, T2 b) {
    return a > b ? a : b;
}

//定义一个类
class Son {
public:
    Son(int age) { m_age = age; }
    ~Son(){}

    int getAge() { return m_age; }

    //重载 > 运算符
    bool operator>(Son& res) const{
        if (this->m_age > res.m_age) return true;

        return false;
    }
    
private:
    int m_age;
    
};

int main(void){

	int n = 6;
	int m = 9;
	char a = 'c';	//'c' 对应的ascll码值是 99

	cout << "Max<int, char>(m, a): " << Max<int, char>(m, a) << endl;	//显式类型调用
	cout << "Max(m, a): " << Max(m, a) << endl;	//自动数据类型推导

    Son s1(18);
	Son s2(21);

	cout << "Max(s1, s2): " << Max(s1, s2).getAge() << endl;
	return 0;
}

```

### 为什么要使用函数模板？

​	如果在一个项目中，需求是能够实现多个函数用来返回两个数的最大值，要求能支持char类型、int类型、double类型变量。如果只使用普通函数，那我们需要写三个重载函数，而如果是使用函数模板，只需要申明一个函数模板，利用模数模板的类型自动推导特性就可以实现。

### 对比：函数模板和普通函数的区别

1. 函数模板可以像普通函数一样被重载
2. C++编译器优先考虑普通函数
3. 如果函数模板可以产生一个更好的匹配，那么选择模板
4. **可以通过空模板实参列表的语法限定编译器只通过模板匹配**



## 4. 关键字

### Const：形参中指定Const

在函数的形参中加上Const，明确表示函数中的实参无须修改。

```c++
void func(const int* x);
void func(const int& x);
```

### typedef：创建别名

```c++
typedef unsigned long int Count;
Count num = 1;
```




## 5. 静态数组：array

对于数组s：

s 和 &s[0] 都表示数组第一个元素的地址；*s 和 s[0] 都表示数组第一个元素的值。

注意，数组名 s 不能置换为除数组的第一个元素之外的地址。

### array列表初始化

```c++
int s[3] = {1,2,3}; // 一维数组
array<int, 3> s = {1, 2, 3};  // 基于std::array创建一维数组，可以调用array方法
int s2[3][4] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12}; // 二维数组
array<array<int, 3>,4> s2 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12}; // 基于std::array创建二维数组，可以调用array方法
```

### array范围for语句

```c++
// 一维数组范围for语句
for (auto a : s) {
    cout << a << ends;
}

cout << endl;

// 二维数组范围for语句
for (auto &row : s2)
{
    for (auto a : row)  //只读。可读可写改成for (auto &a : row)
    {
        cout << a << ends;
    }
    cout << endl;
}
```

### array自定义排序

```C++
// 用自定义函数对象排序
struct {
    bool operator()(int a, int b) const
    {
        return a > b;
    }
} customLess;

sort(s.begin(), s.end(), customLess);
```

### array动态内存分配

```c++
int number;
cin >> number;
float *s3 = new float[number];
```

### 处理字符串数组的标准库函数

```c++
#include <cstring>

size_t strlen(const char* s);	// 显示字符串s长度
char* strcpy(char* s1,const char* s2);	// 复制，将字符串s2复制到数组s1中
char* strcat(char* s1,const char* s2);	// 拼接，通过将字符串s2添加到字符串s1的末尾来返回s1
int strcmp(const char* s1,const char* s2);	// 比较，比较字符串s1和s2，当s1 < s2时，返回负值；s1 = s2时，返回0；s1 > s2时，返回正值
```



## 6. 枚举类型：enum

​	枚举类型(enumeration)是 C++ 中的一种派生数据类型，它是由用户定义的若干枚举常量的**集合**。

定义枚举类型的主要目的是：增加程序的可读性。枚举类型最常见也最有意义的用处之一就是用来**描述状态量**，

### 声明与定义

```c++
enum color_set1 {RED, BLUE, WHITE, BLACK} color1, color2;
enum color_set2 {GREEN2, RED2, YELLOW2, WHITE2} color3, color4;

color3 = RED2;		//将枚举常量值赋给枚举变量
color4 = color3;	//相同类型的枚举变量赋值，color4的值为RED2
int  i = color3;	//将枚举变量赋给整型变量，i的值为1
int  j = YELLOW2;	//将枚举常量赋给整型变量，j的值为0
```





## 7. 结构体类型：struct

​	结构体声明只声明一个结构体“看起来是什么样子的”，所以**不会在内存中创建成员变量**。只有通过定义该结构体类型的变量来实例化结构体，才有地方存储初始值。

```cpp
    //非法结构体声明
    struct Date
    {
        int day = 23,
        month = 8,
        year = 1983;
    }；
```

### 定义与声明

①	先定义结构体类型再单独进行变量定义

```c++
    struct Student
    {
        int Code;
        char Name[20];
        char Sex;
        int Age;
    };
    Student Stu;
    Student StuArray[10];
    Student *pStu;  // 也可以用new来动态创建（在堆中，生存周期就可以自己决定）
```

②	比较安全的带构造的结构体


```c++
    struct node{
        int data;
        string str;
        char x;
        //注意构造函数最后这里没有分号哦！
        node() :x(), str(), data(){} //无参数的构造函数数组初始化时调用
        node(int a, string b, char c) :data(a), str(b), x(c){}//有参构造
    };
    node N1; // 无参构造
    node N2(10,"abc",'a'); // 有参构造
```

### 赋值与访问

**赋值**：初始化结构体变量成员的最简单的方法是使用初始化列表。

```c++
	Stu = {21,"ChenYiKai",'M',23};
```

**访问**：结构体中的变量，可以直接通过`"."`操作符来访问，而对于结构体指针而言：必须通过`"->"`符号来访问指针所指结构体的变量。**如果是使用结构体指针来作为函数参数，则将传递地址，这种方式在使用大型结构体时，效率会更高。**





## 8. 共用体类型：union

​	**共用体的各个成员不能同时带有值，全体成员只能有一个成员的值。**共用体是用于节省有限内存的一种数据类型。

​	**一般情况下unnion可以用来做各种基础数据类型转换。**

### 	声明和定义

```c++
union DEMO{
    char status;
    int a;
    int serial[4];
}demo;
```

<img src="C:\Users\老波比\AppData\Roaming\Typora\typora-user-images\image-20230316171719652.png" alt="image-20230316171719652" style="zoom: 67%;" />

​	上面这个表格代表内存中的数据，demo.status 的值为0x1；demo.a的值为，0x4321。以此类推demo.serial的值就出来了。这个属性是union特有的。













