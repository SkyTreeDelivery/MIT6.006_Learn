# lec01 - Algorithmic thinking, peak finding
## 一维
从一维峰值（1-D peak finding）寻找开始，使用二分法寻找峰值，算法复杂度为O(logn)。
## 二维
然后从一维升维到二维，探索了5种不同的算法。

第一种算法是一维形式的简单拓展，每次搜索[m * (n/2)]的空间====

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220116172156.png)

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220116172244.png)


第二种算法是贪婪算法，每次搜索更大的邻接元素。时间复杂度O(n^2)。

第三种算法从两个纬度上同时缩减，每次搜索[(m/2) * (n/2)]的空间。时间复杂度为O(n)。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220116172136.png)

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220116172257.png)

第四种算法从两个纬度上依次缩减，每次搜索[m * (n/2)]或[(m/2) * n]的空间。时间复杂度为O(n)。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220116172212.png)

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220116172224.png)

这里面算法三是错误的，我认为错误的原因可以解释为，在子区域中是峰值的不能保证其在父区域中也是峰值。

证明方法峰值搜索算法分两步：
1. 一定有返回值
2. 子区域中是峰值的，其在父区域中也一定是峰值。（也可以说第一步的返回值就是原始矩阵的峰值）

人们对于算法的最坏时间复杂度有更多的关注，因为这关系到算法的耗时上限。

算法的效率一次递增，这些算法代表了一些典型的复杂度：
- 一维算法代表了O(1) * O(logn)，单次操作为常数时间，操作O(logn)次。
- 算法1代表了O(n) * O(logn)，单词操作为O(n)，操作O(logn)次。
- 算法2代表了贪婪算法，在最坏情况下对所有元素操作一次。
- 算法3和4代表了O(n + n/2 + ... + 1) < O(2n) = O(n)，操作O(logn)次，但每次操作数减少一半，类似于比为1/2的等比数列求和。

以下是一维算法的递归实现。

```java
class Solution {
    public int findPeakElement(int[] nums) {
        return recursion(nums, 0, nums.length);
    }

    /**
     * 基于虚拟定位（start + num），而不是复制数组元素
     * @param nums
     * @param start
     * @param num
     * @return
     */
    private int recursion(int[] nums, int start, int num){
        // 在子任务中是峰值的元素，在父任务中也是峰值，如果只有一个任务，则一定是峰值
        // 这个判断类似于在循环方法中的while（left<right）
        if(num <= 1) return start;
        int mid = num / 2 + start;  // 绝对坐标转换为相对坐标
        int midNum = nums[mid];
        if(mid - 1 >= 0 && midNum < nums[mid - 1]){  // 如果某一侧元素为0个，则继续
            return recursion(nums, start, num / 2);
        }else if (mid + 1 < nums.length && midNum < nums[mid + 1]){
            return recursion(nums, mid + 1, num - num / 2 - 1);
        }else{
            return mid;
        }
    }

    public static void main(String[] args) {
        Solution solution = new Solution();
        int[] nums = {1,7,3,4,5,6,5,2,1};
        System.out.println(solution.findPeakElement(nums));
    }
}
```



