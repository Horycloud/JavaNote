## 动态规划技巧*

[labuladong](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie)

算法题，其实不是先有了公式或方法才有思路，而是先有思路、把思路用公式、算法表示出来才有了公式、算法。`dp`的递推公式不是本质，本质是理解动态规划是带记事本的暴力循环

`重叠子问题、最优子结构、状态转移方程、状态压缩`

> 最优子结构

一个问题具有`最优子结构`，那么就可以考虑用`动态规划`来解。

将这个问题分治，最优子结构间相互独立，当每个子结构都取得最优解，最终由子结构组成的总问题也得到了最优解。

要符合最优子结构，子问题间必须互相独立，什么叫互相独立，举个例子：

假设考试，每门科目的成绩都是互相独立的。你的原问题是考出最高的总成绩，那么你的子问题就是要把语文考到最高，数学考到最高…… 为了每门课考到最高，你要把每门课相应的选择题分数拿到最高，填空题分数拿到最高…… 当然，最终就是你每门课都是满分，这就是最高的总成绩。

得到了正确的结果：最高的总成绩就是总分。因为这个过程符合最优子结构，“每门科目考到最高”这些子问题是互相独立，互不干扰的。

但是，如果加一个条件：你的语文成绩和数学成绩会互相制约，数学分数高，语文分数就会降低，反之亦然。这样的话，显然你能考到的最高总成绩就达不到总分了，按刚才那个思路就会得到错误的结果。因为子问题并不独立，语文数学成绩无法同时最优，所以最优子结构被破坏。



## 动态规划必练*

> Leetcode必须练动态规划23道

