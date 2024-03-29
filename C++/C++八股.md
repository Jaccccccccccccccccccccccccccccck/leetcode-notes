#### const
1.修饰变量 表明变量不可被改变
- 常量必须初始化，否则编译报错
- 修改常量编译器会报错

2.修饰指针 分为指向常量的指针 和 自身是常量的指针  
- const int *ptr  // 指向常量的指针 const在前
  - 指向的变量必须为常量（使用const定义），指向常量的指针也必须为常量指针
  - 不能通过*ptr = xxx进行复制
- int *const ptr  // 自身是常量的指针
  - 指向的变量可以修改 即通过*ptr = xxx赋值
  - 指针本身不能赋值 ptr = &xxx 会报错

3.修饰引用 指向常量的引用，用于形参类型，避免拷贝，又避免函数对值的修改
4.修饰成员函数，说明该成员函数内不能修改成员变量。


#### static
1.修饰普通变量 修改变量的存储位置和生命周期，使变量存储在静态区，并在main函数运行前就分配空间并赋值
- 普通变量 改变变量的生存周期，存储位置
- 全局变量 改变全局变量的可见范围为本文件
2.修饰普通函数 限制函数的可见范围为本文件
3.修饰类的成员变量 所有的类成员仅有一个此变量，且不需要生成类实例即可访问
4.修饰成员函数 不需要生成类实例即可访问的函数，在此函数内不能访问非静态成员变量

#### 宏定义#define 和 const常量
|  宏定义 #define  | const  |
|  ----  | ----  |
| 相当于字符串替换  | 常量声明 |
| 预处理器处理  | 编译器处理 |
| 无类型安全检查  | 有安全类型检查 |
| 不分配内存  | 要分配内存 |
| 存储在代码段  | 存储在数据段 |
### 特殊用途语言特性
#### 默认实参
在函数声明时，可以给实参赋默认值，然后在调用函数的时候可以省略该参数
默认实参负责填补函数调用缺少的尾部参数
有默认值的参数必须都放在函数参数列表的后面 // 如果出现无默认值参数放在有默认值参数后，编译器会报错

#### inline内联函数
- 定义内联函数是为了减少函数调用的开销
- 函数内的代码会被编译器在相应的调用位置展开
- 相当于宏，但比宏多了类型检查

#### constexpr （const expression） (c++ 11)
可以定义一个变量和函数
告知编译器某个变量和函数和类是一个常量表达式或常量函数，
编译器会把常量函数在编译阶段进行运算

#### 引用
引用是对象的别名
- 不会进行值的copy
- 引用必须初始化
- 引用不允许改变引用的对象，不存在这样的语法

#### 指针
- 指针必须在使用前进行赋值，不然是野指针，常用空指针代表指针没有还未被定义
  - int *ptr = 0 or  int *ptr = NULL or int *ptr = nullptr; // nullptr是c++11中引入的
- void* 是一种没有类型的指针，只能存放地址，并不能直接操作内存空间中的对象

#### 声明和定义
- 为什么要区分两者：因为变量只能定义一次，而其他文件使用时不能定义，只能声明。
- 定义为变量分配存储空间
- 定义是声明

```
int a; // 定义也声明
/* 另外一个文件也需要使用a的时候，但不能直接使用，需要声明 */
extern int a; // 仅声明

/* 在一个文件中使用extern关键字，且进行了初始化，此时是定义 */
extern int a = 5; // 定义
```

#### 代码段 静态区 数据段 ？什么类型数据放在什么地方 ？

#### GCC的优化级别

- gcc中指定优化级别的参数有：-O0、-O1、-O2、-O3、-Og、-Os、-Ofast。

1. 在编译时，如果没有指定上面的任何优化参数，则默认为 -O0，即没有优化。

2. 参数 -O1、-O2、-O3 中，随着数字变大，代码的优化程度也越高，不过这在某种意义上来说，也是以牺牲程序的可调试性为代价的。

3. 参数 -Og 是在 -O1 的基础上，去掉了那些影响调试的优化，所以如果最终是为了调试程序，可以使用这个参数。不过光有这个参数也是不行的，这个参数只是告诉编译器，编译后的代码不要影响调试，但调试信息的生成还是靠 -g 参数的。

