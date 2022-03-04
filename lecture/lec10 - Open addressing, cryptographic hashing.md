# lec10 - Open addressing, cryptographic hashing
## 开放寻址法（open addressing）
### 基本思想
除了链表法，实现哈希表的另一种方法是开放寻址法。开放寻址法不使用指针和链表，而是将所有的元素都放在一个数组里，通过不断地探测找到合适的位置插入和删除元素。

假设数组的长度为m，放入元素的个数为n。开放寻址法需要实现一种哈希函数,$h(k,i)$，k为被放入的元素的key，i为探测的次数，并满足如下条件。
$$i \epsilon \left\{ 1,2,...,m-1 \right\}$$$$h(k,i)\epsilon \left\{ 1,2,...,m-1 \right\}$$

开放寻址法的操作：
- insert：如果探测位置已存在item，则继续下一次探测，直到没有位置。如果存在delete flag，则插入item。
- search：如果探测的位置已存在item，则继续探测，知道没有找到。如果存在delete flag，继续探测。
- delete：删除元素后，在delete位置设置delete flag。

### hash
开放寻址法基于hash来确定每次探测的位置。常见方法有：
- 线性探测法（linear probing）：$h(k,i)=(h'(k)+i)$。
- 双hash法（double hashing）：$h(k,i)=(h_1(k)+i*h_2(k))\,mod\,m$

线性探测法简单，但容易产生群居，因为每个k都会落入到一定的cluster中，每个k只能探测数组的一部分区域，在特定情况下表现不佳。

双hash法在实践中表现良好，因为只要$h_2(k)$和m互质，则每个k都可以探测数组的任意位置。

### Uniform Hashing Assumption
Uniform Hashing Assumption：当每个key有均等的可能具有m!个探测序列中的一个时，hash函数具有最好的性能。即两个key碰撞的概率为1/m!。由于数组只有m个位置，因此探测序列数量的上限为m!。

线性探测法有m（m为质数）个可能的探测序列，因此两个key碰撞的概率为1/m。

双线性探测法有$m^2$个可能的探测序列，因此两个key碰撞的概率为$1/m^2$。m通常使用$2^n$，原因见[[为什么hash函数取余要用质数]]。双线性探测法综合考虑了碰撞概率和计算效率，效果很接近Uniform Hashing Assumption，是最好的开放寻址算法采用的hash方法之一。

假设数组的长度为m，放入元素的个数为n。$α=n/m$，$α$就是数组的负载因子，对于开放寻址法，$0≤α≤1$。每次插入花费时间的期望小于$1/(1-α)$。因此α不能太大，一般在0.5左右就需要扩容，因为随着α的增大，期望探测次数会急剧增加。

### 开放寻址法 vs 链表法
开放寻址法的优势是对内存的利用率。缺点是hash函数需要特别的设计以防止分布聚集，同时负载因子不能太大。

链表法可以拥有更大的负载因子（可以大于1），对hash函数没有防止聚合的要求。
## 密码hash(cryptographic hash)
密码hash指将一段输入字符串映射为定长字符串的hash函数，密码hash在对输入敏感的同时，具有较低的碰撞概率。对于密码hash，我们常成输入为消息（message），输出为摘要（digeset）。

相对于在hash表中使用的hash，密码hash要复杂得多，摘要也更长。

密码hash有三个可能被期望的性质：
- one-way（OW）：单向不可逆
- collision-resistance（CR）：难以找到可能解
- target collision-resistance（TCR）：难以找到原始解

密码hash常用于：
- 密码存储
- 文件改动识别
- 数字签名

常见的密码hash包括MD5，SHA-3等，其中MD5算法已经被证明不是CR的。

## 代码实现
```java
public class OpenAddressHash<K,V> {
    private int count = 0;
    private double loadFactor = 0.5;
    private int maxSize;
    private int tableSize = 8;
    private Node[] table;

    Node deleted = new Node(null, null);

    public OpenAddressHash(){
        table = new Node[tableSize];
        maxSize = (int) (loadFactor * tableSize);
    }

    public void put(K key, V value){
        if(key == null){
            return;
        }
        resize();
        for (int i = 0; i < tableSize; i++) {
            int index = getIndex(key, i);
            if(table[index] == deleted || table[index] == null){
                Entry<K, V> kvEntry = new Entry<>(key, value);
                Node<K, V> kvNode = new Node<>(kvEntry, null);
                table[index] = kvNode;
                count++;
                return;
            }
        }
    }

    /**
     * 获得key对应的value
     * @param key
     * @return
     */
    public V get(K key){
        if(key == null){
            return null;
        }
        for (int i = 0; i < tableSize; i++) {
            int index = getIndex(key, i);
            if(table[index] != null){
                if(table[index] == deleted){
                    continue;
                }
                Entry entry = table[index].entry;
                if(entry.hash == key.hashCode() && entry.key.equals(key)){
                    return (V) entry.value;
                }
            }
        }
        return null;
    }

    /**
     * 为key的第testCount次探测的下标
     * @param key
     * @param testCount
     * @return
     */
    private int getIndex(K key, int testCount){
        // 用使用质数求余
        int hash = key.hashCode();
        return ((hash % (tableSize + 1)) + testCount * (hash % tableSize + 1)) % tableSize;
    }

    private void resize(){
        if(count == maxSize){
            enlarge();
        }
    }

    public V remove(K key){
        if(key == null){
            return null;
        }
        for (int i = 0; i < tableSize; i++) {
            int index = getIndex(key, i);
            if(table[index] != null){
                Entry entry = table[index].entry;
                if(entry.hash == key.hashCode() && entry.key.equals(key)){
                    table[index] = deleted;
                    return (V) entry.value;
                }
            }
        }
        return null;
    }

    private void enlarge(){
        int newCapacity = tableSize * 2;
        Node[] newTable = new Node[newCapacity];
        for (int i = 0; i < tableSize; i++) {
            Node testNode = table[i];
            if(testNode == null){
                continue;
            }
            for (int j = 0; j < newCapacity; j++) {
                int hash = testNode.entry.key.hashCode();
                int index = ((hash % newCapacity) + newCapacity * (hash % newCapacity)) % newCapacity;
                if(newTable[index] == null){
                    newTable[index] = testNode;
                    break;
                }
            }
        }
        tableSize = newCapacity;
        table = newTable;
        maxSize = (int) (tableSize * loadFactor);
    }
}

```








