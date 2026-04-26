
# C++ STL 容器详解

## 一、STL 概述

STL（Standard Template Library）是 C++ 标准库的重要组成部分，提供了通用的模板类和函数，包括容器、迭代器、算法等。

### 1.1 容器分类

| 容器类型 | 特点 | 示例 |
|----------|------|------|
| 序列容器 | 元素有序排列 | vector, list, deque |
| 关联容器 | 元素按键有序排列 | set, map |
| 无序容器 | 元素无序，哈希表实现 | unordered_set, unordered_map |
| 容器适配器 | 包装其他容器 | stack, queue, priority_queue |

## 二、序列容器

### 2.1 vector

动态数组，支持随机访问。

```cpp
#include <vector>

std::vector<int> v;

// 添加元素
v.push_back(1);
v.push_back(2);
v.push_back(3);

// 访问元素
v[0];        // 随机访问
v.at(1);     // 带边界检查
v.front();   // 第一个元素
v.back();    // 最后一个元素

// 大小相关
v.size();    // 元素个数
v.empty();   // 是否为空
v.capacity();// 容量

// 删除元素
v.pop_back();
v.clear();
```

### 2.2 list

双向链表，不支持随机访问。

```cpp
#include <list>

std::list<int> l = {1, 2, 3};

l.push_front(0);   // 头部添加
l.push_back(4);    // 尾部添加
l.pop_front();     // 删除头部
l.pop_back();      // 删除尾部
l.insert(l.begin(), 10);  // 插入
```

### 2.3 deque

双端队列，支持两端快速插入删除。

```cpp
#include <deque>

std::deque<int> d;

d.push_front(1);
d.push_back(2);
d.pop_front();
d.pop_back();
```

### 2.4 forward_list

单向链表，比 list 更节省空间。

```cpp
#include <forward_list>

std::forward_list<int> fl = {1, 2, 3};
fl.push_front(0);
fl.pop_front();
```

## 三、关联容器

### 3.1 set

有序不重复元素集合。

```cpp
#include <set>

std::set<int> s;

s.insert(3);
s.insert(1);
s.insert(2);
s.insert(2);  // 重复元素被忽略

s.find(2);    // 查找
s.count(3);   // 计数（0或1）
s.erase(1);   // 删除
```

### 3.2 multiset

有序可重复元素集合。

```cpp
#include <set>

std::multiset<int> ms;
ms.insert(1);
ms.insert(2);
ms.insert(2);  // 允许重复
ms.count(2);   // 返回 2
```

### 3.3 map

有序键值对，键唯一。

```cpp
#include <map>

std::map<std::string, int> m;

m["apple"] = 5;
m["banana"] = 3;
m.insert({"orange", 4});

m.find("apple");      // 查找
m["apple"];           // 访问值
m.erase("banana");    // 删除
```

### 3.4 multimap

有序键值对，键可重复。

```cpp
#include <map>

std::multimap<std::string, int> mm;
mm.insert({"fruit", 5});
mm.insert({"fruit", 3});  // 相同键
```

## 四、无序容器

### 4.1 unordered_set

哈希集合，无序。

```cpp
#include <unordered_set>

std::unordered_set<int> us;

us.insert(1);
us.insert(2);
us.contains(1);  // C++20 特性
us.find(2);
```

### 4.2 unordered_map

哈希映射，无序。

```cpp
#include <unordered_map>

std::unordered_map<std::string, int> um;

um["key"] = 10;
um.find("key");
```

## 五、容器适配器

### 5.1 stack

栈，后进先出。

```cpp
#include <stack>

std::stack<int> s;

s.push(10);
s.push(20);
s.top();    // 20
s.pop();    // 删除20
```

### 5.2 queue

队列，先进先出。

```cpp
#include <queue>

std::queue<int> q;

q.push(10);
q.push(20);
q.front();  // 10
q.back();   // 20
q.pop();    // 删除10
```

### 5.3 priority_queue

优先队列，最大元素优先。

```cpp
#include <queue>

std::priority_queue<int> pq;

pq.push(3);
pq.push(1);
pq.push(4);
pq.top();   // 4
pq.pop();   // 删除4
```

## 六、迭代器

### 6.1 迭代器类型

| 类型 | 功能 | 支持容器 |
|------|------|----------|
| 随机访问 | 任意位置访问 | vector, deque, array |
| 双向 | 双向遍历 | list, set, map |
| 前向 | 单向遍历 | forward_list |

### 6.2 迭代器操作

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};

// 获取迭代器
auto it = v.begin();  // 指向第一个元素
auto end = v.end();   // 指向末尾之后

// 遍历
while (it != end) {
    std::cout << *it << " ";
    ++it;
}

// 使用范围for（C++11）
for (int num : v) {
    std::cout << num << " ";
}
```

## 七、容器常用算法

### 7.1 查找算法

```cpp
#include <algorithm>

std::vector<int> v = {3, 1, 4, 1, 5, 9};

// 查找
auto it = std::find(v.begin(), v.end(), 4);

// 计数
int count = std::count(v.begin(), v.end(), 1);

// 二分查找（需排序）
std::sort(v.begin(), v.end());
bool found = std::binary_search(v.begin(), v.end(), 5);
```

### 7.2 排序算法

```cpp
std::vector<int> v = {3, 1, 4, 2, 5};

std::sort(v.begin(), v.end());           // 升序
std::sort(v.begin(), v.end(), std::greater<int>());  // 降序
```

### 7.3 变换算法

```cpp
std::vector<int> v = {1, 2, 3, 4};
std::vector<int> result(4);

std::transform(v.begin(), v.end(), result.begin(), 
               [](int x) { return x * 2; });
```

## 八、容器选择指南

| 场景 | 推荐容器 |
|------|----------|
| 频繁随机访问 | vector |
| 频繁插入删除 | list |
| 两端操作 | deque |
| 唯一元素查找 | unordered_set |
| 键值对存储 | unordered_map |
| LIFO操作 | stack |
| FIFO操作 | queue |

## 九、实战示例

### 示例1：使用map统计词频

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    string text = "hello world hello cpp world";
    map<string, int> freq;
    
    string word;
    for (char c : text) {
        if (c == ' ') {
            freq[word]++;
            word.clear();
        } else {
            word += c;
        }
    }
    freq[word]++;
    
    for (const auto& pair : freq) {
        cout << pair.first << ": " << pair.second << endl;
    }
    return 0;
}
```

### 示例2：使用priority_queue实现Top K

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

vector<int> topK(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;
    
    for (int num : nums) {
        pq.push(num);
        if (pq.size() > k) {
            pq.pop();
        }
    }
    
    vector<int> result;
    while (!pq.empty()) {
        result.push_back(pq.top());
        pq.pop();
    }
    return result;
}
```

## 十、总结

| 容器 | 特点 | 时间复杂度（操作） |
|------|------|-------------------|
| vector | 动态数组 | 随机访问O(1), 尾部插入O(1) |
| list | 双向链表 | 插入删除O(1), 访问O(n) |
| deque | 双端队列 | 两端操作O(1) |
| set | 有序集合 | 查找O(log n) |
| map | 有序映射 | 查找O(log n) |
| unordered_set | 哈希集合 | 平均O(1) |
| unordered_map | 哈希映射 | 平均O(1) |
| stack | 栈 | O(1) |
| queue | 队列 | O(1) |

掌握STL容器是编写高效C++代码的关键，选择合适的容器可以显著提升程序性能。
