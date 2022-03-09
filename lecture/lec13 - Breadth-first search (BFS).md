# lec13 - Breadth-first search (BFS)

## 握手引理
在每个有限的无向图中，奇数度的顶点数始终为偶数。多有边的入度和出度都为2E

## 图的数据结构
大体上的分类如下：
- 节点与边是独立的，节点的连接由单独的数据结构存储
	- 邻接表
	- 邻接矩阵
- 节点与边是耦合的
	- 不定义边：v.neighbours
	- 定义边：v.edges，e.nodes, e.nodet
## 边的分类
可以将边分为：
- tree edge
- forward edge
- backward edge
- cross edge

在BFS中，可以存在所有四种边
## 循环图
如果一个图中存在cross edge 等价于 一个图是循环图
