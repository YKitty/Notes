# nullptr：指针空值

- [来源](#来源)
- [nullptr](#nullptr)
- [nullptr_t](#nullptr_t)
- [nullptr的规则](#nullptr的规则)

--------------------

## 来源

在原先C/C++98时，在进行初始化时，都会使用 **NULL** 来进行初始化。用来将指针指向一个空的位置。

但是这样初始化会导致二义性的问题。

传统的C头文件( **stddef.h** )中可以看到 **NULL的宏定义** 

``` C++
#undef NULL
#if defined(_cplusplus)
#define NULL 0
#else
#define NULL ((void*)0)
#endif
```

可以从上面看到NULL可能被定义为常量0，或者是定义为无类型指针（void*）常量。

这样就会导致在使用时就会导致二义性的问题。

举例：

``` C++
/******************************    
   * nullptr：指针空值，仅仅只是对于一个空指针，可以隐式类型转换为要赋值的指针类型    
   * NULL：0 or ((void*)0)    
   * 常量0 or 无类型指针    
   *****************************/     
  void Func(int i)     
  {    
      std::cout << i << "FuncInt" << std::endl;    
  }    

  void Func(void* p)    
  {    
      std::cout << p << "FuncPoint" << std::endl;    
  }    

  int main()    
  {    
      Func(0);    
      //产生二义性问题，不知道调用上面重载的哪一个函数    
      Func(NULL);    
      Func(nullptr);                                                                          
      return 0;    
  }    
```

对于例子中就可以看到当使用NULL就会发生 **二义性问题** 。

对于该问题主要就是常量0二义性的问题，只需要将其改进即可。出于 **兼容性** 的思考，C++11就没有消除NULL，而是新增加了 **nullptr(指针空值)** ，专门用来表示空指针。

## nullptr

nullptr：指针空值

指针空值类型被命名为 **nullptr_t** 。我们可以在 **cstddef** 头文件中可以看到如下定义：

```
typedef decltype(nullptr) nullptr_t;
```

我们可以看到nullptr_t定义的方式非常不一样，与传统的先定义类型，在通过类型声明变量的做法完全相反。充分利用了[decltype]()的功能。在编译时期，推导出其类型，用来定义nullptr。

优点：

1. nullptr是有类型的，并且仅可以 **隐式类型转换为指针类型** 。
2. 对于C++11，使用nullptr使代码的 **健壮性增强** 

那么对于上述代码，使用nullptr组参数就会直接调用参数为指针的函数。

## nullptr_t

**nullptr_t表示指针空值类型，是一种内置类型**

nullptr_t类型的规则：

1. 所有定义为nullptr_t类型的数据都是 **等价的**
2. nullptr_t类型的数据可以 **隐式类型转化为任何一个指针类型**
3. nullptr_t类型的数据不能被 **强制转换为非指针类型** 。即使使用reinterpre <int> (nullptr)的方式，也是无法实现的
4. nullptr_t类型的数据不能使用 **算术表达式**
5. nullptr_t类型数据可以使用 **比较表达式** 。但是只能与指针类型的数据进行比较

``` c++
int main()
{
    nullptr_t nul;
    if (nul == nullptr)
        std::cout << "nullptr_t数据一致性" << std::endl; 

    //隐式类型转换
    int* ip = nullptr;
    char* cp = nullptr;
    float* fp = nullptr;
    
    //算术运算：无法通过编译
    //nullptr + 1;
    //nullptr * 5;
    
    //不能强制类型转换为非指针类型
    //编译出错，不能进行类型转换
    //int i  = nullptr;
    //int j = reinterpret_cast<int>(nullptr);
    
    //关系运算 
    if (nul == nullptr)
        std::cout << "nul==nullptr" << std::endl;
    
    std::cout << sizeof(nullptr) << std::endl; 
    //不能访问type_info中的信息，这是不完整的
    //typedef (decltype)(nullptr) nullptr_t
    //这是在使用nullptr才使用decltype定义nullptr_t，RTTI(运行时类型识别)
    typeid(nullptr);

    return 0;
}
```

**【注意】：nullptr_t看起来是一个指针类型，用起来更是，但是在吧nullptr_t应用于模板时，我们会发现模板却只能将其当作一个普通的类型来进行推导(并不会将其看作是T\* 指针)**

``` c++
/******************************
 * nullptr_t应用于模板
 *****************************/ 

template<typename T>
void g(T* t){}

template<typename T>
void h(T t){}

int main()
{
    g(nullptr); // 编译失败，nullptr的类型是nullptr_t而不是指针
    g((float*)nullptr); // 推导出T = float 

    h(0); // 推导出 T = int
    h(nullptr); // 推导出 T = nullptr_t 
    h((float*)nullptr); // 推导出 T = float*
    return 0;
}
```

对于上述g(nullptr)不能被编译器推导出某种基本类型的指针（或void*指针），因此要让编译器成功推导出nullptr的类型，必须要做显式的类型转换

## nullptr的规则

nullptr类型数据所占用的内存空间大小跟void*相同的。

即：sizeof(nullptr_t) == sizeof(void*)

两者都可以别转化为任何类型的指针，但两者在语法层面有着不同的内涵。

nullptr是编译时期的常量，它的名字是编译时期的一个关键字，能够为编译器所识别。

而 (void* )0 只是一个强制类型转换表达式，其返回的也是一个 void* 指针类型

C++中，nullptr到任何指针的转换都是隐式的，而(void* )0 则是必须经过类型转换之后才能使用。

 **注意：C语言标准中，(void \*) 指针是可以隐式转换为任意指针的，这一点跟C++是不用的**

**C++标准中规定，nullptr_t对象的地址可以被用户使用(虽然看起来好像没有实用价值)** ，这一条规则有着例外，就是虽然nullptr也是一个nullptr_t的对象，C++11标准却规定用户 **不能获得nullptr的地址。** 

原因：nullptr被定义为一个 **右值常量**

C++11标准没有禁止声明一个nullptr的右值引用，并打印其地址。因此可以做一个实验。

``` c++
#include <iostream>
#include <cstddef>  // 使用nullptr_t

/******************************
 * nullptr_t对象取地址
 * nullptr不能取地址
 *****************************/ 
int main()
{
    nullptr_t my_null;
    printf("%x\n", &my_null);
    //printf("%x\n", &nullptr); // 无法编译通过
    printf("%d\n", my_null == nullptr);
    const nullptr_t&& default_nullptr = nullptr; // 右值引用
    printf("%x\n", &default_nullptr);
    return 0;
} 
/******************运行结果************************/
/* 
 * 56c25338
 * 1
 * 56c25340
 */ 
```

【注意】： [本文代码](https://github.com/YKitty/Code/tree/master/C%2B%2BCode/C%2B%2B11/nullptr  )

参考资料：深入理解C++11（书籍）