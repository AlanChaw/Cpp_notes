<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [C++ 动态内存和智能指针](#c-%E5%8A%A8%E6%80%81%E5%86%85%E5%AD%98%E5%92%8C%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88)
    - [1. shared_ptr 类](#1-shared_ptr-%E7%B1%BB)
    - [2. 直接管理内存](#2-%E7%9B%B4%E6%8E%A5%E7%AE%A1%E7%90%86%E5%86%85%E5%AD%98)
    - [3. shared_ptr 和 new 结合使用](#3-shared_ptr-%E5%92%8C-new-%E7%BB%93%E5%90%88%E4%BD%BF%E7%94%A8)
    - [4. 智能指针和异常](#4-%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88%E5%92%8C%E5%BC%82%E5%B8%B8)
    - [5. unique_ptr](#5-unique_ptr)
    - [6. weak_ptr](#6-weak_ptr)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# C++ 动态内存和智能指针

为了更安全的使用动态对象，标准库定义了两个智能指针类型来管理动态分配的对象。当一个对象应该被释放时，指向它的智能指针可以确保自动地释放它。

智能指针的行为类似常规指针，重要的区别是它负责自动释放所指向的对象。新标准库提供的这两种智能指针的区别在于管理底层指针的方式 (以下三种类型都定义在\<memory\>头文件)：
- __shared_ptr__ 允许多个指针指向同一个对象
- __unique_ptr__ 则"独占"所指向的对象
- __weak_ptr__ 是一个伴随类，是一种弱引用

### 1. shared_ptr 类

1. 智能指针也是模板，创建一个智能指针：
    ```cpp
    shared_ptr<string> p1;          // 一个可以指向 string 的 shared_ptr
    shared_ptr<list<int>> p2;       // 一个可以指向 list<int> 的 shared_ptr

    // 使用方式与普通指针类似
    if(p1 & p1->empty())
        *p1 = "hi";
    ```

2. 下边是 shared_ptr 和 unique_ptr 都支持的操作以及 shared_ptr 独有的操作

    <div align="center">
    <img src="./pics/share_and_unique.png" width="700" align=center />
    <br><br>
    </div>

    <div align="center">
    <img src="./pics/share_only.png" width="700" align=center />
    <br><br>
    </div>

3. __make_shared 函数__
    - 最安全的分配和使用动态内存的方式。
    - 此函数在动态内存中分配一个对象并初始化，返回指向该对象的 shared_ptr。
        ```cpp
        // p3 指向 int 类型 42
        shared_ptr<int> p3 = make_shared<int>(42);
        // p4 指向 string 类型 "9999999999"
        shared_ptr<string> p4 = make_shared<string>(10, '9');
        // p5 指向一个 int ，初值为 0
        shared_ptr<int> p5 = make_shared<int>();
        ```
    - 通常用 auto 定义一个对象来存储 make_shared 返回的结果：
        ```cpp
        auot p6 = make_shared<vector<string>>();
        ```

4. __shared_ptr 的拷贝和赋值__
    - 当进行拷贝或赋值操作时，每个 shared_ptr 都会记录有多少个其他 shared_ptr 指向相同的对象，详见上面表中对赋值操作的解释。
    - 每个 shared_ptr 都有一个引用计数，当拷贝一个 shared_ptr，用一个 shared_ptr 初始化另一个，或将其传递给函数、其作为函数返回值时，引用计数加一；当给一个 shared_ptr 赋新值(改变指针指向)、或 shared_ptr 被销毁时，引用计数会减一。  
    当引用计数为0时，就会自动释放自己所管理的对象。
        ```cpp
        auto r = make_shared<int>(42);
        r = q;
        // 给 r 赋值，这样 r 的指向被改变，r原来所指的对象的引用计数会减一，q 指向对象的引用计数加一。 这时 r 所指的对象引用计数为 0，会自动释放。
        ```

5. 当引用计数为0， shared_ptr 销毁其指向的对象时，会调用该对象的析构函数来销毁。

6. 对于一块内存，shared_ptr 类保证只要有任何 shared_ptr 对象引用它，它就不会被释放掉。

7. 使用动态内存的一个常见原因是允许多个对象共享相同的状态。


---
### 2. 直接管理内存

1. 直接管理内存是指用 __new__ 分配内存，用 __delete__ 释放内存。相比于智能指针，手动管理内存很容易出错，而且不能依赖类对象拷贝、赋值和销毁操作的任何默认定义。

2. new 操作在堆中申请一块内存构造对象，并返回指向该对象的指针。对于内置类型需要手动初始化，对自定义类型使用构造函数初始化。

3. 当内存耗尽时，new 操作符会抛出 bad_alloc 异常。但可以通过 __定位new__ 来避免:
    ```cpp
    int *p1 = new (nothrow) int;     // 分配失败，new 操作符会返回一个空指针
    ```

4. __delete__ 接受一个指针，这个指针所指向的内存必须是用 new 分配的、或者指向nullptr。delete执行两个动作：销毁给定指针所指向的对象；释放对应内存。

5. 对于一个由内置指针管理的动态对象(new 分配的)，直到被显示释放前它都是存在的。而且如果函数将一个指向由 new 分配内存的指针给其调用者，相当于给其调用者增加了负担，因为调用者需要记得释放内存。

6. 使用 new 和 delete 管理动态内存的三个常见问题：
    - 忘记 delete 内存
    - 使用已经释放掉的对象
    - 同一块内存释放两次

    坚持使用智能指针，可以避免所有这些问题。


---
### 3. shared_ptr 和 new 结合使用

1. 可以用 new 返回的指针来初始化智能指针：
    ```cpp
    shared_ptr<int> p1(new int(42));     // p2 指向一个值为 42 的 int

    shared_ptr<int> p1 = new int(42);    // 这样用是错误的，智能指针不支持隐式转换
    ```

    当将一个 shared_ptr 绑定到一个普通指针时，我们就将内存管理的责任交给了这个 shared_ptr。一旦这样做了，我们就不应该再使用内置指针来访问 shared_ptr 所指向的内存了。

2. 调用 shared_ptr 类对象的 get() 函数可以返回对象中的指针，但永远不要用 get() 返回的指针初始化另一个智能指针或者为另一个智能指针赋值。

3. shared_ptr 还支持 reset() 操作，将一个新的指针赋予一个 shared_ptr，reset()会更新引用计数。 reset() 经常与 unique() 函数一起使用，来控制多个 shared_ptr 共享的对象：
    ```cpp
    p = new int(1024);      // 这样是错误的，不能将普通指针直接赋值给智能指针
    p.reset(new int(1024)); // 这样是正确的，p指向一个新对象，原引用计数减一
    ```

    ```cpp
    if(!p.unique()){
        p.reset(new string(*p));    // 我们不是唯一用户，分配新的拷贝
    }
    *p += newVal;           // 现在我们知道我们是唯一的用户，可以对对象进行修改了
    ```

### 4. 智能指针和异常

1. 当使用智能指针时，即使程序块过早结束，智能指针也能确保在内存不需要时将其释放。但是对于 new 分配的内存(直接管理的内存)，在 new 之后，delete 之前发生了异常，该内存不会被释放。

2. __自定义删除器__
    - 对于 struct 定义的类(无析构函数)，可以自定义一个删除器函数传给 shared_ptr，从而使 shared_ptr 能够对其指针进行正确释放。
        ```cpp
        struct connection;
        struct destination;
        connection connect(destination *);
        void disconnect(connection);

        void end_connection(connection *p){     // 自定义删除器
            disconnect(*p);
        }

        // 在使用时：
        connection c = connect(&d);         // 建立连接
        shared_ptr<connection> p(&c, end_connection);
        // 使用c 。。。 balabala
        // 即使这段代码块(函数)由于异常而中断，c 也会被正确关闭。
        ```

        当 p 被销毁时，不会对其保存的指针调用 delete,而是 end_connection。

3. 几个智能指针陷阱，需要注意：
    - 不使用相同的内置指针初始化多个智能指针。 因为一个智能指针所指的内存被释放，会使其他的几个空悬。
    - 不 delete 由 get() 函数返回的指针。
    - 不使用 get() 初始化或 reset() 另一个智能指针。
    - 如果使用 get() 返回的指针，当最后一个对应的智能指针销毁后，你的指针就变为无效了。
    - 如果使用智能指针管理的资源不是 new 分配的内存，记住传递给它一个删除器。


### 5. unique_ptr

1. 一个 unique_ptr “拥有” 它所指向的对象。某个时刻只能有一个 unique_ptr 指向一个给定对象。 当 unique_ptr 被销毁时，它所指向的对象也被销毁。

2. 当使用 unique_ptr 时，必须将其绑定到一个 new 返回的指针上。
    ```cpp
    unique_ptr<double> p1;
    unique_ptr<int>(new int(42));
    ```

3. unique_ptr 不支持一般的拷贝和赋值操作，但是可以使用 u.release() 或 u.reset() 函数将指针的所有权从一个 unique_ptr 转移给另一个：
    ```cpp
    // release将p1置为空，并把所有权转移给p2
    unique_ptr<string> p2(p1.release());    

    // 将所有权从p3转移给p2
    unique_ptr<string> p3(new string("trex"));
    p2.reset(p3.release());     // reset 释放了p2的内存，并将p3转移给p2;
    ```

4. 允许拷贝或赋值一个将要被销毁的 unique_ptr 对象。例如从函数返回一个 unique_ptr 对象。

5. 与 shared_ptr 不同，重载 unique_ptr 的删除器时，需要在创建或reset时显示指明删除器的类型：
    ```cpp
    unique_ptr<objT, delT> p (new objT, fcn);

    // 将上面 connection 的例子改为 unique_ptr：

    connection c = connect(&d);
    unique_ptr<connection, decltype(end_connection)*> p(&c, end_connection);

    // 这里 end_connection 是一个函数，decltype()返回一个函数类型，*表示函数指针
    ```


### 6. weak_ptr

1. __weak_ptr 是一种不控制所指向对象生存期的指针，它指向一个由 shared_ptr 管理的对象__。 将一个 weak_ptr 绑定到一个 shared_ptr 不会改变 shared_ptr 的引用计数。一旦最后一个指向对象的 shared_ptr 被释放，对象就会被释放掉，即使有 weak_ptr 指向该对象，也还是会被释放。 因此说它是 “弱” 共享对象。

2. 当创建一个 weak_ptr 对象时，要用一个 shared_ptr 来初始化：
    ```cpp
    shared_ptr<int> p = make_shared<int>(42);
    weak_ptr<int> wp(p);        // 弱共享p; 此时p的引用计数不变
    ```

3. weak_ptr 使用函数 w.expired() 查看是否过期； 还可以通过 w.use_count() 查看共享的 shared_ptr 的引用计数。

4. 因为 weak_ptr 共享的对象可能不存在，所以不能直接使用 weak_ptr 来访问对象，而必须调用 w.lock()。如果对象存在，该函数返回一个指向对象的 shared_ptr，否则返回一个空的 shared_ptr.
    ```cpp
    //在语句块中使用np访问对象，语句块结束np销毁
    if(shared<int> np = wp.lock();){        
        ...
    }
    ```




