<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Effective C++](#effective-c)
  - [1. 让自己习惯C++](#1-%E8%AE%A9%E8%87%AA%E5%B7%B1%E4%B9%A0%E6%83%AFc)
    - [01. 视C++为一个语言联邦](#01-%E8%A7%86c%E4%B8%BA%E4%B8%80%E4%B8%AA%E8%AF%AD%E8%A8%80%E8%81%94%E9%82%A6)
    - [02. 尽量以 const, enum, inline 替换 #define](#02-%E5%B0%BD%E9%87%8F%E4%BB%A5-const-enum-inline-%E6%9B%BF%E6%8D%A2-define)
    - [03. 尽可能使用 const](#03-%E5%B0%BD%E5%8F%AF%E8%83%BD%E4%BD%BF%E7%94%A8-const)
    - [04. 确定对象被使用前已先被初始化](#04-%E7%A1%AE%E5%AE%9A%E5%AF%B9%E8%B1%A1%E8%A2%AB%E4%BD%BF%E7%94%A8%E5%89%8D%E5%B7%B2%E5%85%88%E8%A2%AB%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [2. 构造/析构/赋值运算](#2-%E6%9E%84%E9%80%A0%E6%9E%90%E6%9E%84%E8%B5%8B%E5%80%BC%E8%BF%90%E7%AE%97)
    - [05. 了解C++默默编写并调用哪些函数](#05-%E4%BA%86%E8%A7%A3c%E9%BB%98%E9%BB%98%E7%BC%96%E5%86%99%E5%B9%B6%E8%B0%83%E7%94%A8%E5%93%AA%E4%BA%9B%E5%87%BD%E6%95%B0)
    - [06. 若不想使用编译器自动生成的函数，就该明确拒绝](#06-%E8%8B%A5%E4%B8%8D%E6%83%B3%E4%BD%BF%E7%94%A8%E7%BC%96%E8%AF%91%E5%99%A8%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E7%9A%84%E5%87%BD%E6%95%B0%E5%B0%B1%E8%AF%A5%E6%98%8E%E7%A1%AE%E6%8B%92%E7%BB%9D)
    - [07. 为多态基类声明虚析构函数](#07-%E4%B8%BA%E5%A4%9A%E6%80%81%E5%9F%BA%E7%B1%BB%E5%A3%B0%E6%98%8E%E8%99%9A%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0)
    - [08. 析构函数不要抛出异常](#08-%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0%E4%B8%8D%E8%A6%81%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8)
    - [09. 绝不在构造和析构过程中调用虚函数](#09-%E7%BB%9D%E4%B8%8D%E5%9C%A8%E6%9E%84%E9%80%A0%E5%92%8C%E6%9E%90%E6%9E%84%E8%BF%87%E7%A8%8B%E4%B8%AD%E8%B0%83%E7%94%A8%E8%99%9A%E5%87%BD%E6%95%B0)
    - [10. 令一个 operator= 返回一个 reference to *this](#10-%E4%BB%A4%E4%B8%80%E4%B8%AA-operator-%E8%BF%94%E5%9B%9E%E4%B8%80%E4%B8%AA-reference-to-this)
    - [11. 在 operator= 中处理 “自我赋值”](#11-%E5%9C%A8-operator-%E4%B8%AD%E5%A4%84%E7%90%86-%E8%87%AA%E6%88%91%E8%B5%8B%E5%80%BC)
    - [12. 复制对象时勿忘其每一个成分](#12-%E5%A4%8D%E5%88%B6%E5%AF%B9%E8%B1%A1%E6%97%B6%E5%8B%BF%E5%BF%98%E5%85%B6%E6%AF%8F%E4%B8%80%E4%B8%AA%E6%88%90%E5%88%86)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Effective C++

## 1. 让自己习惯C++
### 01. 视C++为一个语言联邦
1. C++不仅仅是C加上面向对象特性。而且还有模板、异常、重载、STL等等。。。
2. C++并不是一个带有一组守则的一体语言，它是由四个次语言自称的联邦政府
    - C
    - Object-Oriented C++
    - Template C++
    - STL
---
### 02. 尽量以 const, enum, inline 替换 #define

     "宁可以编译器替换预处理器"
1. __使用 const 代替 #define__  
    ```cpp
    #define ASPECT_RATIO 1.653
    ```
    这个记号 ASPECT_RATIO 可能并未进入记号表，从而可能会得到编译错误。而且预处理器盲目地将"ASPECT_RATIO"替换为 1.653， 这可能会导致目标码(object code)出现多份 1.653
    
    所以可使用常量替换上边的宏：
    ```cpp
    const double AspectRatio = 1.653;
    ```

2. __使用 enum 代替 #define__
    ```cpp
    class GamePlayer{
    private:
        enum {NumTurns = 5};
        int scores[NumTurns];
    };
    ```
    enum 的行为比较像 #define 而不像 const，例如取一个 const 的地址是合法的，而取 enum 的地址不合法。enum 和 #define 一样不会导致非必要的内存分配。

3. __使用模板内联函数代替宏定义函数__
    ```cpp
    #define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b))
    ```
    就非常丑，而且在某些特殊情况下会有很麻烦的问题。
    ```cpp
    template<typename T>
    inline void callWithMax(const T& a, const T& b){
        f(a > b ? a : b);
    }
    ```
    这样的模板内联函数会更好，而且它遵守作用域和访问规则，因为是一个真正的函数。
---
### 03. 尽可能使用 const
1. __const 和一般对象与指针一起使用__  
 const 和指针一起使用有四种情况。只需要记住：__如果 const 出现在星号左边，表示被指对象是常量；const 出现在星号右边，表示指针自身是常量；同时出现在左右两边则都是常量。__
    ```cpp      
    char greeting[] = "Hello";
    char* p = greeting;         // 都不是常量
    const char* p = greeting;   // 被指对象是常量
    char* const p = greeting;   // 指针是常量
    const char* const p = greeting; // 都是常量
    ```

2. __const 和STL迭代器一起使用__  
    对于STL迭代器，如果声明迭代器为 const，就像声明指针为 const 一样，表明这个迭代器不得指向其他对象；而如果希望指明迭代器所指的对象不可改动，需要使用 const_iterator.

3. __const 和函数一起使用__  
    - 在一个函数声明式内，const 可以和函数返回值、各个参数、函数自身(如果函数是成员函数)产生关联。
        - 将函数返回值声明为 const，可以使其不会再被赋值。
        - const 参数，在不打算修改传入的值时要把形参声明为 const。
        - const 成员函数，该函数不可以修改自身对象的任何 non-static 成员变量。

4. 使用 const 的目的在于让编译器帮助测试出错误用法。

---
### 04. 确定对象被使用前已先被初始化

1. 读取未初始化的值会导致不明确的行为。对内置类型，需要手动完成初始化；对于内置类型以外的对象，由构造函数完成初始化。

2. 用构造函数对成员进行初始化时，要使用成员初始化器而不是在构造函数体中进行初始化。这样可以避免二次初始化，而且效率更高。为了不用去特意记住哪些变量已被初始化哪些未被初始化，可以规定总是在初始化器中初始化所有的成员变量。

3. 对于不同编译单元内的 __非局部静态变量(全局变量)__，它们的初始化顺序是不确定的，因此是很危险的。 所以，可以将每个非局部静态变量搬到自己的专属函数内进行初始化(在函数内声明为static)，这些函数返回一个指向该局部静态变量的引用。然后调用这样的函数，而不是直接去访问对象本身(因为直接访问对象本身，该对象可能还未被初始化)。这也是 __单例模式__ 的一个常见实现手法。 但是这种函数"内含 static 对象"的情况在多线程系统中是违反线程安全的，解决办法是可以在程序单线程启动阶段手动调用一遍所有这种返回局部静态变量引用的函数。


---
## 2. 构造/析构/赋值运算

### 05. 了解C++默默编写并调用哪些函数

1. 当写下：
    ```cpp
    class Empty { };
    ```
    就好像写下这样的代码：
    ```cpp
    class Empty{
    public:
        Empty() { ... }                            // 默认构造函数
        Empty(const Empty& rhs)                    // 拷贝构造函数
        ~Empty() { ... }                           // 析构函数

        Empty& operator=(const Empty& rhs){ ... }  // 重载的 = 运算符
    };
    ```
    其余部分都是编译器默认实现的，而且是 inline 的，而且只有当它们被调用时才会被编译器创建。 
    
2. 编译器产生的虚构函数是非虚函数，除非这个类的基类将析构函数声明为虚函数。 

3. 对于拷贝构造函数和重载的赋值运算符，编译器只是将源对象的每一个非静态成员变量拷贝到目标对象。

4. 当显示地提供了构造函数后，编译器就不会再创建默认构造函数，因此无需担心编译器的版本覆盖掉自定义的版本。

5. 对内含引用成员的类对象的赋值操作，以及内含 const 成员的类对象的赋值操作，编译器会拒绝生成默认的重载 = 运算符函数，这样的行为需要自己定义。因为更改引用所指向的对象，和更改 const 对象，在C++中都是不合法的。

---
### 06. 若不想使用编译器自动生成的函数，就该明确拒绝

1. 如果不想支持拷贝操作和赋值操作，可以 __将拷贝构造函数和重载赋值运算符函数声明为 private 函数，并且不去实现__。 这样可以避免编译器自动生成默认的函数，而且如果成员函数或友元函数尝试调用这样的函数，连接器会报错(因为只有声明没有定义)。 例如：
    ```cpp
    class HomeForSale{
    public:
        ...
    private:
        HomeForSale(const HomeForSale& );               // 只有函数声明
        HomeForSale& operator=(const HomeForSale& );
    };
    ```

2. 还有一种方法是，声明一个基类，然后将想要实现上述功能的类继承自该基类。 这样的好处一个是重用，再一个是可以把连接器的错误转化成编译时错误。
    ```cpp
    class Uncopyable{
    protected:
        Uncopyable() {}                 // 允许派生类对象对该基类进行构造和析构
        ~Uncopyable() {}
    private:
        Uncopyable(const Uncopyable&);  // 但是不允许复制操作
        Uncopyable& operator=(const Uncopyable&)
    };l
    ```
    ```cpp
    // 我们的类只需要 private 继承 Uncopyable 即可
    class HomeForSale: private Uncopyable{
        ...
    }
    ```
    当任何人，包括成员函数和友元函数尝试调用 HomeForSale 的拷贝构造函数和赋值运算符函数时，编译器生成的 HomeForSale 的对应函数会尝试调用基类的对应函数，而基类中已声明为 private，从而会导致编译错误。
---
### 07. 为多态基类声明虚析构函数

1. 假设 __工厂（factory）函数__，返回一个基类指针，指向一个派生类对象，派生类对象在堆中。为了避免内存泄漏，在用完这个指针后，需要将其 delete。

2. 这样做的问题在于：  
    - 依赖客户来做内存释放，客户本身可能会忘，不靠谱
    - 而且，返回的是 __指向派生类对象的基类指针__，而 delete 时，如果基类析构函数不是虚函数，则只会调用基类的析构函数而不是派生类的析构函数。从而产生未定义行为（通常是基类部分对象被析构，而派生类的成员未被释放）。

3. 对于不会作为基类的类，声明虚析构函数会导致其占用空间增加（因为要维护虚函数表和指向虚函数表的指针）。一般 __只有当类内含有至少一个虚函数时，才将其析构函数声明为 virtual__

4. 所有 STL 的容器都不含虚析构函数，所以自定义类继承自这些类时，都有可能会出现上述问题。

---
### 08. 析构函数不要抛出异常

1. __C++ 不鼓励析构函数抛出异常__ 因为当释放一个 vector\<Obj\> 或 array\<Obj\> 时，中间可能会抛出两个或以上的异常，这会导致未定义行为。

2. 解决方法，可以用类似 为智能指针自定义删除器 的方法，嵌套一个用于管理资源的类：  
    ```cpp
    class Connection{
    public:
        static Connection create();     // 返回 Connection 对象

        void close();                   // 关闭 Connection，可以抛出异常
    }

    class Conn{                         // 资源管理类
    public:
        void close(){
            con.close();
            closed = true;
        }

        ~Conn(){
            if(!closed){
                try{
                    con.close();
                }catch (...){
                    std::abort();       // 一旦 close 抛出异常则结束程序
                }
            }
            
        }
    private:
        Connection con;
        bool closed;
    }
    ```
    Conn类自己提供了一个 close() 函数，用来给客户一个处理异常的机会。  
    就算客户不想用这个功能，可以直接调用 Conn 类的析构函数或者忽略(等自动析构)，一旦在析构时，close()函数抛出异常，可以选择结束程序（abort） 或 将异常吞掉。

---
### 09. 绝不在构造和析构过程中调用虚函数

1. 因为如果基类的构造函数中调用了虚函数，而派生类对象在构造时，会先调用基类的构造函数（基类构造函数调用早于派生类构造函数），这时基类构造函数调用的虚函数，执行的是基类自身的版本，而不是派生类中 overried 的版本。所以就算它是虚函数，调用的也还是基类版本。换句话说，这时虚函数其实就不是虚函数了。

2. 原因在于，派生类调用基类构造函数时，这个对象相当于是一个基类对象，只能访问基类的成员变量，所以只能调用基类版本的虚函数。

3. 对于析构函数也是一样，因为当执行到基类的析构函数时，派生类的部分已经被销毁了（派生类析构函数调用早于基类析构函数），所以析构函数中的虚函数调用时，调用的仍然是基类的版本。

4. 解决方法在于，可以从派生类将一些信息传递给基类的构造函数，而不是调用虚函数。

---
### 10. 令一个 operator= 返回一个 reference to *this

1. 这样做主要是解决连锁赋值的问题，如
    ```cpp
    int x, y, z;
    x = y = z = 10;
    ```
    重载形式：
    ```cpp
    Widget& operator=(const Widget& rhs){    // 注意这里返回的是引用
        ...  // 一系列赋值操作
        return *this;
    }
    ```
    这里返回左手边的引用，从而可以实现连续赋值。

2. 只是一个协议，让重载后 = 的行为跟内置 = 的行为一致，不算是一个 bug. 当然 +=, -= 也要这样做。


---
### 11. 在 operator= 中处理 “自我赋值”

1. 有时候程序会存在潜在的自我赋值行为。 例如 a[i] = a[j]， *px = *py，等。而潜在的问题在于：
    ```cpp
    Widget& operator=(const Widget& rhs){
        delete ptr;
        ptr = new Obj(*(rhs.ptr));
        return *this.
    }
    ```
    当 rhs 与当前对象是同一个对象时，就会gg。所以需要加以判断：
    ```cpp
    if(this == &rhs)                // 这里的检测是直接对对象地址进行检测
        return *this;       
    ```

2. 更进一步的说，上边的代码仍然不能保证“异常安全性”，因为 new Obj()；调用可能导致异常。这里可以使用所谓的 __copy and swap__ 技术。下面是一个好的 operator= 的实现：
    ```cpp
    void swap(Widget& rhs);         // 交换*this 和 rhs 的数据

    Widget& operator=(const Widget& rhs){
        Widget temp(rhs);           // 拷贝构造函数，将 rhs 数据制作一份副本
        swap(temp);                 // 将副本数据拷贝到自身    
        return *this;
    }
    ```

---
### 12. 复制对象时勿忘其每一个成分



