# lec12 - Square roots, Newton's method
## 基于牛顿法实现开方
原始函数
$$f(x)=x^2-a$$
递推式
$$
x_{i+1}=\frac{x_i+\frac{a}{x_i}}{2}
$$
在数值解的递推中用到了除法，后面介绍了基于牛顿法实现的除法计算。

实现
```java
public double sqrt2(double num){
        double lastTest = num;
        double testnum = (lastTest + (num / lastTest)) / 2;
        double error = 0.00000001;
        while (lastTest - testnum > error){
            lastTest = testnum;
            testnum = iter_sqrt(num, testnum);
        }
        return testnum;
    }
```


## 基于牛顿法实现除法
原始函数
$$f(x)=\frac{1}{x}-\frac{b}{R}$$

递推式
$$
x_{i+1}=2x_i-\frac{bx_{i}^{2}}{R}
$$
R的作用是将整个数字放大R倍，利用整数完成计算，然后再还原为小数。

由于Java中数字位数的限制，只能计算32位二进制精度的除法，大概是小数点后10位精度。

实现

```java
public double divide3(double a, double b, int precision){
        long lastnum = ((1L << (precision - Integer.toBinaryString((int)b).length())));
        long testnum = iter_divide(lastnum, b, precision);
        while(Math.abs(lastnum - testnum ) >= 2){
            lastnum = testnum;
            testnum = iter_divide(testnum, b, precision);
        }
        return tofloat((long) (testnum * a), precision);
    }

    private double tofloat(long num, int precision){
        // 小数映射map
        Map<Integer, Double> dotMap = new HashMap<>();
        dotMap.put(1, 0.5);
        for (int i = 2; i <= precision; i++) {
            dotMap.put(i, dotMap.get(i-1) / 2);
        }
        // 正数部分
        long heighNum = num >> precision;
        // 求小数部分
        double sum = 0.0;
        for (int i = 1; i < precision; i++) {
            long bitnum = (num & (1L << (i - 1))) >> (i - 1);
            double v = bitnum == 1 ? dotMap.get(precision - i + 1) : 0;
            sum += v;
        }
        return sum + heighNum;
    }
```
