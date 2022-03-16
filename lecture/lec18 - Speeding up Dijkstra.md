# lec18 - Speeding up Dijkstra
## 双端Dijkstra
双端Dijkstra算法分别从S和T节点计算Dijkstra，直到两端接触。

但首个接触点不一定是最短路径中的节点，因此需要将接触之前计算过的所有边缘节点到对边的最短路径都计算出来，从中找到实际上的最短路径。
## A*
启发式方法，调整边的权重。
$$w'(x, y) = w(x, y) + h(y) − h(x)$$

当$w'(x,y)≥0$时，即$w(x,y)≥h(x)-h(y)$，可以应用dijkstra算法，否则只能使用Bellman-Ford算法。
## landmark
LCA