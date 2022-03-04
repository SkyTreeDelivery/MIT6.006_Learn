# lec11 - Integer arithmetic, Karatsuba multiplication
## 牛顿法
通过迭代计算方程数值解。

$$
x_{i+1}=x_i-\frac{f\left( x_i \right)}{f^{'}\left( x_i \right)}
$$
## Kartsuba乘法
kartsuba乘法通过递归的方式以$n^{log3}$的时间复杂度实现大数乘法。Kartusuba算法的递归算式为。
$$T(n)=3T(n/2)+O(n)$$

```java
 public long bigMulti2(long num1, long num2, int length){
        if(length <= 2){
            return num1 * num2;
        }
        int half = length >> 1;
        long a = getHighHalf(num1, half);
        long b = getLowHalf(num1, half);
        long c = getHighHalf(num2, half);
        long d = getLowHalf(num2, half);
        long z0 = bigMulti2(a,c,half);
        long z2 = bigMulti2(b,d,half);
        long z1 = bigMulti2(a+b,c+d,half) - z0 - z2;
        return (z0 << (length)) + (z1 << half) + z2;
    }

    private long getHighHalf(long num, int half){
        return num >> half;
    }

    private long getLowHalf(long num, int half){
        return num & ((1L << half) - 1);
    }
```

如果将一个大数分为三段，并基于数学技巧减少乘法的次数，可以得到$n^{\log _{3}^{5}}$时间复杂度的算法，递归式为$$T(n)=5T(n/3)+O(n)$$
现在最优的时间复杂度为$O(nlognloglogn)$。