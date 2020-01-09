<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [1. 类、对象、字符串](#1-%E7%B1%BB%E5%AF%B9%E8%B1%A1%E5%AD%97%E7%AC%A6%E4%B8%B2)
- [2. 函数和递归](#2-%E5%87%BD%E6%95%B0%E5%92%8C%E9%80%92%E5%BD%92)
  - [2.1 函数原型和实参类型的强制转换](#21-%E5%87%BD%E6%95%B0%E5%8E%9F%E5%9E%8B%E5%92%8C%E5%AE%9E%E5%8F%82%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%BC%BA%E5%88%B6%E8%BD%AC%E6%8D%A2)
  - [2.2 存储类别和存储期](#22-%E5%AD%98%E5%82%A8%E7%B1%BB%E5%88%AB%E5%92%8C%E5%AD%98%E5%82%A8%E6%9C%9F)
  - [2.3 作用域规则](#23-%E4%BD%9C%E7%94%A8%E5%9F%9F%E8%A7%84%E5%88%99)
  - [2.4 内联函数](#24-%E5%86%85%E8%81%94%E5%87%BD%E6%95%B0)
  - [2.5 引用和引用形参](#25-%E5%BC%95%E7%94%A8%E5%92%8C%E5%BC%95%E7%94%A8%E5%BD%A2%E5%8F%82)
  - [2.6 默认实参](#26-%E9%BB%98%E8%AE%A4%E5%AE%9E%E5%8F%82)
  - [2.7 函数重载](#27-%E5%87%BD%E6%95%B0%E9%87%8D%E8%BD%BD)
  - [2.8 函数模板](#28-%E5%87%BD%E6%95%B0%E6%A8%A1%E6%9D%BF)
- [3. 类模板array和vector](#3-%E7%B1%BB%E6%A8%A1%E6%9D%BFarray%E5%92%8Cvector)
  - [3.1 array对象例子](#31-array%E5%AF%B9%E8%B1%A1%E4%BE%8B%E5%AD%90)
  - [3.2 C++11 基于范围的for语句](#32-c11-%E5%9F%BA%E4%BA%8E%E8%8C%83%E5%9B%B4%E7%9A%84for%E8%AF%AD%E5%8F%A5)
  - [3.3 vector](#33-vector)
- [4. 指针](#4-%E6%8C%87%E9%92%88)
  - [4.1 使用指针的按引用传递](#41-%E4%BD%BF%E7%94%A8%E6%8C%87%E9%92%88%E7%9A%84%E6%8C%89%E5%BC%95%E7%94%A8%E4%BC%A0%E9%80%92)
  - [4.2 内置数组和指针](#42-%E5%86%85%E7%BD%AE%E6%95%B0%E7%BB%84%E5%92%8C%E6%8C%87%E9%92%88)
  - [4.3 sizeof 运算符](#43-sizeof-%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [4.4 指针表达式和算术运算](#44-%E6%8C%87%E9%92%88%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%92%8C%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97)
  - [4.5 基于指针的字符串(C风格)](#45-%E5%9F%BA%E4%BA%8E%E6%8C%87%E9%92%88%E7%9A%84%E5%AD%97%E7%AC%A6%E4%B8%B2c%E9%A3%8E%E6%A0%BC)
  - [其他](#%E5%85%B6%E4%BB%96)
- [5. 类](#5-%E7%B1%BB)
  - [5.1 实例研究](#51-%E5%AE%9E%E4%BE%8B%E7%A0%94%E7%A9%B6)
  - [5.2 C++11 委托构造函数](#52-c11-%E5%A7%94%E6%89%98%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
  - [5.3 析构函数](#53-%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0)
  - [5.4 不同作用域下构造函数和析构函数的调用时机](#54-%E4%B8%8D%E5%90%8C%E4%BD%9C%E7%94%A8%E5%9F%9F%E4%B8%8B%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E5%92%8C%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0%E7%9A%84%E8%B0%83%E7%94%A8%E6%97%B6%E6%9C%BA)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## 1. 类、对象、字符串  

1. 创建一个GradeBook类(一个成员函数)并打印信息

    ```cpp
    #include <iostream>
    #include <string>
    using namespace std;

    class GradeBook{
    public:
        explicit GradeBook(string name)
            : courseName(name){
        }
        
        void setCourseName(string name){
            courseName = name;
        }
        
        string getCourseName() const{
            return courseName;
        }
        
        void displayMessage() const{
            cout << "Welcome to the grade book of "
            << getCourseName() << endl;
        }
    private:
        string courseName;
    };

    int main(int argc, const char * argv[]) {
        string courseName;
        cout << "Please enter the course name: " << endl;
        getline(cin, courseName);  // read name with blanks;
        cout << endl;
        
        GradeBook myGradeBook(courseName);
        myGradeBook.displayMessage();
        
        return 0;
    }

    ```

-  "endl" 和 "\n"  

    - std::cout << std::endl; 插入换行符并flush the burrer
    - std::cout << "\n; 只是插入换行符

-  __函数声明为const__，告诉编译器这个函数不能修改调用它的对象

- __explicit关键字__：用来修饰构造函数，被修饰的类不能发生隐式类型转换，只能做显式类型转换。

2. 头文件中不应包含using声明，要使用std::， 但是cpp文件中可以

---

## 2. 函数和递归
### 2.1 函数原型和实参类型的强制转换  

1. __函数原型(也成为函数声明)__ 告诉编译器函数的名称、函数返回数据的类型、函数预期接收的形参个数以及形参的类型和顺序。
    - 除非函数定义在其使用之前，否则要先声明函数原型。
    - 如果函数在调用前就已经定义，那么函数的定义也可作为函数原型，当然，好的编程习惯是总提供函数原型。
2. __函数签名__：  
    函数声明中的函数名和实参类型称为函数签名（不包含返回类型）。如果两个函数函数名相同，参数类型、参数个数都相同，只有返回类型不同，会被拒绝重载。 函数签名的例子:
    ```cpp
    maximum(int, int, int);
    ```

3. __实参类型强制转换__:  
    函数声明的一个重要特性是“实参类型强制转换“。例如：程序调用一个函数时可以使用整型实参，即使该函数原型指定的是double数据类型的形参，函数也还是会正常执行。
    ```cpp
    // 函数声明为：
    int maximum(double a, double b, double c);

    // 函数调用时, 其中 a, b, c 都为int类型
    int m = maximum(a, b, c);
    ```

4. __实参类型升级规则和隐式类型转换__:  
    实参的隐式类型转换发生时，int可以升级为double，double也可转换为int(但是会被截取)。在发生隐式类型转换时，如果把高级基本数据类型转换为低级，会产生不确定的值。

---

### 2.2 存储类别和存储期
1. 程序中标识符包括变量名和函数名等。实际上，每个标识符还有其他属性，包括 __存储类别、作用域、和链接(linkage)__ .
    - __存储类别:__ 共有5个存储类别说明符: auto、register、extern、 mutable、和static，它们决定了变量的存储期。  
    - __标识符的作用域__ 是指标识符在程序中可被引用的范围。
    - __标识符的链接__ 决定了标识符是只在声明它的源文件中可以识别，还是在经编译后链接在一起的多个文件中可以识别。
    - __标识符的存储期__ 决定了标识符在内存中存在的时间。按存储类别说明符可以划分出四个存储期：
        - 自动存储期
        - 静态存储期
        - 动态存储期
        - 线程存储期

2. 具有 __自动存储期__ 的变量包括：
    - 函数中的局部变量
    - 函数的形参
    - 用register声明的局部变量或函数形参，用 __register__ 声明的变量建议将该变量放入计算机的一个存储器中，register现在通常没有必要，现在的很多编译器可以自动识别经常使用的变量，并放到CPU存储器中。    
    
    (自动存储是节省内存的一个方法，也是最小特权原则的一个例子。)

3. __静态存储期__：
    - 关键字 __extern__ 和 __static__ 用于声明具有静态存储期的变量或函数。具有静态存储期的变量从程序开始执行一直到程序结束都一直存在于内存中，在程序执行遇到这样的变量声明时，便会对其进行一次性的初始化。
    - __extern__:  extern关键字实际上有两种用法，一种是用于声明全局变量或全局函数，例如：
        ```cpp
        extern int a;
        ```
        另外还有一种使用:
        ```cpp
        extern "C"{

            ...
        }
        ```
        用于告诉编译器这部分代码用C语言的方式进行编译和链接，目的是为了实现C++和C及其他语言的混合编程。
    
    - __static__: 用于声明静态局部变量。static声明的局部变量仅被其声明所在的函数所知。但是static局部变量在函数返回后仍保留原来的值。而且static在未显示初始化时会自动初始化为0. 类中的静态成员变量被该类所有对象所共享，也成为 __类变量__  

    
---
### 2.3 作用域规则
1. 作用域是对于标识符而言的，对程序中的标识符，程序中可使用该标识符的范围成为该标识符的作用域。作用域包括
    - 语句块作用域
    - 函数作用域
    - 全局命名空间作用域
    - 函数原型作用域

2. 一元作用域分辨运算符 (::)
    ```cpp
    #include <iostream>
    using namespace std;

    int number = 7;

    int main(int argc, const char * argv[]) {
        double number = 10.5;
        
        cout << "local variable: " << number << endl;
        cout << "global variable: " << ::number << endl;
        
        return 0;
    }
    ```
    输出：
    ```cpp
    local variable: 10.5
    global variable: 7
    ```
    总是使用一元作用域分辨运算符来引用全局变量更为清晰。

---
### 2.4 内联函数
1. __内联函数(inline function)__:  
    如果一个函数是内联的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方。对内联函数进行任何修改，都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧的函数。如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 inline，在调用函数之前需要对函数进行定义。如果已定义的函数多于一行，编译器会忽略 inline 限定符。

---
### 2.5 引用和引用形参

1. __按值传递和按引用传递__ (pass by value, pass by reference):   
    - 按值传递：  
        ```cpp
        int x = 3;
        int result;
        result = squareByValue(x);

        int squareByValue(int number){
            return number * number;
        }
        ```
    - 按引用传递：
        ```cpp
        int x = 3;
        squareByReference(x);

        void squareByReference(int &numberRef){
            numberRef *= numberRef;
        }
        ```
        按引用传递性能较高，可以消除按值传递复制数据所产生的开销；  
        但是按引用传递会削弱安全性。  
        有时候，为了传递大型对象，要避免复制开销，但是又要保证安全性，可以使用const施加限定。

2. 函数内引用作为别名：  
    ```cpp
    int count = 1;
    int &cRef = count;
    cRef++;
    ```
    如果cRef被更改，则count也会被修改。这里&符号是作为引用的意思，不是取地址的意思。

3. 从函数中返回引用会导致 __虚悬引用__ 问题(Dangling reference)，因为该引用对应的变量在函数执行结束时已经被销毁。

---
### 2.6 默认实参

1. 当程序在函数调用中对具有默认实参的形参省略了其对应的实参时，编译器会自动重写这个函数并插入默认的实参值。

2. 使用默认实参的例子:

    ```cpp
    int boxVolume(int length = 1, int width = 1, int height = 1);

    int main(){
        cout << boxVolume() << endl;
        return 0;
    }

    // 在函数定义部分就没必要再写一遍默认实参了
    int boxVolume(int length, int width, int height){
        return length * width * height;
    }
    ```

---
### 2.7 函数重载  
1. C++允许定义多个具有相同名字的函数，只要这些函数具有不同的函数签名（名字相同，但是接受的形参的数量不同或类型不完全相同，返回值无所谓）。

2. 函数重载的例子:
    ```cpp
    #include <iostream>
    using namespace std;

    int square(int x){
        cout << "square of integer " << x << " is :";
        return x * x;
    }

    double square(double x){
        cout << "square of double " << x << " is :";
        return x * x;
    }

    int main(int argc, const char * argv[]) {
        cout << square(7) << endl;
        cout << square(7.5) << endl;
        
        return 0;
    }
    ```
    输出：
    ```
    square of integer 7 is :49
    square of double 7.5 is :56.25
    ```
---
### 2.8 函数模板  
1. 对于重载函数，如果对于每种数据类型程序逻辑和操作都是相同的，使用函数模板可以使重载执行起来更加紧凑和方便。在模板函数调用中提供了实参类型，C++就会自动生成独立的函数模板特化(function template specialization)来恰当地处理每种类型的调用。这样，一个函数模板实质上就定义了一整套的重载的函数。

2. 使用函数模板的例子:
    ```cpp
    #include <iostream>
    using namespace std;

    template <typename T>
    T maximum(T value1, T value2, T value3){
        T maxValue = value1;
        
        if (value2 > maxValue)
            maxValue = value2;
        if (value3 > maxValue)
            maxValue = value3;
        return maxValue;
    }


    int main(int argc, const char * argv[]) {
        int i = 1, j = 2, k = 3;
        cout << maximum(i, j, k) << endl;
        
        double m = 1.5, n = 2.5, p = 3.5;
        cout << maximum(m, n, p) << endl;
        
        return 0;
    }
    ```

3. C++11 __函数的尾随返回值类型__  
例如，为了指定函数模板maximum的尾随返回值类型，需进行如下书写:
    ```cpp
    template <typename T>
    auto maximum(T x, T y, T z) -> T
    ```
    具体是干啥的以后再写

---

## 3. 类模板array和vector

### 3.1 array对象例子

1. 求array对象元素之和
    ```cpp
    #include <iostream>
    #include <array>

    #define ARRAY_SIZE 5

    int main(int argc, const char * argv[]) {
        using namespace std;
        const size_t arraySize = ARRAY_SIZE;
        array<int, arraySize> a = {10, 20, 30, 40, 50};
        int total = 0;
        
        for(size_t i = 0; i < a.size(); i++)
            total += a[i];
        
        cout << "Total of array elements: " << total << endl;
        
        return 0;
    }
    ```

2. 对于数组，C++没有自动的边界检查机制。引用超出array对象边界的元素是一个执行时的逻辑错误，不是语法错误，也不会有警告信息。

3. static local array  
    对于函数中的大型array对象，可以用static来声明，这样在函数结束时该array不会被销毁，也不会在每次执行函数时初始化，可以提高性能。

---
### 3.2 C++11 基于范围的for语句
1. C++11 的新特性，提供了基于范围的for语句，相当于for..each 或者 for...in，例子：
    ```cpp
    #include <iostream>
    #include <array>

    #define ARRAY_SIZE 5

    int main(int argc, const char * argv[]) {
        using namespace std;
        
        array<int, ARRAY_SIZE> items = {1, 2, 3, 4, 5};

        cout << "items in array: ";
        for(int item: items)
            cout << item << " ";
        /* 等价于
        for (int i = 0; i < items.size(); i++)
            cout << items[i] << " ";
        */
        cout << endl;
        
        for(int &itemRef: items)  //若要修改，需要使用元素的引用
            itemRef *= 2;
        
        cout << "items after modification: ";
        for(int item: items)
            cout << item << " ";
        cout << endl;
        
        return 0;
    }
    ```
    输出：
    ```
    items in array: 1 2 3 4 5 
    items after modification: 2 4 6 8 10 
    ```

2. 多维array对象.  
    例子，二维数组的声明和打印
    ```cpp
    #include <iostream>
    #include <algorithm>
    #include <array>
    #include <string>

    using namespace std;

    #define N_ROWS 2
    #define N_COLS 3

    template<typename T>
    void printArray(const std::array<std::array<T, N_COLS>, N_ROWS> a){
        for(auto row: a){
            for(T element: row)
                cout << element << " ";
            cout << endl;
        }
        cout << endl;
    }

    int main(int argc, const char * argv[]) {
        
        array<array<int, N_COLS>, N_ROWS> array1 = {1, 2, 3, 4, 5, 6};
        printArray(array1);
        
        return 0;
    }
    ```

2. array对象的排序与查找
    - 标准库sort()和binary_search()函数, 在`<algorithm`>头文件中
    ```cpp
    array<string, ARRAY_SIZE> colors = {"red", "orange", "yellow",
        "green", "blue", "indigo", "violet"
    };

    sort(colors.begin(), colors.end());
    bool found = binary_search(colors.begin(), colors.end(), "indgo");
    ```

    - 标准库容器的begin()和end()成员函数属于对应类的成员，返回的是对象容器的首尾迭代器。这里的这些操作跟vector一样。

---
### 3.3 vector
1. 声明一维和二维向量
    ```cpp
    //一维
    int size = 10;
    vector<int> vec(size);
    //二维
    int row = 5, col = 10;
    vector<vector<int>> matrix(row, vector<int>(col));
    ```

2. 排序
    ```cpp
    sort(vec.begin(), vec.end(), static compare);
    ```
3. 切片
    ```cpp
    vector<int> v2 = vector<int>(v1.begin(), v1.begin()+10);
    ```
4. 获取最后一个元素
    ```cpp
    vec[vec.size()-1];  //直接返回引用
    vec.at()       //直接返回引用，跟用下标一样
    vec.back();    //返回指向最后一个元素的引用
    vec.end()-1;   //返回一个指向最后一个元素的迭代器
    vec.rbegin();  //返回反向迭代器
    ```
5. 删除最后一个元素 
    ```cpp
    vec.pop_back();
    ```

---
## 4. 指针

### 4.1 使用指针的按引用传递

1. 使用指针的按引用传递，实际上传的是一个地址，这个地址在函数中被赋值给形参(一个指针)，这个形参在函数结束时仍然会被销毁，本质上依然是按值传递。这也说明，在C++中，所有的参数其实都是按值传递的。 例子：

    ```cpp
    // 函数声明
    void cubeByReference(int * a);

    // 函数调用
    int number = 5;
    cubeByReference(&numeber);
    ``` 

2. 这其实也是C语言所支持的传递引用的方式。在C语言中不支持直接传引用，即 在函数声明中不允许出现&关键字.

---
### 4.2 内置数组和指针

1. 内置数组的声明和使用
    ```cpp
    // 几种声明方式
    int c[5];
    int c[5] = {1,2,3,4,5};
    int c[] = {1,2,3,4,5};

    // C++11特性，允许begin()和end()函数接收一个内置数组作为实参，返回一个迭代器
    // begin() 和 end() 在<iterator>中定义
    sort(begin(c), end(c));
    ```

2. 内置数组的局限性(跟C相同)
    - 不知道自身大小
    - 不提供越界检查
    - 不能作为对象相互赋值
    - 无法相互比较

3. 内置数组的名字的值可隐式地转换为这个数组第一个元素的地址。

4. 指针和内置数组的交换使用
    ```cpp
    int b[5];
    int *bPtr;

    // 下面两种操作等价
    bPtr = b;
    bPtr = &b[0];

    // 下面两种操作等价
    int c = *(bPtr + 3);
    int c = b[3];
    ```

4. 内置数组的名字实际上是一个const指针，不能做修改。 但指向数组的非const指针可以自由修改。

5. 内置数组总是以传引用的方式传给函数，其实就是因为数组名本身就相当于一个指向第一个元素的指针。而如果想避免数组的值在函数中被修改，应使用const修饰符。这里有将指针传给函数的几种方式：
    - 指向非const数据的const指针，有最大的访问权限，可以修改指针指向的数据，也可以修改指针指向其他数据。
    - 指向const数据的非const指针，指针本身可以被修改为指向其他数据，但是不能通过该指针修改其所指向的数据了。
    - 指向非const数据的const指针，这种指针始终指向内存中的固定位置，不能修改指向，但其指向的值可以修改。
    - 指向const数据的const指针，指针的指向无法修改，其所指向的值也无法修改。

---
### 4.3 sizeof 运算符

1. 使用sizeof()函数作用于一个内置数组类型，会返回这个数组整个所占的字节数，返回类型为size_t。但是如果使用sizeof()作用于一个内置数组在函数中的形参，实质上相当于sizeof()作用于了一个指针，返回值只能是4.

2. 用sizeof()求内置数组元素个数
    ```cpp
    sizeof(numbers) / sizeof(numbers[0]);
    ```

3. sizeof()也可以接受基本类型作为实参，返回该类型所占的字节大小。
    ```cpp
    sizeof(char);
    sizeof(int);
    ...
    ```

4. sizeof()也可以用于一个表达式，返回存储这个表达式所需的字节数，只有当sizeof类型名的时候才需要使用圆括号，作用于表达式或变量时不需要使用圆括号。 sizeof()是一个编译时运算符，它的操作数不会被求值，而是在编译时就算出整个操作数的大小。

---
### 4.4 指针表达式和算术运算

1. 指针可以自增(++)或自减(--)，相当于往前或往后挪动n个字节，n取决于指针指向的类型。如果指针指向数组中的某个值，指针自增1后将指向下一个元素(这里跟C是一样的)。 当然，指针也可以加或减一个整数。

2. 同类型的指针相减。如果p1包含地址3000，p2包含地址3008(两个int类型指针)，那么p2-p1的值为2.  不同类型指针相减是一个逻辑错误。

3. 同类型指针之间可以进行赋值操作。

4. void* 是一种通用指针，任何指向基本类型或类类型的指针都可以赋值给void* 类型的指针。但是反过来不行，需要强制类型转换。但是void* 指针不能被解引用，因为编译器不知道它所指向的数据类型占多少个字节。

5. 指针的比较只有当两个指针指向同一个内置数组中的元素时才有意义，比较的是两个指针对应元素的下标。
---

### 4.5 基于指针的字符串(C风格)

1. 基于指针的字符串是一个以空字符'\0'结尾的内置char数组，这个空字符标记了字符串在内存中结束的位置。

2. __注意：对一个内置char数组进行sizeof()运算得到的是包含'\0'在内的长度__。 而且用C语言内存分配方法(malloc)对char数组进行动态内存分配时，要考虑到末尾的'\0'。

3. 例子：
    ```cpp
    //以下两种写法等价
    char color[] = "blue";
    char color[] = {'b', 'l', 'u', 'e', '\0'};
    ```

4. 另外，cout和cin都支持作用于一个内置字符数组。

---
### 其他
1. C++11中，指针应初始化为nullptr，或者一个相应类型的地址。一个值为nullptr的指针”指向空“，被称为空指针。 而在早期版本中，空指针的值是0或NULL.


---
## 5. 类

### 5.1 实例研究

这一部分以Time类为例来展开研究。  
- Time类定义：
    ```cpp
    //  Time.h
    #ifndef TIME_H
    #define TIME_H

    class Time{
    public:
        Time();                         // 构造函数
        void setTime(int, int, int);    // 设置时分秒
        void printUniversal() const;    // 以universal格式打印
        void printStandard() const;     // 以standard格式打印
        
    private:
        int hour;
        int minute;
        int second;
    };

    #endif
    ```
- Time类实现：
    ```cpp
    //  Time.cpp
    #include <iostream>
    #include <iomanip>
    #include <stdexcept>
    #include "Time.h"

    using namespace std;

    Time::Time()
    : hour(0), minute(0), second(0){}

    void Time::setTime(int h, int m, int s){
        if((h >= 0 && h < 24) && (m >= 0 && m < 60)
            && (s >= 0 && s < 60)){
            hour = h;
            minute = m;
            second = s;
        }
        else{
            throw invalid_argument("parameter out of range");
        }
    }

    void Time::printUniversal() const{
        cout << setfill('0') << setw(2) << hour << ":"
        << setw(2) << minute << ":"
        << setw(2) << second << endl;
    }

    void Time::printStandard() const{
        cout << ((hour == 0 || hour == 12) ? 12 : hour % 12) << ":"
        << setfill('0') << setw(2) << minute << ":"
        << setw(2) << second << (hour < 12 ? " AM" : " PM") << endl;
    }
    ```

1. 在头文件中的预处理指令，#ifndef ... #endif，用于防止试图多次包含一个头文件导致错误。按照惯例，预处理指令应该使用大写的头文件名，并用下划线代替圆点。

2. Time构造函数使用 __类内初始化器(C++11)__ 将数据成员初始化为-，确保其能够以一个可靠的状态开始。这里构造函数不允许传任何参数，后续的赋值操作是由成员函数setTime()完成的。

3. 在setTime()的定义中对非法输入情况会抛出异常，这需要在调用setTime()时使用try...catch来捕获异常。这里的throw语句创建了一个名为invalid_argument的新对象，后边的括号是对这个对象的构造函数进行调用，这里允许指定一个用户自定义的错误信息字符串。这个throw会立即终止函数setTime()，然后异常会被返回到try的位置。
    ```cpp
    // 对setTime()抛出的异常进行捕获
    Time t;
    try{
        t.setTime(99, 99, 99);
    }catch(invalid_argument &e){
        cout << e.what() << endl;
    }
    ```

4. 在打印操作中，setfill()函数用于指定填充符，setw()用于设置宽度，但setfill()也将应用到后续的显示中，因此说它是一个”黏性“设置。而setw()仅会对其紧挨着的后一个值起作用。

5. 当成员函数的定义在类外部时，需要使用二元作用域分辨符将其”绑定“到该类，而在类内部的定义不需要。而且 __在类定义内部定义的成员函数会被隐式地声明为inline的__。利用这一特性，可以将简单和稳定的成员函数定义在类的内部来提高程序的性能。
---
### 5.2 C++11 委托构造函数
- 类的构造函数和成员函数一样可以被重载，如果重载了函数，需要在类的定义中为构造函数的各个版本提供独立的构造函数定义。
- 例如:
    ```cpp
    // Time.h 中的构造函数
    Time();
    Time(int);
    Time(int, int);
    Time(int, int, int);
    ```
    ```cpp
    // Time.cpp 中的函数定义
    Time::Time(int h, int m, int s)
        : hour(h), minute(m), second(s){}

    Time::Time()
        :Time(0, 0, 0){}
    
    Time::Time(int h)
        :Time(h, 0, 0)

    Time::Time(int h, int m)
        :Time(h, m, 0)
    ```

    在上边这个例子中，C++11允许构造函数调用同一个类的其他构造函数，相当于它把工作委托给其所调用的构造函数，这样的构造函数成为委托构造函数。(以前的处理方法是将重写的部分定义为一个单独的private函数)
---
### 5.3 析构函数
1. 析构函数的 ~ 符号本来是取补符号，这也象征着构造函数和析构函数是互补的。

2. 析构函数不接收任何参数，也不返回任何值。

3. 当对象撤销时，类的析构函数会隐式地调用。 __析构函数本身并不释放对象占用的内存空间__ ，它只是在系统回收对象的内存之前执行扫尾工作。

4. 每个类都有一个析构函数，如果程序员没有显式地提供析构函数，那么编译器会生成一个”空的“析构函数。
---
### 5.4 不同作用域下构造函数和析构函数的调用时机

1. 全局作用域的对象。
    - 在文件内任何其他函数(包括main函数)开始执行前调用
    - main函数执行结束时，相应的析构函数被调用

2. 局部对象的构造函数和析构函数
    - 当程序执行到自动局部对象的定义时，该对象的构造函数被调用
    - 当程序执行离开对象的作用域时，相应的析构函数被调用

3. static局部对象的构造函数和析构函数
    - static局部对象的构造函数只在程序第一次执行到该对象的定义处时被调用一次
    - 相应的析构函数调用发生在main函数结束时

