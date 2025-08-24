# 中位数贪心
**定理：将 a 的所有元素变为 a 的中位数是最优的。**

证明：设 a 的长度为 n，设要将所有 a[i] 变为 x。假设 a 已经从小到大排序。首先，如果 x 取在区间 [a[0],a[n−1]] 之外，那么 x 向区间方向移动可以使距离和变小；同时，如果 x 取在区间 [a[0],a[n−1]] 之内，无论如何移动 x，它到 a[0] 和 a[n−1] 的距离和都是一个定值 a[n−1]−a[0]，那么去掉 a[0] 和 a[n−1] 这两个最左最右的数，问题规模缩小。不断缩小问题规模，如果最后剩下 1 个数，那么 x 就取它；如果最后剩下 2 个数，那么 x 取这两个数之间的任意值都可以（包括这两个数）。因此 x 可以取 a[n/2]。

作者：灵茶山艾府
链接：https://leetcode.cn/problems/apply-operations-to-maximize-frequency-score/solutions/2569301/hua-dong-chuang-kou-zhong-wei-shu-tan-xi-nuvr/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。