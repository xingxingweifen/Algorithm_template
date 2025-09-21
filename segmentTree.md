# 线段树模板

 [参考灵神模板](http://leetcode.cn/discuss/post/3583665/fen-xiang-gun-ti-dan-chang-yong-shu-ju-j-bvmv/)

```cpp
#include <bits/stdc++.h>
using namespace std;

template<typename T>
class SegmentTree{
private:
    int n; // 所维护数组的区间长度
    vector<T> tree; // 线段树 可以粗略计算大小为4 * n

    void build(const vector<T>& a, int o, int l, int r){
        /*
        params:
            a -- 初始化数组
            o -- 代表线段树的节点下标 从 1 开始，易得其左右孩子的下标分别为 2 * o 和 2 * o + 1
            线段树节点o中维护着a[l, r]的感兴趣值
        */
       
        // 采用后序遍历进行初始化
        if (l == r){
            tree[o] = a[l];
            return;
        }

        int mid = (l + r) / 2;
        build(a, 2 * o, l, mid);
        build(a, 2 * o + 1, mid + 1, r);
        tree[o] = max(tree[2 * o], tree[2 * o + 1]); // 根据需求更改
    }
public:
    // 线段树维护一个长为 n 的数组（下标从 0 到 n-1），元素初始值为 init_val
    SegmentTree(int n, T init_val) : SegmentTree(vector<T>(n, init_val)) {}
    // 通过a进行初始化 即后序遍历
    SegmentTree(const vector<T>& a) : n(a.size()), tree(2 << bit_width(a.size() - 1)) {
        build(a, 1, 0, n - 1);
    }
    /*进行单点更新，需要更新a数组下标为i的元素为val  logn*/
    void update(int o, int l, int r, int i, T val){
        // 后序更新
        if (l == r){
            tree[o] = val;
            return;
        }
        int mid = (l + r) / 2;
        if (i <= mid){
            update(2 * o, l, mid, i, val);
        }else{
            update(2 * o + 1, mid + 1, r, i, val);
        }

        tree[o] = max(tree[2 * o], tree[2 * o + 1]); // 根据需求更改
    }
    /*进行区间查询，查询a[left, right]的属性  log n*/
    T query(int o, int l, int r, int left, int right){
        if (l >= left && r <= right){
            return tree[o];
        }

        int mid = (l + r) / 2;
        if (right <= mid){ // 位于左子树
            return query(2 * o, l, mid, left, right);
        }
        if (left >= mid + 1){ // 位于右子树
            return query(2 * o + 1, mid + 1, r, left, right);
        }

        T l_res = query(2 * o, l, mid, left, mid);
        T r_res = query(2 * o + 1, mid + 1, r, mid + 1, right);

        return max(l_res, r_res); // 根据需求进行更改
    }
};
```

