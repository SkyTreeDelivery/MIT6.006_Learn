# lec21 - Parenthesization, Edit Distance, Knapsack
## 序列中的动态规划模板
对一个序列执行动态规划是一个常见的模板。

在[[lec21 - Parenthesization, Edit Distance, Knapsack | lec20]] 中，文字排版和21点都是一个序列动态规划问题，常见的有三种子问题模式，suffixes[i:]、prefixes[:i]、substrings[i:j]。

前两者的子问题数量是$O(n)$，后者的子问题数量是$O(n^2)$。subffixes和prefixes不会同时使用，suffixes更符合人类的思维模式，但实际上suffixes和prefixes是等价的。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319172653.png)

## 矩阵连乘次序
对于多个相乘的矩阵，不同的计算次序的时间消耗不同。以合适的括号优化矩阵相乘的次序可以优化矩阵连乘的性能。这是一个典型的substring[i:j]问题。

$dp[i][j]$表示每个子问题，i表示插入$($的下标，j表示插入$)$的下标时的最小成本。 

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319173100.png)

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319173714.png)

## 编辑距离
编辑距离是度量两段字符串相似度的常用算法，可以用于DNA比较、CVS diff、拼接检查、抄袭检查等应用中。

可以基于动态规划算法解决编辑距离。

编辑距离 = 将一个字符串A经过有限次操作转换为目标字符串B的操作次数或加权总和。

之前的问题都是针对一个字符串进行操作，而编辑距离需要同时处理两个字符串。

子问题为$dp[i][j]$，表示$x[i:]$和$y[j:]$之间的最小编辑距离。

通过定义insert、delete和replace方法的成本来进行动态规划。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319180907.png)


![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319180836.png)![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319180959.png)

## 01背包问题
背包问题一个朴素的处理方法是，将每个item是否装入背包作为state，但这会导致state的数量为$O(n^2)$。

更好的解决方案是，记录装入每个物品时剩余的背包空间。子问题定义为$dp[i][j]$，表示在装入第i个item时，背包空间剩余为j时的最大value。

与之前的问题不同，背包的子问题是有存在依赖的约束的，即在装入一个item是，剩余的背包空间取决于前面装入的item。

背包问题的动态规划解法的时间复杂度为$O(nS)$，这是一个伪多项式时间的算法。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319181649.png)
![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319181701.png)

## 伪多项式时间
[伪多项式时间的理解 - stackoverflow](https://stackoverflow.com/questions/19647658/what-is-pseudopolynomial-time-how-does-it-differ-from-polynomial-time)

伪多项式时间涉及到标准时间复杂度的定义，传统的时间复杂度时依据输入的数量进行描述，算法的时间复杂度取决于输入量的单位，而标准时间复杂度将输入数字的bit长度视为单位。

> **如果一个算法的传统时间复杂度是多项式时间的，而标准时间复杂度不是多项式时间的，则我们称这个算法是伪多项式时间的。**

> 多项式时间算法 (polynomial time algorithm) 表示：算法的复杂度与输入的规模呈多项式关系
> 
> 伪多项式时间算法 (pseudopolynomial time algorithm) 是表示：算法的复杂度与输入规模呈指数关系，与输入的数值大小呈多项式关系。

> 冒泡排序：给定 n 个64位的数字，进行 n-1 次扫描交换，将数字从小到大排序。
> 
> 素数测试：给定数字 n，通过从 2 到根号 n 的整数遍历，判断 n 是否为素数。
> 
> 字面上看，两者复杂度都是 O(n^k) ( k 为整数) 。但区别在于，前者的 n 是数字个数的多少，后者的 n 是数字的大小。
> 
> 因此，前者输入总规模 s1 增长与数字大小无关，s1 = 64n；后者增长规模与数字大小紧密相关，输入总规模为 s2 = logn 。
> 
> 所以可知冒泡排序中复杂度 O(n^2) = O(s1^2/64^2) 为多项式算法，后者素数测试O(n) = O(2^(s2)) 为伪多项式算法啦。

背包问题的动态规划算法是一个伪多项式算法，因为其时间复杂度与背包的规模有关（？）。

## Subset-Sum问题
背包问题的变种，假设往背包里装入黄金，黄金的价值与重量成正比，每个item只能使用一次，是否能找出一种装法，是的背包刚好装满。

和01背包问题类似，区别在于最终的目标价值是一定的，问题变成找到一条路径使得背包刚好装满。背包装满的状态为$dp[n][0]=1$，其他$dp[n][j]=0$。

Solution是$dp[0][S]$。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319182908.png)

## K-Sum问题
背包问题的变种，假设往背包里装入黄金，黄金的价值与重量成正比，每个item只能使用一次，是否能找出一种装法，刚好装入k个item并使背包刚好装满。

相比于Subset-Sum问题，添加了一个新的约束条件，必须装入k个item。

背包装满的状态为$dp[n][0][0]=1$，其他$dp[n][j][k]=0$。

Solution是$dp[0][S][K]$。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220319183310.png)

## Multi-K-Sum问题
背包问题的变种，假设往背包里装入黄金，黄金的价值与重量成正比，每个item可以装入无限次，是否能找出一种装法，刚好装入k个item并使背包刚好装满。

相比于K-Sum问题，改变了约束，每个item可以使用无限次。

递归公式如下：
$$
dp\left[ i \right] \left[ j \right] \left[ k \right] =\left( \begin{array}{c}
	dp\left[ i+1 \right] \left[ j \right] \left[ k \right]\\
	dp\left[ i+1 \right] \left[ j-s_it \right] \left[ k-t \right] \,\,if\,\,j\geqslant s_it\,\,and\,\,k>0\\
\end{array} \right) 
$$
初始条件$dp[n][0][0]=1$，其他$dp[n][j][k]=0$。

Solution是$dp[0][S][K]$。