|                           力扣中国                           |                           力扣国际                           |  难度  |      |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----: | ---- |
| [746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/) | [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs) |  Easy  |      |
| [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/) | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs) |  Easy  |      |
| [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/) | [Unique Paths](https://leetcode.com/problems/unique-paths/)  |  Easy  |      |
| [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/) | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) | Medium |      |
| [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/) | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) |  Easy  |      |
| [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/) | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/) | Medium |      |
| [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/) | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock) |  Easy  |      |
| [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/) | [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) |  Easy  |      |
| [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/) | [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) |  Hard  |      |
| [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/) | [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) |  Hard  |      |
| [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) | Medium |      |
| [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) | [Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee) | Medium |      |
| [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/) |   [Coin Change](https://leetcode.com/problems/coin-change)   | Medium | √    |
| [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/) | [Coin Change 2](https://leetcode.com/problems/coin-change-2) | Medium | √    |
| [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/) | [Edit Distance](https://leetcode.com/problems/edit-distance) |  Hard  | √    |
| [115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/) | [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/) |  Hard  |      |
| [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/) | [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) |  Hard  |      |
| [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/) | [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/) |  Hard  |      |
| [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/) | [Maximal Square](https://leetcode.com/problems/maximal-square/) | Medium |      |
| [983. 最低票价](https://leetcode-cn.com/problems/minimum-cost-for-tickets/) | [Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/) | Medium |      |
|                                                              | [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) |  Easy  |      |
|                                                              | [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/) | Medium |      |
|                                                              | [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence) | Medium |      |



## 斐波那契数列

`fib(n)`为第`n`项的值

```java
public static int fib(int n){
  	if(n == 1 || n == 2) return n;
  	return fib(n - 1) + fib(n - 2);
}
```

**带备忘录的解法**：

```java
public static int fib(int n){
  	if(n < 1) return 0;
  	int[] memo = new int[n+1]; // 初始化备忘录，值全为0
  	return helper(memo, n);    // 进行带备忘录的递归
}

private static int helper(int[] memo, int n)[
  	if(n == 1 || n == 2) return 1;
  	if(memo[0] != 0) return memo[n];
  	memo[n] = helper(memo, n-1) + helper(memo, n-2);
  	return memo[0];
]
```



## 0-1 背包

[labuladong](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247485064&idx=1&sn=550705eb67f5e71487c8b218382919d6&chksm=9bd7f880aca071962a5a17d0f85d979d6f0c5a5ce32c84b8fee88e36d451f9ccb3bb47b88f78&scene=21#wechat_redirect)

> 给你一个可装载重量为`W`的背包和`N`个物品（每个物品不一样），每个物品有重量和价值两个属性。其中第`i`个物品的重量为`wt[i]`，价值为`val[i]`，现在让你用这个背包装物品，最多能装的价值是多少？

**示例：**

输入：

```python
W = 4, N = 3
wt = [2, 1, 3]
val = [4, 2, 3]
```

输出：6

解释：选择前两件物品装进背包，总重量 3 小于`W`，可以获得最大价值 6。

这个题目中的物品不可以分割，要么装进包里，要么不装，不能说切成两块装一半。这也许就是 `0-1` 背包这个名词的来历。

> **动态规划套路**

1. **第一步要明确两点：`「状态」`和`「选择」`**

先说状态，如何才能描述一个问题局面？只要给定几个可选物品和一个背包的容量限制，就形成了一个背包问题，对不对？所以状态有两个，就是「背包的容量」和「可选择的物品」。

再说选择，也很容易想到啊，对于每件物品，你能选择什么？选择就是「装进背包」或者「不装进背包」嘛。

明白了状态和选择，动态规划问题基本上就解决了，只要往这个框架套就完事儿了：

```
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 择优(选择1，选择2...)
```

2. **第二步要明确`dp`数组的定义**

`dp`数组是什么？其实就是描述问题局面的一个数组。换句话说，我们刚才明确问题有什么「状态」，现在需要用`dp`数组把状态表示出来。

首先看看刚才找到的「状态」，有两个，也就是说我们需要一个二维`dp`数组，一维表示可选择的物品，一维表示背包的容量。

**`dp[i][w]`的定义如下：对于前`i`个物品，当前背包的容量为`w`，这种情况下可以装的最大价值是`dp[i][w]`。**

比如说，如果 dp[3][5] = 6，其含义为：对于给定的一系列物品中，若只对前 3 个物品进行选择，当背包容量为 5 时，最多可以装下的价值为 6。

PS：为什么要这么定义？便于状态转移，或者说这就是套路，记下来就行了。建议看一下我们的动态规划系列文章，几种动规套路都被扒得清清楚楚了。

**根据这个定义，我们想求的最终答案就是**`dp[N][W]`。

base case 就是`dp[0][..] = dp[..][0] = 0`，因为没有物品或者背包没有空间的时候，能装的最大价值就是 0。

细化上面的框架：

```
int dp[N+1][W+1]
dp[0][..] = 0
dp[..][0] = 0

for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            把物品 i 装进背包,
            不把物品 i 装进背包
        )
return dp[N][W]
```

3. **第三步，根据`「选择」`，思考状态转移的逻辑**

简单说就是，上面伪码中「把物品`i`装进背包」和「不把物品`i`装进背包」怎么用代码体现出来呢？

这一步要结合对`dp`数组的定义和我们的算法逻辑来分析：

先重申一下刚才我们的`dp`数组的定义：

`dp[i][w]`表示：对于前`i`个物品，当前背包的容量为`w`时，这种情况下可以装下的最大价值是`dp[i][w]`。

+ **如果没有把这第**`i`个物品装入背包，那么很显然，最大价值`dp[i][w]`应该等于`dp[i-1][w]`。

+ **如果把这第**`i`个物品装入了背包，那么`dp[i][w]`应该等于`dp[i-1][w-wt[i-1]] + val[i-1]`。

首先，由于`i`是从 1 开始的，所以对`val`和`wt`的取值是`i-1`。

而`dp[i-1][w-wt[i-1]]`也很好理解：你如果想装第`i`个物品，你怎么计算这时候的最大价值？**换句话说，在装第**`i`个物品的前提下，背包能装的最大价值是多少？

显然，你应该寻求剩余重量`w-wt[i-1]`限制下能装的最大价值，加上第`i`个物品的价值`val[i-1]`，这就是装第`i`个物品的前提下，背包可以装的最大价值。

综上就是两种选择，我们都已经分析完毕，也就是写出来了状态转移方程，可以进一步细化代码：

```
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```

4. **最后一步，把伪码翻译成代码，处理一些边界情况**

处理`w - wt[i-1]`可能小于 0 导致数组索引越界的问题



这里遍历数组的顺序就是正着遍历

| index |  0   |  1   |  2   |  3   |  4   |  5   |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: |
|  wt   |      |      |      |      |      |      |
|  val  |      |      |      |      |      |      |

![](/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/01背包.png)

完整代码如下：

```java
class Solution {
  	public int knapSack (int W, int N, int[] wt, int[] val){
      	int[][] dp = new int[N+1][W+1];
      	for(int i = 1; i <= N; i++){
          	for(int j = 1; j <= W; j++){
              	if(j-wt[i-1] < 0){
                  	dp[i][j] = dp[i-1][j];  // 当前背包容量装不下，只能选择不装入背包
                }else {
                  	// 装入或者不装入背包，择优
                  	dp[i][j] = Math.max(dp[i-1][j-wt[i-1]]+val[i-1],
                                        dp[i-1][j] );
                }
            }
        }
      	return dp[N][W];
    }
}
```

> 时间复杂度：O(n^2)
>
> 空间复杂度：O(n^2)



## L322.零钱兑换

[322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

>给定不同面额的硬币` coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

零钱兑换问题符合最优子结构，比如你想求 `amount = 11` 时的最少硬币数（原问题），如果你知道凑出 `amount = 10` 的最少硬币数（子问题），你只需要把子问题的答案加一（再选一枚面值为 1 的硬币）就是原问题的答案。因为硬币的数量是没有限制的，所以子问题之间没有相互制，是互相独立的。

**如何列出正确的状态转移方程**？

首先要明确我们的目的：用最少的硬币组成目标金额

1. **确定 base case**

	这个很简单，显然目标金额 `amount` 为 0 时算法返回 0，因为不需要任何硬币就已经凑出目标金额了。

2. **确定「状态」，也就是原问题和子问题中会变化的变量**。

	由于硬币数量无限，硬币的面额也是题目给定的，只有目标金额会不断地向 base case 靠近，所以唯一的「状态」就是目标金额 `amount`。

3. **确定「选择」，也就是导致「状态」产生变化的行为**。

	目标金额为什么变化呢，因为你在选择硬币，你每选择一枚硬币，就相当于减少了目标金额。所以说所有硬币的面值，就是你的「选择」。

4. **明确** **`dp`** **函数/数组的定义**。

	这里直接一步到位，采用自底向上的做法，数组`dp[amount]`代表当目标金额为`amount`时需要的最少硬币数（即最终的解）。

	状态转移方程为：

	$$dp(n)= \begin{cases}0, \quad n = 0 \\-1, \quad n<0 \\min\{  dp(n-coin) + 1 \quad  | \quad coin \in coins \},\quad  n>0 \end{cases}$$

`dp(n)` 的定义：输入一个目标金额 `n`，返回凑出目标金额 `n` 的最少硬币数量。

PS：为啥 `dp` 数组初始化为 `amount + 1` 呢，因为凑成 `amount` 金额的硬币数最多只可能等于 `amount`（全用 1 元面值的硬币），所以初始化为 `amount + 1` 就相当于初始化为正无穷，便于后续取最小值。

```java
import java.util.*;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        Arrays.fill(dp, amount+1);
        dp[0] = 0;
        for(int i = 0; i <= amount; i++){
            for(int coin:coins){
                if(i-coin >= 0){
                    dp[i] = Math.min(dp[i-coin]+1, dp[i]);
                }
            }
        }
        return (dp[amount] == amount+1) ? -1 : dp[amount];
    }
}
```

> 时间复杂度：`O(kN)`，`k`为硬币面值的种类数
>
> 空间复杂度：`O(N)`



## L518.零钱兑换II**

[518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

**这是一个完全背包问题**

> 给定不同面额的硬币`coins`和一个总金额`amount`。写出函数来计算可以凑成总金额的硬币`组合数`。假设每一种面额的硬币有无限个。 

这道题是在`0-1`背包问题的变形，是完全背包问题，那么什么是完全背包问题呢?

**再来回顾一下0-1背包的题目**：给你一个可装载重量为`W`的背包和`N`个物品（每个物品不一样），每个物品有重量和价值两个属性。其中第`i`个物品的重量为`wt[i]`，价值为`val[i]`，现在让你用这个背包装物品，最多能装的价值是多少？

`0-1`背包问题中物品的数量是有限的，准确来说每个物品都是独一无二的，而这里的每一种面额的硬币有无限个，这就是`完全背包问题`，思路跟0-1背包的思路是一样的，只是状态转移方程略有改变而已。

**我们可以根据`0-1`背包的思路来解这道题**：

我们将不同面额的硬币比作`0-1`背包问题中的物品，将总金额比作背包的容量，这样就可以转化为背包问题的模型来进行求解。

1. **第一步：明确「状态」和「选择」**

状态有两个，就是「背包的容量」和「可选择的物品」，选择就是「装进背包」或者「不装进背包」。

```java
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 计算(选择1，选择2...)
```

2. **明确** **`dp`** **数组的定义**

`dp[i][j]` 的定义如下：只使用前 `i` 个物品，当背包容量为 `j` 时，有 `dp[i][j]` 种方法可以装满背包。

转换为硬币兑换的模型就是：只使用`coins`中的前`i`个硬币的面值，若想凑出金额`j`，有 `dp[i][j]`种凑法。

base case 为 `dp[0][..] = 0， dp[..][0] = 1`。因为如果不使用任何硬币面值，就无法凑出任何金额；如果凑出的目标金额为 0，那么“无为而治”就是唯一的一种凑法。

我们最终目的就是求 `dp[N][amount]`，其中 `N` 为 `coins` 数组的大小。

3. **根据「选择」，思考状态转移的逻辑**

如果不使用 `coins[i]` 这个面值的硬币，那么凑出面额 `j` 的方法数 `dp[i][j]` 应该等于 `dp[i-1][j]`，继承之前的结果。

如果使用 `coins[i]` 这个面值的硬币，那么 `dp[i][j]` 应该等于 `dp[i][j-coins[i-1]]`。

首先由于 `i` 是从 1 开始的，所以 `coins` 的索引是 `i-1` 时表示第 `i` 个硬币的面值。

`dp[i][j-coins[i-1]]` 也不难理解，如果你决定使用这个面值的硬币，那么就应该关注如何凑出金额 `j - coins[i-1]`。

综上就是两种选择，而我们想求的 `dp[i][j]` 是「共有多少种凑法」，所以 `dp[i][j]` 的值应该是以上两种选择的结果之和：

```java
class Solution {
    int change(int amount, int[] coins) {
        int n = coins.length;
        int[][] dp = amount int[n + 1][amount + 1];
        for (int i = 0; i <= n; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= amount; j++){
              	if (j - coins[i-1] >= 0){
                    dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]];
                }else {
                    dp[i][j] = dp[i - 1][j];
                }
            }   
        }
        return dp[n][amount];
    }
}
```

而且，我们通过观察可以发现，`dp` 数组的转移只和 `dp[i][..]` 和 `dp[i-1][..]` 有关：

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/完全背包.png" style="zoom:50%;" />

所以可以压缩状态，进一步降低算法的空间复杂度：

```java
class Solution {
    public int change(int amount, int[] coins) {
        int n = coins.length;
        int[] dp = new int[amount + 1];
        dp[0] = 1; 
      
        for (int i = 0; i < n; i++){
            for (int j = 1; j <= amount; j++){
                if (j - coins[i] >= 0){
                    dp[j] += dp[j-coins[i]];
                }
            }
        }
        return dp[amount];
    }
}
```

甚至可以这样：

```java
class Solution {
    public int change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;

        for (int coin : coins) {
            // j直接从coin开始，避免了j - coins[i] >= 0的判断
            for (int j = coin; j <= amount; j++) {   
              dp[j] += dp[j - coin];
            }
        }
        return dp[amount];
    }
}
```

> 时间复杂度 O(N*amount)
>
> 空间复杂度 O(amount)



## 子序列解题模板

[动态规划设计之最长递增子序列](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484498&idx=1&sn=df58ef249c457dd50ea632f7c2e6e761&chksm=9bd7fa5aaca0734c29bcf7979146359f63f521e3060c2acbf57a4992c887aeebe2a9e4bd8a89&scene=21#wechat_redirect)

[子序列解题模板：最长回文子序列](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484666&idx=1&sn=e3305be9513eaa16f7f1568c0892a468&chksm=9bd7faf2aca073e4f08332a706b7c10af877fee3993aac4dae86d05783d3d0df31844287104e&scene=21#wechat_redirect)

>注意`「子序列」`和`「子串」`这两个名词的区别：
>
>子串一定是连续的，而子序列不一定是连续的。

`nums[]`的子序列问题都是将问题分为以`nums[i]`为结尾的子序列：

+ 如果求的是最长子序列长度，那么最终必定为以某一`nums[i]`为结尾的子序列的长度



## 数组最大值个数

> 只遍历一次找出数组最大值个数

```java
public static int find(int[] arr){
  	int len = arr.length;
  
  	int MAX = 0;
  	int cnt = 0;
  	for(int i = 0; i < len; i++){
      	if(arr[i] > MAX){
          	MAX = arr[i];
          	cnt = 1;
        }else if(arr[i] == MAX){
          	cnt++;
        }
    }
  	return cnt;
}
```



## 最长递增子序列

> 给定一个`无序`的整数数组，找到其中最长上升子序列的`长度`。

**注意：**这里没要求子序列连续。

**示例：**

```
输入：[10, 9, 2, 5, 3, 7, 101, 18]
输出：4
解释：最长的上升子序列是[2, 3, 7, 101]，长度为4
```

**说明：**

+ 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
+ 算法的时间复杂度应该为O(n^2)

**进阶：**

+ 你能将算法的时间复杂度降到O(nlgn)吗？

**方法一：** 动态规划

**思路：**

首先要定义 dp 数组的含义：`dp[i]` 表示以 `nums[i]` 这个数`结尾`的最长递增子序列的长度。

比如：

| index |  0   |  1   |  2   |  3   |  4   |  5   |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: |
| nums  |  1   |  4   |  3   |  4   |  2   |  3   |
|  dp   |  1   |  2   |  2   |  3   |  2   |  ?   |

算法演进的过程如下：

![](/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/最长递增子序列长度.gif)

根据这个定义，我们的最终结果（子序列的最大长度）应该是 dp 数组中的最大值，即：

```java
int res = 0;
for(int i = 0; i < dp.length; i++){
  	res = Math.max(dp[i], res);
}
return res;
```

求状态转移方程：

根据刚才我们对 dp 数组的定义，现在想求 `dp[5]` 的值，也就是想求以 `nums[5] 为结尾`的最长递增子序列。

`nums[5] = 3`，既然是递增子序列，我们只要找到前面那些结尾比 3 小的子序列，然后把 3 接到最后，就可以形成一个新的递增子序列，而且这个新的子序列长度加一。

当然，可能形成很多种新的子序列，但是我们只要最长的，把最长子序列的长度作为 dp[5] 的值即可。

![](/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/最长递增子序列长度1.gif)

$$ dp[5] =  max \begin{cases}
dp[0] + 1, \quad nums[0] = 1 < nums[5]\\
dp[4] + 1, \quad nums[4] = 2 < nums[5]
\end{cases} $$

最终状态转移方程为：

$$dp[n] =  max\{
dp[i] + 1, \quad i\in [0,n-1]  \quad and \quad nums[i]  < nums[n]
\}$$

base case为1，dp 数组应该全部初始化为 1，因为子序列最少也要包含自己，所以长度最小为 1。

```java
import java.util.*;

class Solution {
    public int findNumberOfLIS(int[] nums) {
      	int[] dp = new int[nums.length];
      	Arrays.fill(dp, 1);
      	int res = 0;
      	for(int i = 0; i < nums.length; i++){
          	for(int j = 0; j < i; j++){
              	if(nums[j] < nums[i]){
                  	dp[i] = Math.max(dp[i], dp[j]+1);
                }
            }
          	res = Math.max(res, dp[i]);
        }
      	return res;
    }
}
```

> 时间复杂度：O(n^2)
>
> 空间复杂度：O(n)

**方法二：** 二分查找思想

至于二分查找的思想可以看这篇文章[动态规划设计之最长递增子序列](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484498&idx=1&sn=df58ef249c457dd50ea632f7c2e6e761&chksm=9bd7fa5aaca0734c29bcf7979146359f63f521e3060c2acbf57a4992c887aeebe2a9e4bd8a89&scene=21#wechat_redirect)

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
      	int[] top = new int[nums.length];
      	int piles = 0;
      	for(int i = 0; i < nums.length; i++){
          	int poker = nums[i];
          	int left = 0;
          	int right = piles;
          	while(left < right){
              	int mid = left + (right - left)/2;
              	if(top[mid] > poker){
                  	right = mid;
                }else if(top[mid] < poker){
                  	left = mid + 1;
                }else{
                  	right = mid;
                }
            }
          
          	if(left == piles) piles++;
          	top[left] = poker;
        }
      	return piles;
    }
}
```

> 时间复杂度：O(nlgn)
>
> 空间复杂度：O(n)



## L673.最长递增子序列的个数

[673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

> 给定一个`未排序`的整数数组，找到最长递增子序列的`个数`。

**示例 1:**

```
输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
```

**示例 2:**

```
输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。
```

**注意:** 给定的数组长度不超过 2000 并且结果一定是32位有符号整数。

「最长递增子序列」那道题谁让求最长子序列的长度，这道题是求个数。

**思路**：

首先要定义 dp 数组的含义：

`len[i]` 表示以 `nums[i]` 这个数`结尾`的最长递增子序列的长度

`cnt[i]` 表示以 `nums[i]` 这个数`结尾`的，且最长递增子序列为`len[i]`的组合数



| index |  0   |  1   |  2   |  3   |  4   |  5   |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: |
| nums  | `1`  | `3`  | `5`  | `4`  | `7`  | `6`  |
|  len  |  1   |  2   |  3   |  3   |  4   |  4   |
|  cnt  |  1   |  1   |  1   |  1   |  2   |  2   |

以`nums[i]`为结尾，那么肯定要找前面比`nums[i]`小的。

`cnt[i]` = 所有的前面比`nums[i]`小的，且等于`len[i]-1`的`len[j]`的个数

比如上面的例子：

求`cnt[3]`

+ `len[3]-1` 等于 2
+ 然后找索引3之前的比`nums[3]=4`小且`dp[j]`为2的`nums[j]`
+ 最终找到一个，即`len[1]=2`

+ 故`cnt[3]=1`

简而言之就是：`cnt[i]` 等于 `len[j] `的个数，只不过这个`len[j]`要满足三个条件：

1. `i < j`
2. `nums[j] < nums[i]`
3. `len[j] == len[i] -1`

最后返回的结果：

+ 先求得`len[]`的最大值MAX
+ 返回所有`len[]`中等于MAX的索引所对应的`cnt`的和

最终代码如下：

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int LEN = nums.length;
        int[] len = new int[LEN];
        int[] cnt = new int[LEN];
        Arrays.fill(len, 1);
        Arrays.fill(cnt, 1);
    
        for(int i = 1; i < LEN; i++){
            for(int j = 0; j < i; j++){
                if(nums[j] < nums[i]){
                    if(len[j] > len[i] - 1){
                        len[i] = len[j] + 1;
                        cnt[i] = cnt[j];
                    }else if(len[j] == len[i] - 1){
                        cnt[i] += cnt[j];
                    }
                }
                
            }
        }
        // 只需遍历一遍就可找出dp数组中最大值的个数
        int MAX = 0; // 保存当前最大值
        int res = 0;  // 保存当前最大值的个数
        for(int i = 0; i < LEN; i++){
            MAX = Math.max(MAX, len[i]);
        }
        for(int i = 0; i < LEN; i++){
            if(len[i] == MAX){
                res += cnt[i];
            }
        }
        return res;
        
    }
}
```

> 时间复杂度：O(N^2)
> 空间复杂度：O(N)



## L674.最长连续递增序列

[674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

> 给定一个`未经排序`的整数数组，找到最长且`连续`的递增序列，并返回该序列的`长度`。

**示例 1:**

```
输入: [1,3,5,4,7]
输出: 3
解释: 最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为5和7在原数组里被4隔开。 
```

**示例 2:**

```
输入: [2,2,2,2,2]
输出: 1
解释: 最长连续递增序列是 [2], 长度为1。
```

**注意：**数组长度不会超过10000。

思路：连续递增，那么只考虑`nums[i]`和其前面的`nums[i-1]`即可

解法一：

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int LEN = nums.length;
        if(LEN == 0 || nums == null) return 0;
        int[] dp = new int[LEN];
      	Arrays.fill(dp, 1);
        int res = 0;
      	for(int i = 1; i < LEN; i++){	
            if(nums[i-1] < nums[i]){
                dp[i] = dp[i-1]+1;
            }
            res = Math.max(res, dp[i]);
        }
      	return (res > 1)? res:1;
    }
}
```

