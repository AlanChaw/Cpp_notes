
## Effective C++


### 1. 视C++为一个语言联邦
1. C++不仅仅是C加上面向对象特性。而且还有模板、异常、重载、STL等等。。。
2. C++并不是一个带有一组守则的一体语言，它是由四个次语言自称的联邦政府
    - C
    - Object-Oriented C++
    - Template C++
    - STL
---
### 2. 尽量以 const, enum, inline 替换 #define

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
    }
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
### 3. 尽可能使用 const
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
### 4. 确定对象被使用前已先被初始化

1. 读取未初始化的值会导致不明确的行为。对内置类型，需要手动完成初始化；对于内置类型以外的对象，由构造函数完成初始化。

2. 用构造函数对成员进行初始化时，要使用成员初始化器而不是在构造函数体中进行初始化。这样可以避免二次初始化，而且效率更高。为了不用去特意记住哪些变量已被初始化哪些未被初始化，可以规定总是在初始化器中初始化所有的成员变量。

3. 

