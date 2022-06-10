# lec19 - Memoization, subproblems, guessing, bottom-up; Fibonacci, shortest paths
> **动态规划**（英语：Dynamic programming，简称DP）是一种在[数学](https://zh.wikipedia.org/wiki/%E6%95%B0%E5%AD%A6 "数学")、[管理科学](https://zh.wikipedia.org/wiki/%E7%AE%A1%E7%90%86%E7%A7%91%E5%AD%A6 "管理科学")、[计算机科学](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6 "计算机科学")、[经济学](https://zh.wikipedia.org/wiki/%E7%BB%8F%E6%B5%8E%E5%AD%A6 "经济学")和[生物信息学](https://zh.wikipedia.org/wiki/%E7%94%9F%E7%89%A9%E4%BF%A1%E6%81%AF%E5%AD%A6 "生物信息学")中使用的，通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。
> 
> 动态规划常常适用于有重叠子问题和[最优子结构](https://zh.wikipedia.org/w/index.php?title=%E6%9C%80%E4%BC%98%E5%AD%90%E7%BB%93%E6%9E%84&action=edit&redlink=1)性质的问题，动态规划方法所耗时间往往远少于朴素解法。
> 
> 动态规划背后的基本思想非常简单。大致上，若要解一个给定问题，我们需要解其不同部分（即子问题），再根据子问题的解以得出原问题的解。
> 
> 通常许多子问题非常相似，为此动态规划法试图仅仅解决每个子问题一次，从而减少计算量：一旦某个给定子问题的解已经算出，则将其[记忆化](https://zh.wikipedia.org/wiki/%E8%AE%B0%E5%BF%86%E5%8C%96 "记忆化")存储，以便下次需要同一个子问题解之时直接查表。这种做法在重复子问题的数目关于输入的规模呈[指数增长](https://zh.wikipedia.org/wiki/%E6%8C%87%E6%95%B8%E5%A2%9E%E9%95%B7 "指数增长")时特别有用。

## 什么是动态规划
可以从很多角度理解动态规划：

动态规划是一种细致的暴力算法，因为他访问了所有可能的状态，并从中选择最优解

动态规划 = 递归 + 重用 + 猜测 + 记忆。重用使得动态规划从原始算法的指数级下降为多项式级。

动态规划依赖于最小子问题，每个最小子问题具有最优子结构。

动态规划算法的耗时 = 子问题数量 * 每个子问题的耗时。

动态规划在具有DAG性质的状态图中寻找最短/最长路径。

解决动态规划问题的难点在于如何定义状态，以及状态之间如何转换。如果构建了正确的状态图，那么动态规划问题就是一个最短路径问题。

## 动态规划的两种范式
有两种解决递归或者自底向上。后者需要按照拓扑排序执行。

前者的依赖关系是基于递归树显式表达的，后者的依赖关系基于拓扑排序隐含表达的。

递归、自底向上和基于图的算法之间可以相互转换。

## Fibonacci
Fibonacci求解就是一个典型的动态规划问题。

递归解法

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220316163959.png)

自底向上解法，k从1到n，反映了从小问题出发，利用最优子结构+重用解决大问题。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220316164021.png)

## shortest paths
基于动态规划的最小路径问题反应了多态规划的实质。

### 无环图
如果图中不存在循环，那么最短路径算法的递归解法是有解的，算法的时间复杂度为$O(V+E)$。子问题的数量为$V$，所有子问题耗时为$O(V+E)=O(V^2)$。这实际上就是Topo sort + DFS。在很多动态规划问题中，就是对状态图进行Toposort + DFS，其时间复杂度就是$O(V+E)$。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220316164300.png)

### 有环图
如果图中存在循环，那么原有的递归式会无限递归下去。但如果改造一下，可以将有环图转换为无环图，就能求解了。此时子问题数量为$V^2$，算法的总时间复杂度为$$O(V'+E')=O(V^2+VE)=O(V^3)$$
实际上就是Bellmen-Ford算法。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220316164531.png)

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220316164701.png)