4. 参数 -Os 是在 -O2 的基础上，去掉了那些会导致最终可执行程序增大的优化，如果想要更小的可执行程序，可选择这个参数。

5. 参数 -Ofast 是在 -O3 的基础上，添加了一些非常规优化，这些优化是通过打破一些国际标准（比如一些数学函数的实现标准）来实现的，所以一般不推荐使用该参数。

### 面向对象程序设计
面向对象程序设计（OOP）的核心思想是**数据抽象**（类），**继承**（虚函数），**动态绑定**（虚函数指针？）
#### 虚函数

#### this指针
- 成员函数通过一个名为**this**的额外隐式参数来访问调用他的那个对象。
- 当调用一个成员函数时，用请求该函数的对象地址初始化this，然后调用成员函数，成员函数访问成员变量时，会隐式使用this指针
- 什么时候需要显式地使用this
  - 需要在成员函数里面明确地使用自己的时候，如实现对象的链式引用

### 数据结构 STL

#### vector的实现
- 标准库里的容器如vector的定义都使用到了泛型编程，使用了**类模板**。
- 实现
  - 使用了三个指针分别表示开辟连续空间首尾位置，和当前使用到的位置（指针其实是迭代器）
  - 通过三个指针可以实现容器大小，使用大小，是否为空，是否为满
  - 当新增一个元素时
    - 尾部插入且当前容器还有空间，直接将元素插入到当前使用到的指针位置，指针++
    - 中间插入且当前容器还有空间，将当前位置和后面位置的元素后移，然后将当前元素放在指定位置
    - 如果当前容器已经使用完了空间，会进行动态扩容
      - 首先申请一个更大的内存空间（gcc的实现是扩容一倍）
      - 然后把旧内存空间的元素都copy到新的内存空间
      - 释放掉原来的内存空间
  - 当删除一个元素时（为什么pop_back调用destory，而erase不调用？）
    - // https://cplusplus.com/reference/vector/vector/pop_back/
    - // https://www.cnblogs.com/sfwtoms/p/3931330.html 
    - // https://segmentfault.com/a/1190000040103598
    - 如果在尾部删除一个元素，销毁掉对象，并将使用结束指针--
    - 如果在中间部位删除一个元素，则将后边的元素前移，并将使用结束指针--


#### destory & delete
destory释放对象内的动态内存，即调用对象的析构函数
delete释放对象内存前也会调用对象的析构函数（如果有）

#### 泛型编程
泛型编程出现就是为了实现C++的STL（标准模板库），其本质是参数化类型，编写类型无关的代码。
在编译阶段，编译器会实例化出一个模板函数或者模板类。
模板是泛型编程的基础

#### 迭代器
- 迭代器是C++标准库容器提供的标准遍历方式
- 可以通过容器的begin() / end() 函数进行获取首尾迭代器
- 语法上使用像指针提供++/--操作进行左右移动，通过*（解引用符）获取值和修改值
- 可以使用for（auto it: vec）语句避免显式地使用迭代器
  

### lambda表达式


#### 数组 vs 指针
数组和指针关联紧密的原因是：编译器会把数组的行为转换成指针行为
- 区别
  - **概念上两者不同**，类型不同，相对应的赋值**操作**，取值操作都不一样
  - sizeof(指针)得到指针本身大小  sizeof(数组)得到数组整体大小
- 数组作为函数参数时，会退化为指向数组收个元素的地址的指针



### 拷贝控制操作
当定义一个类时，显式或隐式的指令此类型的对象拷贝、移动、赋值和销毁时做什么
#### 内存分配和管理

#### malloc、free

#### std::move

#### 深copy和浅copy



#### NULL 和 nullptr区别
NULL来自C语言，由宏定义实现，而nullptr是C++11的新增关键字
在C语言中，NULL被定义为(void*)0, 而在C++语言中，NULL则被定义为整数0。
- 为什么？
- C语言中常常把指针赋值为NULL， int \*a = (void\*)0 没有语义和没有语法错误
- C++中如果执行 int \*a = (void\*)0 会编译报错，因为不允许隐式的指令类型转换
但是因为这样c++对null的类型定义成了0，但这不对，所以要引入nullptr，试想一个下面的例子
一个可以重载的函数，其参数一个为接受一个指针类型，一个接收一个整型，传入NULL时会调用整型的重载函数，这样是不对的









