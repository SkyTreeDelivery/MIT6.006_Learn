# lec06 - AVL Trees, AVL Sort
```toc
```
## 定义平衡
一个树是否平衡取决于对于平衡的定义，严格的平衡指任意一个节点的左右子树的高度之差小于等于1

一个BST树的插入、搜索与删除操作的时间复杂度只能称之为O(h)，h表示树的高度。如果树的平衡的，那么$h=logn$，如果树是不平衡的，h可能退化为n。

只有一个balanced BST的插入、搜索与删除的时间复杂度才为O(logn)。

## 平衡树的最坏时间复杂度
当平衡树最不平衡是，平衡树具有最坏时间复杂度。

即左右子树之间的高度相差1。用公式表达的话，就是：

$$
N_h=N_{h-1}+N_{h-2}+1 > 2 *N_{h-2}>4*N_{h-4}>2^{h/2}
$$
因此
$$
h<\ 2*log _nN_h
$$

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220217170317.png)


## 左旋、右旋与重平衡
### 左旋、右旋
首要的需要解决的问题是如何在插入的过程中保持树的平衡。

主要手段是通过左旋与右旋操作，当树失去平衡时恢复树的平衡。

左旋代码与优选代码需要注意的2点

1. 注意**边界情况**，即father，left和right为null的节点，并添加相应的判断

2. 注意**节点连接关系改变的先后顺序**。如果连接关系变化了，后续的代码就不要用原始的连接关系

左旋代码与右旋代码，左旋与右旋的区别是：

1. 交换了A和B
1. 交换了left与right

```java
private void left_rotate(Node node){
        if(node == null){
            return;
        }
        Node A = node;
        Node B = node.right;
        Node O = A.father;
        if(O != null){
            if(O.left != null && O.left.father == O) O.left = B;
            if(O.right != null && O.right.father == O) O.right = B;
        }
        A.father = B;
        B.father = O;
        B.left.father = A;
        A.right = B.left;
        B.left = A;
        if(O == null) root = B;
    }

private void right_rotate(Node node){
        Node B = node;
        Node A = node.left;
        Node O = B.father;
        if(O != null){
            if(O.right != null && O.right.father == O) O.right = A;
            if(O.left != null && O.left.father == O) O.left = A;
        }
        B.father = A;
        A.father = O;
        A.right.father = B;
        B.left = A.right;
        A.right = B;
        if(O == null) root = A;
    }
```

### 再平衡

再平衡分为两种情况

情况一：只旋转一次

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220216161557.png)

情况二：旋转两次，先右后左

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220216161716.png)

重平衡代码

```java
private void reBalance(Node node){
        reHeight();
        while (node != null){
            // 左轻右重
            if(height(node.left) + 2 <= height(node.right)){
                if(height(node.left) > height(node.right)){
                    right_rotate(node.right); // 先进行右旋
                }
                left_rotate(node);
                reHeight();
            }
            //右轻左重
            if(height(node.right) + 2 <= height(node.left) ){
                if(height(node.right) > height(node.left)){
                    left_rotate(node.left); // 先进行左旋
                }
                right_rotate(node);
                reHeight();
            }
            node = node.father;
        }
    }
```

