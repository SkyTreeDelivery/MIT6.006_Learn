# lec09 - Table Doubling, Karp-Rabin
## hash表的增长与收缩

在[[lec08 - Hashing with chaining]]中，我们引入了哈希表，但此时的hash表不能应对不断增长的item。

我们希望hash表在溢出时自动增长。

有几种策略：
1. 溢出时将容量+1
2. 溢出时将容量*2

第一种方法在溢出后的每次操作的时间复杂度都退化为$O(n)$,n次操作的时间复杂度为$O(n^2)$。第二种方法的平均时间复杂度为$O(n)$，因此选择方法2。

与之类似的，Python中的list类型也是采用了动态增长模式，每次添加的均摊时间复杂度为$O(1)$。

有了动态增长，自然有动态收缩，一个简单的想法是和动态增长保持一致，在item数量为$2^n$时自动收缩。

但有个问题，如果在增长的边界进行反复的插入和删除，哈希表的时间复杂度会退化为$O(n)$。一个好的方案是在item数为$m+1$时增长为$2*m$，在item数为$m/4$时收缩为$m/2$。这样可以保证在增长和收缩后不会立即进行收缩和增长，从而使均摊时间复杂度变为$O(1)$。

## 字符串搜索 Karp-Rabin
### 理论
在一个字符串中查找子串，最简单的方法如下图所示。一次比较每一个可能的选项，假设查找的子串的长度为L，由于字符串之间的比较的最坏时间复杂度为O(L)，算法的时间复杂度为$$O(L*(n-L))=O(L*n)$$

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220223165855.png)

从理论上分析，每个可能的候选方案都需要被筛选，因此字符串搜索算法的理论最优实践复杂度应该为$O(n)$。

Karp-Rabin方法就是一种时间复杂度为$O(n)$的字符串搜索算法。Karp-Rabin基于滑动窗口和hash的思想实现，也可称之为[[滑动hash]]。

滑动hash的ADT包括3个方法：
- hash，计算hash值，可以是将结果映射到一个更小的集合，也可以是一个等大的但效率更高的集合
- append，向窗口中添加一个item
- skip，从窗口中移除一个item

对于字符串搜索算法来说，由于ASSCI字符集只有256个字符，因此hash可以将一个字符串映射为一个底为256的数字。在一般的情况中，字符串可能会很大，因此需要取模来控制数字的规模。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220223171140.png)

在[[leetcode 187 重复的DNA序列]]的一种实现中也使用了滑动窗口的思想

### 实现
Java实现

```java
class RollingHash {
    public int hash;
    public int t_hash;
    public String s;
    public String t;
    public int p;
    public int base = 256;
    public int magic;

    public RollingHash(String s, String t, int p) {
        this.s = s;
        this.t = t;
        this.p = p;
        this.magic = getMagic(); // 计算magic
        this.t_hash = hash(t, 0, t.length()); // 计算t的hash值;
    }

    /**
     * 在字符串s中搜索t
     * @return
     */
    public int search() {
        this.hash = hash(s, 0, t.length());
        int hitNum = 0;
        if (this.hash == this.t_hash && s.startsWith(t)) {
            hitNum++;
        }
        for (int i = 0; i < s.length() - t.length(); i++) {
            roll(s.charAt(i), s.charAt(i + t.length()));
            if (this.hash == this.t_hash && s.substring(i + 1, i + 1 + t.length()).equals(t)) {
                hitNum++;
            }
        }
        return hitNum;
    }

    /**
     * 对s的子串计算hash值
     * @param s
     * @param start
     * @param length
     * @return
     */
    private int hash(String s, int start, int length) {
        int sum = 0;
        for (int i = start, x = length - 1; i < start + length; i++, x--) {
            int baseFactor = 1;
            for (int k = 0; k < x; k++)
                baseFactor = (baseFactor * base) % p;
            sum += (s.charAt(i) * baseFactor) % p;
        }
        return sum % p;
    }

    /**
     * 窗口滑动一次
     * @param oldChar
     * @param newChar
     */
    private void roll(char oldChar, char newChar) {
        hash = ((hash * base - (oldChar * magic)) % p + newChar) % p;
    }

    /**
     * 计算magic常量
     * @return
     */
    private int getMagic() {
        int baseFactor = 1;
        for (int k = 0; k < t.length(); k++)
            baseFactor = (baseFactor * base) % p;
        return baseFactor;
    }
}
```



