# Depth - First Search (DFS), Topological Sort
## 边的分类
在DFS中，只存在tree edge和backword edge。
## 拓扑排序
对一个[有向无环图](https://baike.baidu.com/item/%E6%9C%89%E5%90%91%E6%97%A0%E7%8E%AF%E5%9B%BE/10972513)(Directed Acyclic Graph简称DAG)G进行拓扑排序，是将G中所有顶点排成一个线性序列，使得图中任意一对顶点u和v，若边$<u,v>∈E(G)$，则u在线性序列中出现在v之前。通常，这样的线性序列称为满足拓扑次序(Topological Order)的序列，简称拓扑序列。

拓扑排序就是得到拓扑序列。有很多应用，例如类加载的顺序。

拓扑排序主要有两种算法，一种是基于DFS的拓扑排序，一种是Kahn算法。