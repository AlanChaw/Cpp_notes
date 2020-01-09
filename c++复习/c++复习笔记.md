<!-- START doctoc generated TOC please keep comment here to allow auto update -->

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
  - [3.3 多维array对象](#33-%E5%A4%9A%E7%BB%B4array%E5%AF%B9%E8%B1%A1)
  - [3.4 vector](#34-vector)
- [4. 指针](#4-%E6%8C%87%E9%92%88)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# 1. 类、对象、字符串  

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

- __explicit关键字__：用来修饰构造函数，只能用于单个参数的构造函数，被修饰的类不能发生隐式类型转换吗，只能做显式类型转换。

2. 头文件中不应包含using声明，要使用std::， 但是cpp文件中可以

---

# 2. 函数和递归
## 2.1 函数原型和实参类型的强制转换  

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

## 2.2 存储类别和存储期
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
## 2.3 作用域规则
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
## 2.4 内联函数
1. __内联函数(inline function)__:  
    如果一个函数是内联的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方。对内联函数进行任何修改，都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧的函数。如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 inline，在调用函数之前需要对函数进行定义。如果已定义的函数多于一行，编译器会忽略 inline 限定符。

---
## 2.5 引用和引用形参

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
## 2.6 默认实参

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
## 2.7 函数重载  
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
## 2.8 函数模板  
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

# 3. 类模板array和vector

## 3.1 array对象例子

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
## 3.2 C++11 基于范围的for语句
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

2. array对象的排序与查找  
    - 标准库sort()和binary_search()函数, 在`<algorithm`>头文件中
    ```cpp
    array<string, ARRAY_SIZE> colors = {"red", "orange", "yellow",
        "green", "blue", "indigo", "violet"
    };

    sort(colors.begin(), colors.end());
    bool found = binary_search(colors.begin(), colors.end(), "indgo");
    ```

    - 标准库容器的begin()和end()成员函数属于对应类的成员，返回的是对象容器的首尾迭代器
---
## 3.3 多维array对象

1. 例子，二维数组的声明和打印
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
---
## 3.4 vector
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
# 4. 指针



