# lec07 - Counting Sort, Radix Sort, Lower Bounds for Sorting
## 算法最优复杂度的理论推导
如果搜索与排序是基于比较模型，那么过程就表现为二叉决策树。树的叶节点是所有可能的情况，而树的高度就是算法的最差时间复杂度。
### search
search方法的决策树的叶节点的数量是$n+1$，由于决策树是二叉的，那个最差的时间复杂度就是$logn$。因此search方法的时间复杂度就是$logn$。

决策树可视化结果如下：

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220218124606.png)

### sorting
sorting方法的决策树的叶节点的数量是$n!$，由于决策树是二叉的，sorting的最差时间复杂度是$log(n!)$。

推导过程如下：

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220218124850.png)

## 时间复杂度为O(n)的排序算法
### counting sort
计数排序是一种不基于比较的排序方法，可以使solid的，也可以是ubsolid的。计数排序通过定义一个长度为k的数组，通过遍历长度为n的待排序数组，统计每个元素出现的次数，然后将结果输出。

时间复杂度为$O(n+k)$，n表示原数组的长度，k表示计数数组的长度。当n与k数量级相似或者n>>k时，算法的时间复杂度为O(n)，但如果$k>>n$ ，那么counting sort的效率就大大降低了。这意味着counting sort的执行效率并不固定，收到数据分布的影响。
### radix sort
基数排序是一种不基于比较的排序方法，因此不受$Ω(nlogn)$下界的限制。基数排序是通过将待排数字补0到相同位数，然后从小到大分别对每个位数进行solid排序，时间复杂度为$O(k*n)$。

基数排序效果示意图：

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220218132107.png)

## solid sort
| 排序算法 | 是否solid | 时间复杂度 |
| -------- | --------- | ---------- |
| 插入排序 | s         | n          |
| 归并排序 | s         | nlogn      |
| 堆排序   | us        | nlogn      |
| 计数排序 | s         | n+k        |
| 基数排序 | s         | k\*n        | 
