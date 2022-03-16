# lec20 - Parent pointers; text justification, perfect-information blackjack
## 动态规划的一般解题思路和模板
动态规划的步骤：
1. 定义子问题(sub-problem)（定义状态）。
2. 猜测(guess)子问题之间的关系（定义状态之间的转换）。
3. 写出递归式(recurse)并添加记忆(memoize)
4. 找出顺序（topo）（可选）
5. 通过某个子问题或者组合几个子问题来解决

最难的是找出子问题，然后是定义子问题之间

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316170244.png)

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316170615.png)

### 常见的动态规划模板
dp[i] = min(dp[j]) for j in i to n

dp[i] = max(dp[j]) for j in i to n if xxx

## 文字排版
首先定义坏排版。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316171157.png)

最好的排版就是所有行的坏排版综合最小。

![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316171608.png)
![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316171619.png)

## 21点
![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316172223.png)
![](https://gitee.com/skytreedelivery/cloudimage/raw/master/img/20220316172241.png)

## 最长递增子序列
最长增长序列的解是dp中的dp[i] for i in 0 to n中的最大值，而不是dp[0]，这是因为之前的问题都是只能从i=0开始，而该问题可以从任意位置开始。

subproblem: dp[i]=从i到n之间的最长增长子序列

guess：n-i个choice。

recursion：dp[i] = max(len(dp[j])) for j in i+1 to n if A[i] < A[j]，dp[n] = 0

memosize: dpmemo = {i, dp[i]}

order: n-> 0

solution：max(dpmemo)