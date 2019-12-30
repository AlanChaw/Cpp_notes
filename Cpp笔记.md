
# Chapter 1-3

## Chapter 3 类、对象、字符串  

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

-  函数声明为const，告诉编译器这个函数不能修改调用它的对象

- explicit关键字：用来修饰构造函数，只能用于单个参数的构造函数，被修饰的类不能发生隐式类型转换吗，只能做显式类型转换。

2. 头文件中不应包含using声明，要使用std::， 但是cpp文件中可以

---

# Chapter 6 函数和递归

1. 函数签名：  
    函数声明中的函数名和实参类型称为函数签名（不包含返回类型）。如果两个函数函数名相同，参数类型、参数个数都相同，只有返回类型不同，会被拒绝重载。

2. 实参类型强制转换：  
    函数声明的一个重要特性是“实参类型强制转换“。例如：程序调用一个函数时可以使用整型实参，即使该函数原型指定的是double数据类型的形参，函数也还是会正常执行。

3. 实参类型升级规则和隐式类型转换:  
    实参的隐式类型转换发生时，int可以升级为double，double也可转换为int(但是会被截取)。在发生隐式类型转换时，如果把高级基本数据类型转换为低级，会产生不确定的值。

4. 内联函数(inline function):  
    如果一个函数是内联的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方。对内联函数进行任何修改，都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧的函数。如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 inline，在调用函数之前需要对函数进行定义。如果已定义的函数多于一行，编译器会忽略 inline 限定符。

5. 按值传递和按引用传递。(pass by value, pass by reference):   
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

6. 函数内引用作为别名：  
    ```cpp
    int count = 1;
    int &cRef = count;
    cRef++;
    ```
    如果cRef被更改，则count也会被修改。这里&符号是作为引用的意思，不是取地址的意思。

7. 从函数中返回引用会导致 __虚悬引用__ 问题(Dangling reference)，因为该引用对应的变量在函数执行结束时已经被销毁。

8. 一元作用域分辨运算符 (::)
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
    总是使用一元作用域分辨运算符来引用全局变量更为清晰。

9. 函数重载  
    C++允许定义多个具有相同名字的函数，只要这些函数具有不同的函数签名（名字相同，但是接受的形参的数量不同或类型不完全相同，返回值无所谓）。
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
10. 函数模板(function template)  
    对于重载函数，如果对于每种数据类型程序逻辑和操作都是相同的，使用函数模板可以使重载执行起来更加紧凑和方便。在模板函数调用中提供了实参类型，C++就会自动生成独立的函数模板特化(function template specialization)来恰当地处理每种类型的调用。这样，一个函数模板实质上就定义了一整套的重载的函数。
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

11. 递归(recursion)
    - 每个递归调用都会创建函数变量的一份副本，会占用相当可观的空间
    - 任何可以用递归解决的问题都可以用迭代解决

---

# Chapter 7 类模板array和vector,异常捕获

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

4. __基于范围的for语句(C++11)__  
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

5. 类中的静态成员变量被该类所有对象所共享，也成为 __类变量__  

6. array对象的排序与查找  
    - 标准库sort()和binary_search()函数, 在`<algorithm`>头文件中
    ```cpp
    array<string, ARRAY_SIZE> colors = {"red", "orange", "yellow",
        "green", "blue", "indigo", "violet"
    };

    sort(colors.begin(), colors.end());
    bool found = binary_search(colors.begin(), colors.end(), "indgo");
    ```

    - 标准库容器的begin()和end()成员函数属于对应类的成员，返回的是对象容器的首尾迭代器

7. 多维array对象
    - 注意a[x, y]被认为是a[y]
    - 例子，二维数组的声明和打印
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

8. Vector  
    - pass

---
# Chapter 8 指针

pass


---
# 其他笔记

1. C++的stl中的哈希表  
    STL中，map 对应的数据结构是 红黑树。红黑树是一种近似于平衡的二叉查找树，里面的数据是有序的。在红黑树上做查找操作的时间复杂度为 O(logN)。而 unordered_map 对应 哈希表，哈希表的特点就是查找效率高，时间复杂度为常数级别 O(1)， 而额外空间复杂度则要高出许多。所以对于需要高效率查询的情况，使用 unordered_map 容器。而如果对内存大小比较敏感或者数据存储要求有序的话，则可以用 map 容器。

    ```cpp
    #include <iostream>
    #include <unordered_map>
    #include <string>
    int main(int argc, char **argv) {
        std::unordered_map<int, std::string> map;
        map.insert(std::make_pair(1, "Scala"));
        map.insert(std::make_pair(2, "Haskell"));
        map.insert(std::make_pair(3, "C++"));
        map.insert(std::make_pair(6, "Java"));
        map.insert(std::make_pair(14, "Erlang"));
        std::unordered_map<int, std::string>::iterator it;
        if ((it = map.find(6)) != map.end()) {
            std::cout << it->second << std::endl;
        }
        return 0;
    }
    ```

