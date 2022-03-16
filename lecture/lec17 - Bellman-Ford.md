# lec17 - Bellman-Ford
Bellman-Ford算法是一个S.P.算法，通过将所有边松弛V次找到最短路径。

时间复杂度为O(VE)。BF算法并非每次迭代只在最短路径上前进一次，一次迭代中不同的边迭代次序可能会使该算法在最短路径上前进0~E次，只是在最坏情况下，可需要迭代N次才结束，因此如果两次迭代没有发边被松弛，则可以认为达到了稳定状态，即算法结束。

Bellman-Ford算法是可以应用于存在负权边的图的S.F.算法。Dijkstra的原始版本不能应用与带负权边的图，但Dijkstra的改进版本可以，如果closed的节点仍可以重新变为待检测节点，则能处理负权边。

## 最长路径问题
> 在[图论](https://zh.wikipedia.org/wiki/%E5%9B%BE%E8%AE%BA "图论")和[理论计算机科学](https://zh.wikipedia.org/wiki/%E7%90%86%E8%AB%96%E8%A8%88%E7%AE%97%E6%A9%9F%E7%A7%91%E5%AD%B8 "理论计算机科学")中，**最长路径问题**是指在给定的图中找出长度最长的[道路](https://zh.wikipedia.org/wiki/%E9%81%93%E8%B7%AF_(%E5%9B%BE%E8%AE%BA) "道路 (图论)")。一条不具有任何重复[顶点](https://zh.wikipedia.org/wiki/%E9%A1%B6%E7%82%B9_(%E5%9B%BE%E8%AE%BA) "顶点 (图论)")的路径被称为简单路径。无权图中路径的长度就是边的数量，而有权图中路径长度是边权重之和。不同的是，与此相反的[最短路径问题](https://zh.wikipedia.org/wiki/%E6%9C%80%E7%9F%AD%E8%B7%AF%E9%97%AE%E9%A2%98 "最短路问题")（不含负权环）可以在多项式时间内解决。而最长路径问题是[NP困难](https://zh.wikipedia.org/wiki/NP%E5%9B%B0%E9%9A%BE "NP困难")的，这意味着除非[P = NP](https://zh.wikipedia.org/wiki/P/NP%E9%97%AE%E9%A2%98 "P/NP问题")，否则对应于任意的图，没有办法在[多项式时间](https://zh.wikipedia.org/wiki/%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6 "时间复杂度")内解决该问题。更强的困难结果表明这个问题也是[近似](https://zh.wikipedia.org/wiki/%E8%BF%91%E4%BC%BC%E7%AE%97%E6%B3%95 "近似算法")的。但是，有一个线性时间的方法可以用于[有向无环图](https://zh.wikipedia.org/wiki/%E6%9C%89%E5%90%91%E6%97%A0%E7%8E%AF%E5%9B%BE "有向无环图")，这对于发现调度问题中的[关键路径](https://zh.wikipedia.org/wiki/%E5%85%B3%E9%94%AE%E8%B7%AF%E5%BE%84 "关键路径")有重要的作用。

最长路径是一个NP完全问题，但在某些情况下，可以将最长路径问题转化为最短路径问题。如果Graph是一个DAG，则可以通过Topo sort + DFS解决（动态规划）。