> 时间复杂度：O(n)
>
> 空间压缩：O(n)

解法二：滑动窗口（状态压缩），只需要保存前面的状态即可，而不需要保存所有状态

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int LEN = nums.length;
        if(LEN == 0 || nums == null) return 0;
      
        int pre = 1;
      	int cur = 1;
        int res = 0;
      	for(int i = 1; i < LEN; i++){	
          	pre = cur;
            if(nums[i-1] < nums[i]){
                cur = pre + 1;
            }else{
              	cur = 1;
            }
            res = Math.max(res, cur);
        }
      	
      	return (res > 1)? res:1;
    }
}
```

>时间复杂度：O(n)
>
>空间压缩：O(1)



## L516.最长回文子序列

[516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

> 给定一个字符串 `s` ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 `s` 的最大长度为 `1000` 。
>
> **注意**：子序列是不要求连续的

**示例 1：**

```
输入"bbbab"
输出 4
一个可能的最长回文子序列为 "bbbb"。
```

**示例 2：**

```
输入"cbbd"
输出 2
一个可能的最长回文子序列为 "bb"。
```



这里对 `dp` 数组的定义是：在子串 `s[i..j]` 中，最长回文子序列的长度为 `dp[i][j]`。

具体来说，如果我们想求 `dp[i][j]`，假设知道子问题 `dp[i+1][j-1]` 的结果（`s[i+1..j-1]` 中最长回文子序列的长度），你是否能想办法算出 `dp[i][j]` 的值（`s[i..j]` 中，最长回文子序列的长度）呢？

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/长回子1.png" style="zoom:20%;" />

可以！这取决于 `s[i]` 和 `s[j]` 的字符：

如果它俩相等，那么它俩加上 `s[i+1..j-1]` 中的最长回文子序列就是 `s[i..j]` 的最长回文子序列：

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/长回子2.png" style="zoom:23%;" />

如果它俩不相等，说明它俩不可能同时出现在 `s[i..j]` 的最长回文子序列中，那么把它俩分别加入 `s[i+1..j-1] `中，看看哪个子串产生的回文子序列更长即可：

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/长回子3.png" style="zoom:23%;" />

以上两种情况写成代码就是这样：

```java
if(s.charAt(i) == s.charAt(j)){
  	dp[i][j] = dp[i+1][j-1] + 2;
}else{
  	dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
}
```

至此，状态转移方程就写出来了，根据 dp 数组的定义，我们要求的就是 `dp[0][n - 1]`，也就是整个 `s` 的最长回文子序列的长度。

首先明确一下 base case，如果只有一个字符，显然最长回文子序列长度是 1，也就是 `dp[i][j] = 1 (i == j)`。

因为 `i` 肯定小于等于` j`，所以对于那些` i > j `的位置，根本不存在什么子序列，应该初始化为 0。

另外，看看刚才写的状态转移方程，想求 `dp[i][j]` 需要知道` dp[i+1][j-1]`，`dp[i+1][j]`，`dp[i][j-1] `这三个位置；

再看看我们确定的 base case，填入 dp 数组之后是这样：

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/长回子4.png" style="zoom:30%;" />

为了保证每次计算 `dp[i][j]`，左下右方向的位置已经被计算出来，只能斜着遍历或者反着遍历：

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/长回子5.png" style="zoom:30%;" />

这里选择反着遍历（第二种方式），代码如下：

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        for(int i = n-1; i >= 0; i--){
            dp[i][i] = 1;
            for(int j = i+1; j < n; j++ ){
                if(s.charAt(i) == s.charAt(j)){
                    dp[i][j] = dp[i+1][j-1] + 2;
                }else{
                    dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
                }
            }
        }
        return dp[0][n-1];
    }
}
```

