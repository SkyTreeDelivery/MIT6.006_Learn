# lec08 - Hashing with chaining
之前介绍的数据结构的各项操作的一般最优实践复杂度$O(logn)$

Hashing 数据结构的插入、查找和删除的平均时间复杂度为$O(1)$

最简单的情况是被索引的对象是整数。最简单的想法是建立一个数组（也可以称之为table）用来存放item。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220221150110.png)

足够简单，但也有两个问题：

1. 空间利用率太低
2. 不能处理非整数item

对于第二个问题，可以用一个函数将非整数对象映射到整数。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220221150155.png)

对于第一个问题，可以用一个函数将存储对象映射到一定的范围内。

这会带来新的问题，item可能会发生冲突。

一个简单的想法是使用一个list用来存放冲突的item。

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220221150205.png)

这个映射函数被称为hash。

map性能的好坏一部分取决于映射函数的选择是否能让item均匀地分散到table中。因为如果所有的item都放在一个slot里，search的性能就会退化为$O(n)$。

为了衡量table中slot的负载情况，定义了一个负载因子$α$（load factor）
$$α=n/m$$

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220221150531.png)

在理想情况下，各slot的负载均匀分布，那么map的search，insert和delete的时间复杂度就为$O(1+α)$。如果$α=O(1)$，则时间复杂度为$O(1)$。

介绍三种常使用的hash方法：

方法一：

$$h(k) = k\ mod\ m$$
m为一个不相近与2和10的指数倍的质数。很简单，但除质数太慢了。

方法二：

$$h(k)=[(a*k)\ mod\ 2^w]>>(w-r)$$

a和是一个随机数，w是计算机的字长（bit length of word）。该方法充分利用计算机硬件的特点来加速hash值的计算。

a需要满足两个条件：
1. 奇数
2. $2^{w-1}<a<2^w$，且α不接近于$2^{w-1}$和$2^w$

原理可视化如下图：

![](https://zhang113751picgo.oss-cn-hangzhou.aliyuncs.com/img/20220221151323.png)

方法三：

$$h(k) = [(ak+b)\ mod\ p]\ mod\ m$$

a和b是随机数，p和m都是一个大于宇宙原子数的质数。该hash函数保证在最差的情况下也能使负载均衡。当然相比于方法二，运算效率就低很多了。

## 代码实现
```java
public class HashTable<K,V> {
    private int count = 0;
    private double loadFactor = 0.75;
    private int maxSize;
    private int tableSize = 8;
    private Node[] table;

    public HashTable(){
        table = new Node[tableSize];
        maxSize = (int) (loadFactor * tableSize);
    }

    public HashTable(int loadFactor){
        this.loadFactor = loadFactor;
        table = new Node[tableSize];
        maxSize = (int) (loadFactor * tableSize);
    }

    public void put(K key, V value){
        changeSize();
        Entry<K, V> kvEntry = new Entry<>(key, value);
        Node<K, V> kvNode = new Node<>(kvEntry, null);
        int index = getIndex(key);
        Node tableTest = table[index];
        if(tableTest == null){
            table[index] = kvNode;
        }else{
            while (tableTest.next != null){
                tableTest = tableTest.next;
            }
            tableTest.next = kvNode;
        }
        count++;
    }

    private int getIndex(K key){
        return key.hashCode() % tableSize;
    }

    public V get(K key){
        int index = getIndex(key);
        int findKeyHash = key.hashCode();
        for (Node tableTest = table[index]; tableTest != null; tableTest = tableTest.next){
            if(tableTest.entry.hash == findKeyHash && tableTest.entry.key.equals(key)){
                return (V) tableTest.entry.value;
            }
        }
        return null;
    }

    public V remove(K key){
        int index = getIndex(key);
        int findKeyHash = key.hashCode();
        for (Node tableTest = table[index], lastNode = null; tableTest != null; lastNode = tableTest, tableTest = tableTest.next){
            if(tableTest.entry.hash == findKeyHash && tableTest.entry.key.equals(key)){
                lastNode.next = tableTest.next;
                return (V) tableTest.entry.value;
            }
        }
        return null;
    }

    private void changeSize(){
        if(count == maxSize){
            enlarge();
        }
//        if(count == maxSize / 4){
//            enSmall();
//        }
    }

    private void enlarge(){
        int newTableSize = tableSize * 2;
        Node[] newTable = new Node[newTableSize];
        for(int i = 0; i < tableSize; i++){
            for (Node testNode = table[i]; testNode != null; testNode = testNode.next){
                int newIndex = testNode.entry.hash % newTableSize;
                testNode.next = newTable[newIndex];
                newTable[newIndex] = testNode;
            }
        }
        table = newTable;
        tableSize = newTableSize;
        maxSize = (int) (tableSize * loadFactor);
    }
}
```
