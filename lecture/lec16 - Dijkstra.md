# lec16 - Dijkstra
## Dijkstra算法的一般形式

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220310181840.png)

## Dijkstra算法不同实现的时间复杂度对比

基于数组的实现：$O(V^2)$
基于堆的实现：$O((V+E)logV)$
基于斐波那契堆的实现：$O(VlogV+E)$

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220310182158.png)

## Rubik's Cube, StarCraft Zero
在Rec中，vertor介绍了两个问题，2\*2\*2魔方问题和星际争霸问题

这两个问题都是用了状态机来解决问题，即构建一个Graph，节点表示可能的状态，边表示在状态之间进行转换的动作。

### Rubik's Cube
对于魔方问题，节点就是一个长度为24的数组，表示魔方面的所有可能排列。边表示魔方在不同状态之间的转换，对应现实中旋转一个魔方块。

节点/状态的建模：

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220311113916.png)

边/转换（move）的建模：

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220311113936.png)

这里面涉及Graph表达方式的选择：显示Graph和隐式Graph。显示的Graph提前构建一个包含所有可能情况的Graph，对于较小的问题，这是可行的，但是对大Graph不友好。例如2\*2\*2的魔方问题，显示构建Graph的空间复杂度为$O(24!)$
$$24!=620448401733239439360000$$
我们可以隐式地去构建Graph，每当访问一个节点时，才计算它的邻接节点，可以有效节约空间。

### StarCraft Zero
技能树

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220311115728.png)

对于星际争霸问题，图的节点是各种单位和建筑的状态，边是一条指令，包括建造建筑，生产兵种和提升科技。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220311121541.png)

和魔方问题一样，也是构建一个隐式Graph。和魔方问题不同的是，魔方问题存在一些确定性的节点用于描述终点。而星际争霸问题的终点是占领敌军总部，不存在一个明确的节点状态表示游戏结束，这意味着结束的节点则是不确定的，因为在很多节点都有可能赢得游戏的胜利，因此我们需要人为定义一些具体的节点作为阶段性目标，尽可能增大我们获胜的可能性。我们通过这些条件筛选出合适的节点。

例如：在开战两分钟之内派遣一定兵力骚扰敌方（描述为小狗数量要大于等于$6*log(1+M)$，M为开战时间，单位min）。在最短时间内建造150个小狗等。可以理解问题占领敌方总部是战略目标，而这些小目标则是战术目标。

在动态构建隐式Graph图时，如果一些节点无法满足条件，则不进行后续节点的推算，可以理解为未达到战术目标。通过定义这些战术条件可以有效减少节点的数量。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220311121603.png)

状态机方法和线性优化法很像，但线性优化只能用于解决状态之间不存在依赖的问题，而状态机可以解决状态之间存在依赖的问题。线性优化会给出一个最优解的状态，状态机可以给出得到最优解的路线。