> 时间复杂度：O(n^2)
>
> 空间复杂度：O(n^2)



## L53.最大子序和

[53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

[labuladong](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/zui-da-zi-shu-zu)

> 给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

这道题不能用滑动窗口来做，因为数组中含有复数，这样就无法决定窗口的大小。

[和为S的连续正数序列](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=13&&tqId=11194&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking) 中便用到了滑动窗口

思路可看官方题解，讲的很清楚，这里不再赘述...

方法一：动态规划

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int tmp = 0;  // tmp 表示以nums[i]为结尾时的最大子序和，我们的目的是求得所有的，然后找出最大的
        int res = nums[0];  // res表示到目前nums[i]为止，之前以 nums[k],0≤k≤i 为结尾的所有最大子序和的最大值。
        for(int each : nums){
            tmp = Math.max(tmp + each, each);
            res = Math.max(res, tmp);
        }
        return res;
    }
}
```

> 时间复杂度：O(n)
>
> 空间复杂度：O(1)

方法二：分治法

```java
class Solution {
    public class Status {
        public int lSum, rSum, mSum, iSum;

        public Status(int lSum, int rSum, int mSum, int iSum) {
            this.lSum = lSum;
            this.rSum = rSum;
            this.mSum = mSum;
            this.iSum = iSum;
        }
    }

