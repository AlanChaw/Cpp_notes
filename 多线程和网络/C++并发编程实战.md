<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [C++并发编程实战笔记](#c%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E5%AE%9E%E6%88%98%E7%AC%94%E8%AE%B0)
  - [1. 线程管理](#1-%E7%BA%BF%E7%A8%8B%E7%AE%A1%E7%90%86)
    - [1.1 线程管理基础](#11-%E7%BA%BF%E7%A8%8B%E7%AE%A1%E7%90%86%E5%9F%BA%E7%A1%80)
    - [1.2 向线程函数传参](#12-%E5%90%91%E7%BA%BF%E7%A8%8B%E5%87%BD%E6%95%B0%E4%BC%A0%E5%8F%82)
    - [1.3 转移线程所有权](#13-%E8%BD%AC%E7%A7%BB%E7%BA%BF%E7%A8%8B%E6%89%80%E6%9C%89%E6%9D%83)
    - [1.4 运行时决定线程数量](#14-%E8%BF%90%E8%A1%8C%E6%97%B6%E5%86%B3%E5%AE%9A%E7%BA%BF%E7%A8%8B%E6%95%B0%E9%87%8F)
    - [1.5 标识线程](#15-%E6%A0%87%E8%AF%86%E7%BA%BF%E7%A8%8B)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# C++并发编程实战笔记

## 1. 线程管理

### 1.1 线程管理基础

1. 线程函数就是线程的入口，main()函数就是初始线程的入口。通常是无参数无返回值的函数。使用 C++ 线程库启动线程，可以归结为构造 `std::thread` 对象：
    ```cpp
    void do_some_work();
    
    std::thread my_thread(do_some_work);
    ```

    与大多数标准库一样，传重载了函数调用运算符（）的类的对象也是可以的：
    ```cpp
    class background_task{
    public:
        void operator()(  ) const{
            do_something();
            do_something_else();
        }
    };

    background_task f;
    std::thread my_thread(f);
    ```
    也可以使用 lambda 表达式（就相当于传一个函数了）：
    ```cpp
    std::thread my_thread([]{
        do_something();
        do_something_else();
    })
    ```

2. 启动线程后，在线程对象被销毁之前，必须决定以何种方式等待线程结束，否则程序就会终止。因此，即使在线程被创建后发生异常，也要确保线程能够正确 join 或 detach。

3. 对于使用 join() 来同步时，要确保在线程对象创建后，join()调用之前，的异常安全性，可以使用 RAII：
    ```cpp
    class thread_guard{
        thread& t;
    public:
        explicit thread_guard(thread& _t)
            : t(_t) {}

        ~thread_guard(){
            if(t.joinable())
                t.join();
        }

        // 禁止拷贝操作
        thread_guard(const thread_guard&) = delete;
        thread_guard& operator=(const thread_guard&) = delete;
    }

    // 当调用线程函数时：
    void f(){
        int local_state = 0;
        thread t(my_func);

        do_some_thing();

        thread_guard g(t);
    }
    ```
    使用 RAII 时，相当于利用了一层 `thread_guard` 做封装，而一旦函数执行过程中，线程 `join()`之前，出现了任何异常，编译器也会自动调用 `thread_guard` 对象 `g` 的析构函数，从而确保线程能够正确加入.


4. 如果选择分离线程(detach)，就必须保证线程结束之前，线程所要访问到的数据都是有效的。例如，当前函数已经退出，函数中局部变量已经被销毁，而线程还没有结束，仍然尝试访问函数中局部变量。要解决这种问题，可以使线程函数功能齐全，将数据都复制到线程中，而非复制到共享数据中（使用一个可接受参数的线程函数）。

5. 如果线程分离，就不能与主线程直接产生交互，主线程也没有 `std::thread` 可以引用该线程。分离线程在后台运行 `joinable() == false`。只有 `joinable() == true` 的时候才可以选择对线程进行 `join()` 或 `detach()` 这种分离线程通常称为 __守护线程(daemon threads)： 没有任何显示的用户接口，并在后台运行的线程__。 


---
### 1.2 向线程函数传参

1. __这里有一个大坑：__ 标准约定 `std::thread` 构造时向函数对象传递实际参数的拷贝（支持移动语义），而不是转发实际参数。即使函数要求接受引用，在构造时传进去的依然是拷贝（这其实是 thread 类构造函数的问题），这种情况在一些编译器是可以通过编译的，但是在函数中修改所谓的引用时，修改的只是线程内部拷贝的引用。这种情况在有些编译环境下根本无法通过编译。所以要求在传引用时，使用 `std::ref()` 函数明确指明传的是引用才可以：

    ```cpp
    void fn(int& a);

    int a = 10;
    std::thread t(fn, std::ref(a));
    t.detach();
    ```
    这样函数 `fn()` 就会真正接收到一个变量 `a` 的引用。

2. 传参时的另一个问题：要注意参数要在传给线程函数前要初始化完成，以满足异常安全性，例如：

    ```cpp
    void f(int i, std::string s);

    std::thread t(f, 3, "hello");
    ```
    这里 "hello" 是作为字面值传入的，会由 `const char*` 被隐式转换成 `string` ，但是这个转换过程中可能会出现异常，所以，需要在调用 `std::thread()` 前就将其转换为 `string` 对象。 这其实与 [Effective C++ 17. 以独立语句将 newed 对象置入智能指针](https://github.com/AlanChaw/Cpp_notes/blob/master/Effective_C%2B%2B.md#17-%E4%BB%A5%E7%8B%AC%E7%AB%8B%E8%AF%AD%E5%8F%A5%E5%B0%86-newed-%E5%AF%B9%E8%B1%A1%E7%BD%AE%E5%85%A5%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88) 所说的情况是一样的，因为一旦中途出现异常会出问题。

---
### 1.3 转移线程所有权

1. 线程不允许拷贝，但允许移动（发生所有权在实例中的转移）。这一点与 `unique_ptr` 很像。 顺带一提，对这类支持移动但不支持拷贝的对象，可以通过 `std::move` 函数把一个对象移动给另一个空对象，但是不支持移动给另一个非空的对象。
    ```cpp
    using std::thread;
    using std::move;

    t1 = thread(func1);
    // 将 t1 所有权转移给 t2
    thread t2 = move(t1);      
    // 声明一个临时对象 thread(func2)，对于临时对象，会隐式调用移动操作
    t1 = thread(func2);             

    // 将 t2 所有权转移给一个空对象 t3，是可以的
    thread t3;
    t3 = move(t2);
    
    // 将 t3 所有权转移给一个非空对象，会使程序崩溃
    t1 = move(t3);
    ```

2. 支持移动的情况下，也就是说支持其所有权在函数内外进行转移，也就是说
    - 一个 `std::thread` 对象可以作为函数返回值
    - 可以将 `std::thread` 对象作为参数传递给函数

    也就是说，guard_thread 类还可以进行一些改进，即不使用 `std::thread` 指针作为成员，而是直接使用一个对象，这样在构造时直接传入一个线程对象，将这个线程对象的所有权转移给自身成员中的线程对象即可。

---
### 1.4 运行时决定线程数量

1. 函数 `std::thread::hardware_concurrency()` 可以返回能同时并发在一个程序中的线程数量。多核系统中返回的是CPU核心数量。系统信息无法获取时返回0。这个值可以帮助我们动态地、合理地选择开放多少个线程，非常有用。

2. 例子，实现一个并行版本的 `std::accumulate()` 累加器函数。[代码链接](./parallel_accumulate.cpp)


---
### 1.5 标识线程



