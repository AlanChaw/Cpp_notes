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

### 1. shared_ptr 类






### 2. 直接管理内存





### 3. shared_ptr 和 new 结合使用





### 4. 智能指针和异常





### 5. unique_ptr





### 6. weak_ptr