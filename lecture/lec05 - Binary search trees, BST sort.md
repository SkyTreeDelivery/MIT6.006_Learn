# lec05 - Binary search trees, BST sort
## represenation invariant
一个数据结构可以存储和查询信息，其中存储信息的操作被称为**更新**（update），而查询信息的操作被称为**查询**（query）

- query
	- min
	- max
	- search
- update
	- insert
	- delete

每个数据结构的一个重要的属性是**represenetation invariant**（RI，rep invariant），即该数据结构所遵循的一些在操作之后不改变的性质。例如有序数组的RI是元素是有序的，堆的RI是每个节点大于等于或小于等于其子节点，BST的RI是每个节点大于其左子树并小于其右子树。

## Binary search trees(BST)
BST，也就是**二叉搜索树**。和堆不同，BST的约束是每个节点的左子树中的节点都小于该节点，右子树中的节点都大于该节点。

虽然堆和BST都是二叉树，但两者存在不同。如果说堆是一个无序数组，只是表示成了树的样子，是一个仿真树，那么二叉树就是有通过指针串联的节点构成的真树。

基本的BST的一些操作的时间复杂度：
- query
	- min: $O(logn)$
	- max: $O(logn)$
	- search: $O(logn)$
	- next_larger: $O(logn)$
- update
	- insert: $O(logn)$
	- delete: $O(logn)$

next_larger操作是一个寻找目标节点的**后继节点**（successor node）的过程。
## Argument BST
如果想要进一步降低每个操作的时间复杂度，可以设计增强的BST，例如在每个节点中增加一个属性记录其子树的最小值，那么min操作的时间复杂度就降低为$O(1)$。

相比于基础BST，每个增强的BST的添加了新的RI，在进行操作时需要维持这些RI的一致性。例如在插入和删除操作中更新每个节点的min属性，而插入和删除操作的时间复杂度仍为$O(logn)$。
## 保持平衡
BST可能是不平衡的，在这种情况下BST的插入和删除操作的时间复杂度会退化为$O(n)$。在[[lec06 - AVL Trees, AVL Sort | 第六课]]中会介绍一种平衡二叉树——AVL Trees。

## BST树实现