    public int maxSubArray(int[] nums) {
        return getInfo(nums, 0, nums.length - 1).mSum;
    }

    public Status getInfo(int[] a, int l, int r) {
        if (l == r) {
            return new Status(a[l], a[l], a[l], a[l]);
        }
        int m = (l + r) >> 1;
        Status lSub = getInfo(a, l, m);
        Status rSub = getInfo(a, m + 1, r);
        return pushUp(lSub, rSub);
    }

    public Status pushUp(Status l, Status r) {
        int iSum = l.iSum + r.iSum;
        int lSum = Math.max(l.lSum, l.iSum + r.lSum);
        int rSum = Math.max(r.rSum, r.iSum + l.rSum);
        int mSum = Math.max(Math.max(l.mSum, r.mSum), l.rSum + r.lSum);
        return new Status(lSum, rSum, mSum, iSum);
    }
}
```

> 时间复杂度：时间复杂度为 O(n)
> 空间复杂度：递归会使用 O(logn) 的栈空间，故渐进空间复杂度为 O(logn)

「方法二」相较于「方法一」来说，时间复杂度相同，但是因为使用了递归，并且维护了四个信息的结构体，运行的时间略长，空间复杂度也不如方法一优秀，而且难以理解。那么这种方法存在的意义是什么呢？

对于这道题而言，确实是如此的。但是仔细观察「方法二」，它不仅可以解决区间 `[0, n - 1][0,n−1]`，还可以用于解决任意的子区间 `[l, r][l,r]` 的问题。如果我们把 `[0, n - 1][0,n−1]` 分治下去出现的所有子区间的信息都用堆式存储的方式记忆化下来，即建成一颗真正的树之后，我们就可以在 O(logn) 的时间内求到任意区间内的答案，我们甚至可以修改序列中的值，做一些简单的维护，之后仍然可以在 O(logn) 的时间内求到任意区间内的答案，对于大规模查询的情况下，这种方法的优势便体现了出来。这棵树就是上文提及的一种神奇的数据结构——线段树。

## L1143.最长公共子序列 ？

[1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

>给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。
>
>一个字符串的子序列是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
>例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
>
>若这两个字符串没有公共子序列，则返回 0。

[labuladong](https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/zui-chang-gong-gong-zi-xu-lie)

最长公共子序列（Longest Comm on Subsequence，简称` LCS`）是一道非常经典的面试题目，因为它的解法是典型的`二维动态规划`，大部分比较困难的字符串问题都和这个问题一个套路，比如说`编辑距离`。而且，这个算法稍加改造就可以用于解决其他问题，所以说 LCS 算法是值得掌握的。

为啥这个问题就是动态规划来解决呢？因为子序列类型的问题，穷举出所有可能的结果都不容易，而动态规划算法做的就是穷举 + 剪枝，它俩天生一对儿。所以可以说只要涉及子序列问题，十有八九都需要动态规划来解决，往这方面考虑就对了。



```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
				int m = text1.length(), n = text2.length();
      	int[][] dp = new int[m+1][n+1];

      	for(int i = 1; i <= m; i++){
          	for(int j = 1; j <= n; j++){
              	if(text1.charAt(i-1) == text2.charAt(j-1)){
                  	dp[i][j] = 1 + dp[i-1][j-1];
                }else {
                  	dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
      	return dp[m][n];
    }
}
```

转化为字符数组节省时间

```java
class Solution {
    public int  longestCommonSubsequence(String text1, String text2) {
        char[] t1 = text1.toCharArray();
        char[] t2 = text2.toCharArray();
        int m = t1.length;
        int n = t2.length;
        int[][] dp = new int[m+1][n+1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (t1[i-1] == t2[j-1]){
                    // 这边找到一个 lcs 的元素，继续往前找
                    dp[i][j] = 1+ dp[i-1][j-1];
                }else {
                    //谁能让 lcs 最长，就听谁的
                    dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[m][n];
    }
}
```



## L72.编辑距离

[72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

> 给你两个单词 *word1* 和 *word2*，请你计算出将 *word1* 转换成 *word2* 所使用的最少操作数 。
>
> 你可以对一个单词进行如下三种操作：
>
> 1. 插入一个字符
> 2. 删除一个字符
> 3. 替换一个字符

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```



编辑距离算法被数据科学家广泛应用，是用作`机器翻译`和`语音识别`评价标准的基本算法。

最直观的方法是暴力检查所有可能的编辑方法，取最短的一个。所有可能的编辑方法达到指数级，但我们不需要进行这么多计算，因为我们只需要找到距离最短的序列而不是所有可能的序列。

我们可以对任意一个单词进行三种操作：

插入一个字符；

删除一个字符；

替换一个字符。

题目给定了两个单词，设为 A 和 B，这样我们就能够六种操作方法。

但我们可以发现，如果我们有单词 A 和单词 B：

+ 对单词 A 删除一个字符和对单词 B 插入一个字符是等价的。例如当单词 A 为 doge，单词 B 为 dog 时，我们既可以删除单词 A 的最后一个字符 e，得到相同的 dog，也可以在单词 B 末尾添加一个字符 e，得到相同的 doge；

+ 同理，对单词 B 删除一个字符和对单词 A 插入一个字符也是等价的；

+ 对单词 A 替换一个字符和对单词 B 替换一个字符是等价的。例如当单词 A 为 bat，单词 B 为 cat 时，我们修改单词 A 的第一个字母 b -> c，和修改单词 B 的第一个字母 c -> b 是等价的。

这样一来，我们可以保留一个字符串不变，只是操作另一个字符串。比如保留B，只操作A：

+ 在单词 A 中`插入`一个字符

+ `删除` A 中的一个字符
+ `修改` A 中的一个字符

如下图，这里给出了最短编辑距离的其中一种编辑方式：

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/最短编辑距离.png" style="zoom:25%;" />

上图最短编辑距离为5。

可以发现操作不只有三个，其实还有第四个操作，就是什么都不要做（skip）。比如当 $s1[i] == s2[j] $ 时，执行 $i--$ 即 $j--$

了解上述过程之后，下面就直接用动态规划的思路解题，定义动态规划数组的含义：

`dp[i][j]` 表示`s1[0...i]` 和 `s2[0...j]` 的最小编辑距离。

`base case` 就是 `dp[..][0]` 和 `dp[0][..]`，表示当其中一个字符串0时，最小编辑距离肯定取决于不为0的那个字符串的长度，比如 A 为0，B 不为0，那么最短编辑距离有两种选择：

+ 要么将 B 中的字符全部删除变为0
+ 要么向 A 中逐个添加字符使其跟 B 相同

当 $s1[i] != s2[j] $，有三种选择：

+ 在单词 A 中`插入`一个字符

+ `删除` A 中的一个字符
+ `修改` A 中的一个字符

只不过这三个选择哪一个，取决于选择哪一个使得`dp[i][j]`，所以选择三个中的最小值

<img src="/Users/superfarr/Desktop/JavaNote/算法与数据结构/image/最短编辑距离1.png" style="zoom:25%;" />

**Java实现如下**：

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m+1][n+1];

        // base case
        for(int i = 0; i <= m; i++) dp[i][0] = i;
        for(int i = 0; i <= n; i++) dp[0][i] = i;
        
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(word1.charAt(i-1) == word2.charAt(j-1)){
                    dp[i][j] = dp[i-1][j-1];
                }else{
                    dp[i][j] = min(
                        dp[i][j-1] + 1,
                        dp[i-1][j] + 1,
                        dp[i-1][j-1] + 1
                    );
                }
            }
        }
        return dp[m][n];
    }

    public static int min(int a, int b, int c){
        return Math.min(a, Math.min(b, c));
    }
}
```

>时间复杂度：$O(n^2)$
>
>空间复杂度：$O(n^2)$



## 高楼扔杯子？

[labuladong](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484675&idx=1&sn=4a4ac1c0f1279530b42fedacc6cca6e6&chksm=9bd7fb0baca0721dda1eaa1d00b9a520672dc9d5c3be762eeca869be35d7ce232922ba8e928b&scene=21#wechat_redirect)

>你面前有一栋从 1 到`N`共`N`层的楼，给你`K`个杯子（`K`至少为 1）。现在确定这栋楼存在楼层`0 <= F <= N`，在这层楼将杯子扔下去，杯子`恰好`没摔坏（高于`F`的楼层都会坏，低于`F`的楼层都不会坏）。
>
>现在问，`最坏`情况下，`至少`要扔几`次`杯子，才能`确定`这个楼层`F`呢？
>
>也就是让你找摔不坏杯子的最高楼层`F`

PS：

+ F 可以为 0，比如说杯子在 1 层都能摔坏，那么 F = 0

+ 杯子没坏，还可以拿来继续试验

> 题外话：为什么题目问的是`最坏情况下至少`？

这就相当于找这两个因素下的极值。

打个比方，一名NBA篮球运动员的投射命中率受`投篮距离`和`自身状态`两个因素影响：

+ 自身状态一定的情况下，投篮距离越长命中率越低
+ 投篮距离一定的情况下，自身状态越差命中率越低

这里给两个因素都规定一个区间，投篮距离为`[篮下, 三分线]`，自身状态为`[30, 100]`（状态为0不太现实）。那么题目中的`最坏情况下至少`在这里指的就是，当该球员状态最差（为30）时站在三分线投篮的命中率，假如这个命中率为x%，那么求得了这个命中率之后，是不是可以断定该球员的底线，就是当环境最恶劣（三分线）的情况下自身状态又最差（为30）的命中率为x%，那么该球员不管站在哪个位置`[篮下, 三分线]`且自身的状态`[30, 100]`不管怎样，他的命中率肯定是大于等于x%的。

回到该问题，掌握了最坏情况下至少需要x个杯子，那么是不是不管遇到什么情况，只要给我大于等于x个杯子，就一定能找到这个F。

> `最坏情况`和`至少`分别是啥意思？

F不坏，F+1坏了，也就是说我们肯定要找到这两个相邻的值（F 和 F+1）才能确定F，也就是找到这个临界点，

所谓最坏情况，就是坏到不能再坏，一共有N层楼，最坏情况下找到F，试了N-1次，到最后一次（第N次）才试到F这层，这就是最坏情况，也就是总共试了N次（每个楼层都试了一次），还能再坏吗？不能了吧。







## L887.鸡蛋掉落？

高楼扔杯子进阶

[887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/)

>你将获得 K 个鸡蛋，并可以使用一栋从 1 到 N  共有 N 层楼的建筑。
>
>每个蛋的功能都是一样的，如果一个蛋碎了，你就不能再把它掉下去。
>
>你知道存在楼层 F ，满足 0 <= F <= N 任何从高于 F 的楼层落下的鸡蛋都会碎，从 F 楼层或比它低的楼层落下的鸡蛋都不会破。
>
>每次移动，你可以取一个鸡蛋（如果你有完整的鸡蛋）并把它从任一楼层 X 扔下（满足 1 <= X <= N）。
>
>你的目标是确切地知道 F 的值是多少。
>
>无论 F 的初始值如何，你确定 F 的值的最小移动次数是多少？

F的最小移动次数也就是最少需要需要在不同的楼层丢几次，才能确定这个F值

函数签名：

```java
class Solution {
    public int superEggDrop(int K, int N) {

    }
}
```



无论F的初始值如何，确定F的值的最小移动次数是多少？

这么说最终的结果跟F是多少无关，只跟鸡蛋的个数K与N有关。



## 121. 买卖股票的最佳时机

[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

>给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
>
>如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
>
>注意：你不能在买入股票前卖出股票。

**方法一**：一次遍历

如果股票价格呈非单调递增，那么最终利润为0；

如果股票价格呈非单调递减，那么最终利润大于等于0

其次就是呈起伏的趋势，

我们一共需要维护的量就两个：

+ 股票的最低价格`minPrice`
+ 利润的最大值`maxProfit`

① 在股票价格`下跌`阶段，股票的最低价格`minprice`可能会更新，至于会不会更新，就看当前的值是否比`minprice`小，但是利润的最大值`maxprofit`不可能更新；

② 在股票价格`持平`阶段，`minprice`和`maxprofit`都不会更新，直接跳过；

③ 在股票价格`上涨`阶段，股票的最低价格`minprice`不可能更新，`maxprofit`是否更新要看`price[i]-minprice` > `maxprofit` ? 如果大于就更新，否则不更新。

**代码如下**：

```java
class Solution {
    public int maxProfit(int[] prices) {
      	int minPrice = Integer.MAX_VALUE;
      	int maxProfit = 0;
      	for(int i = 0; i < prices.length; i++){
          	if(prices[i] < minPrice){
              	minPrice = prices[i];
            }else if(prices[i] - minPrice > maxProfit){
              	maxProfit = prices[i] - minPrice;
            }
        }
      	return maxProfit;
    }
}
```

+ 时间复杂度：O(n)

+ 空间复杂度：O(1)



**方法二**：动态规划

`dp[][]`

















