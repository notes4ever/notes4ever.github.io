Timsort是一种混合稳定排序算法，源自归并排序（merge sort）和插入排序（insertion sort）。

2002年Tim Peters为Python编程语言创建了Timsort。自从Python 2.3开始，Timsort一直是Python的标准排序算法。如今，Timsort 已是是 Python、 Java、  Android平台 和 GNU Octave 的默认排序算法。

**本质上 Timsort 是一个经过大量优化的归并排序**，而归并排序已经到达了最坏情况下，比较排序算法时间复杂度的下界，所以在最坏的情况下，Timsort 时间复杂度为 O(nlgn)。在最佳情况下，即输入已经排好序，它则以线性时间运行 O(n)。

## 参考资料
- https://sikasjc.github.io/2018/07/25/timsort/
- https://www.cnblogs.com/hejiayang/p/14119741.html