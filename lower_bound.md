此文档用于记录lower_bound的自定义比较函数参数

```cpp
// 版本 1：使用默认的 < 运算符比较
template< class ForwardIt, class T >
ForwardIt lower_bound( ForwardIt first, ForwardIt last, const T& value );

// 版本 2：使用自定义比较函数
template< class ForwardIt, class T, class Compare >
ForwardIt lower_bound( ForwardIt first, ForwardIt last, const T& value, Compare comp );
```

对于comp的作用等价于手写lower_bound中if表达式的作用。

```cpp
auto lower_bound = [&](int target)->int{
    int left = 0, right = events.size();
    while (left < right){
        int mid = (left + right) >> 1;
        if (events[mid][1] < target){ // 等价于在此处调用comp(events[mid], value)
            left = mid + 1;
        }else{
            right = mid;
        }
    }

    return left;
};
```

