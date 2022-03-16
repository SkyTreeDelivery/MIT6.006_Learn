# lec15 - Single-Source Shortest Paths Problem
单源最短路径算法，s.p.，寻找图中指定其实节点到所有节点的最短路径。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220310165516.png)

对于没有负环的图，通用的S.P.算法结构如下，所有的边都要经历一次松弛操作。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220310165335.png)

常见的S.F.算法包括：
- BFS：O(V+E)
- Dijkstra：最优时间复杂度$O(VlogV+E)$
- BF：最优时间复杂度$O(VE)$
## 不同的图中的最短路径算法
无权图。使用BFS。

带权图。可以通过添加虚拟节点将边均分为单位长度的子边，将带权图转换为无权图，然后执行BFS。也可以直接使用Dijkstra或BF算法。

## JM问题
通过将原图转换为一个同时包含代表奇数边的奇节点和代表偶数边的偶节点，寻找从SE到TO的最短路径。

通过将原图转换为一个G(V,E,t)的图解决不同时间边的权重不同的问题。
