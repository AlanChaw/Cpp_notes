
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