2. 用transform转换字符串大小写
    ```cpp
    transform(s.begin(), s.end(), s.begin(), ::tolower);
    ```

3.  ASCII 码范围
    - a-z：97-122
    - A-Z：65-90
    - 0-9：48-57

4. 排序函数 <algorithm>  
    - std::sort(vector.begin(), vector.end(), function/object)，第三个参数可以传函数指针，对象，或者什么都不传（默认的compare函数）  
    - 对数组：std::sort(a[0], a[10])

5. 堆 （优先队列） <queue>::priority_queue
    例子：找到数组中第k大的数 (leetcode 215)
    ```cpp
        priority_queue<int, vector<int>, greater<int>> store;
        //堆中维持k个最大数
        for (int i = 0; i < numsSize; i++)
        {
            store.push(nums[i]);
            if (int(store.size()) > k)
            {
                store.pop();
            }
        }

        result = store.top();
    ```
    使用greater<int> 为小顶堆，否则为大顶堆，或使用自己的比较函数


---

# STL库的一些常用容器和相关操作  

## \<string\>常用操作

1. 获取长度
    ```cpp
    str.length()
    ```

2. 切片
    ```cpp
    str.substr(a, len);  //接受两个int，从a开始，切出共len个字符的子串
    ```

3. string和数值类型相互转换
    ```cpp
    int num = std::stoi(str);       //string -> int
    double dnum = std::stod(str);   //string -> double
    ```
    ```cpp
    string str = std::to_string(...)  // int,double... -> string
    ```



## \<vector\>常用操作
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
    

## \<unordered_map\>常用操作
1. 遍历
    ```cpp
    for (auto iter = groups.begin(); iter != groups.end(); iter++){
            auto key = iter->first;
            auto val = iter->second;
    }
    ```

2. 添加
    ```cpp
        map.insert(std::make_pair(key, val));
    ```

3. 修改
    ```cpp
        map[key] = newVal;  //最好是在确保该key存在的情况下再做此操作
    ```

4. 查找 （使用迭代器）
    ```cpp
    std::unordered_map<int, std::string>::iterator iter;
    if ((iter = map.find(key)) != map.end()) {
        std::cout << iter->second << std::endl;
    }

    //或
    if(map.count(key) == 0)
    ```
    或者使用at()，但是如果该key不存在会抛出OOR异常

5. 删除
    ```cpp
    map.erase(key);
    ```
    不要轻易使用map[key]，因为如果该Key不存在，会被自动创建。

## \<queue> 常用操作
1. 初始化
    ```cpp
    std::queue<int> first;            // empty queue
    std::queue<int> second (mydeck);  // queue initialized to copy of deque
    ```

2. 成员函数
    - empty()
    - size()
    - front()  访问队首元素
    - back()   访问队尾元素
    - push()
    - pop()

## \<deque> 双向队列，常用操作

1. 成员函数
    - at() 访问元素
    - push_front()
    - pop_front()
    - push_back()
    - pop_back()
    - front()   队首元素引用
    - back()    队尾元素引用
    - size()
    - empty()

## \<set> 和 \<unordered_set>

- 函数
    - insert()  插入
    - erase()   删除

- 区别在于set将数据有序存储，插入和查询的时间复杂度为O(logn)，一般是用红黑树实现
- 而unordered_set中数据是无序的，插入和查询时间复杂度为O(1)，跟map 和 unordered_map的关系一样，map中是按key排序的，支持运算符重载。
- 自动排序的优点是使得搜寻元素时具有良好的性能，具有对数时间复杂度。但是造成的一个缺点就是：
    - 不能直接改变元素值。因为这样会打乱原有的顺序
    - 改变元素值的方法是：先删除旧元素，再插入新元素。
    - 存取元素只能通过迭代器，从迭代器的角度看，元素值是常数。

## \<multiset>

- set和multiset的区别是：set插入的元素不能相同，但是multiset可以相同。



## 随机数   
```cpp
/** Get a random element from the collection. */
int getRandom() {
    int s = nums.size();
    int r = rand() % s;
    return nums[r];
}
```

## 结构体中的运算符重载
注意
```cpp
struct Node{
    int id;
    int weight;
    Node(int _id, int _weight){
        id = _id;
        weight = _weight;
    }

    bool operator< (const Node b) const{    //注意这里的const
        return weight > b.weight;
    }
};
```

# C++ 迭代器