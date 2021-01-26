## 前言

[labuladong](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247485044&idx=1&sn=e6b95782141c17abe206bfe2323a4226&chksm=9bd7f87caca0716aa5add0ddddce0bfe06f1f878aafb35113644ebf0cf0bfe51659da1c1b733&scene=21#wechat_redirect)

在刷题的过程中我们会经常用到二分查找算法，该算法听着很简单，当然实现起来也不难。

但是，我们会时常碰到一些特殊情况，根据情况不同，二分查找的代码也会有细微差别，所以真正掌握二分的代码也不是件非常容易的事。

今天我们就来扒一扒在各种场景下的代码应该如何写。

以下的讨论都是基于`问题的解是存在`的前提下。对于特殊情况，比如数组中无 target 值等，只需在该函数外考虑。

## 场景一

**场景一**：`经典场景。在一个有序的数组中寻找某个下标使得其对应的值等于某个指定的 target`

场景一是二分查找最常见的应用场景

代码如下：

```java
public static int binarySearch (int[] arr, int target){
    int left = 0, right = arr.length - 1;
    while(left <= right){
        int mid = left + (right-left)/2;  
        if(arr[mid] == target) {
          	return mid;
        } else if (arr[mid] < target) {
          	left = mid + 1;
        } else {
          	right = mid - 1;
        }
    }
    return left;
}
```

上面的这种二分场景，多用于数组中不含有重复元素(或者含有重复元素，当解有多个的时候，随机返回其中的一个下标)。

**为什么是 “l = mid + 1” 和 “r = mid - 1” 而不是 “l = mid + 1” 和 “r = mid - 1” ？**

+ 可以假设这种情况，如果我们不用 “l = mid + 1” 而用 “l = mid”，当最终左右指针相邻时，此时 mid = l，如果我们要找的值 target = arr[r]，且如果 arr[l] < arr[r]，即arr[mid] < arr[r]，执行 “l = mid”，此时结果会怎样？结果是 l 不变，r也不变，这样就进入了死循环

+ 同理，采用 "r = mid - 1" 亦是如此

**为什么 while 的终止条件是 “l <= r”，而不是 “l < r” ？**

+ 考虑到边界情况，如果问题的解出现在数组的最左边和或者最右边的情况，需要考虑 l 和 r 为问题的可能解，因此终止条件是 “l <= r”，而不是 “l < r”

## 场景二

**场景二**：`在一个有序的数组中，左边第一个大于等于目标值target的下标，即左边界问题，也是最大的最小问题。`

> 首先我们要明白，“ mid = l + (r-l)/2 ”是向下取整的，当最终左右指针相邻时 mid 总是等于 l 的

代码如下：

```java
public static int leftBound(int[] arr, int target){
    int left = 0, right = arr.length - 1;
    while(left < right){
        int mid = left + (right-left)/2;
        if(arr[mid] < target) {
          	left = mid + 1;
        } else {
          	right = mid;
        }
    }
    return left;
}
```

问题分析：

**为什么等于 mid 的时候不直接返回？**

+ 因为该种场景下，可能存在多个数组值等于target，但是，我们是要求出最左边的那个下标

 + 因此，当arr[mid] == target的时候，不是直接返回，而是接着往左寻找（丢弃右半部分），更新 r

为什么是  “r = mid” 而不是 “r = mid + 1” ？
  + 当arr[mid] == target， 此时的 mid 有可能就是最左边应该返回的答案，因此为了防止继续往左查找的时候不漏掉这个解，故将 r 更新为r = mid， 而不是mid-1

为什么是  “l = mid + 1” 而不是 “l = mid” ？
  + 由于是要寻找左边第一个大于等于指定值target所对应的下标，那么当 arr[mid] < target 时说明 mid 的左边（包括 mid 所对应的位置）是肯定不满足条件的，因此在舍弃左半部分之后，进行更新 l 的时候 l = mid + 1

**为什么 while 的终止条件是 “l < r”，而不是 “l <= r” ？**
  + 反证！
  + 如果循环结束条件是 l <= r，如前面所述，mid = l + (r-l)/2 是向下取整的
  + 因此 mid 总是往左偏，当 l 和 r 相等的时候，此时 mid 总是等于 l
  + 而当 arr[mid] == target 的时候，我们由于没有直接返回，而是令r = mid，也就是r == l
  + 故造成死循环...

## 场景三

**场景三**：`在一个有序的数组中，右边第一个小于等于目标值target的下标，即右边界问题，也是最小的最大问题。`

代码如下：

```java
public static int binarySearchLower(int[] arr, int target){
    int left = 0, right = arr.length-1;
    while(left < right){
        int mid = left + (right-left+1)/2;
        if(arr[mid] > target) {
          	right = mid - 1;
        } else {
          	left = mid;
        }
    }
    return left;
}
```

可以参考场景二进行做相同的三个问题思考

**为什么是 “mid = l + (r-l+1)/2” 而不是 “mid = l + (r-l)/2” ？**

  + 此处多加一个1，是为了当 l 和 r 相邻时让 mid 往右偏，因为该问题是求右边界的问题
  + 考虑数组[2, 2], target == 2，此时，由于要求右边界，因此，mid应该向上取整，故二分的时候分子多加了一个1





以上为了便于理解只给出假设答案在数组中的情况，现在补充完整代码：

```java

```



## 总结

> 对于二分问题的代码，只需要记住如下的关键点

`判断问题是求左边界还是求右边界，从而确定mid往哪偏。`

`依据问题是求左边界还是右边界，从而确定在二分淘汰的时候是更新谁，保留谁为mid。`

 `其实很多问题都可以转化为最优解问题，比如最大中的最小，最小中的最大，都可以转化为场景一和场景二，只是将if条件单独封装成一个函数，将问题的判断单独考虑，其他代码不变。`

相关的leetcode问题有: 
  + [分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum/)
  + [在D天内运送包裹的能力](https://leetcode-cn.com/problems/xiao-zhang-shua-ti-ji-hua/)
  + [小张刷题计划](https://leetcode-cn.com/problems/xiao-zhang-shua-ti-ji-hua/)
  + [旋转数组的最小数字](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&&tqId=11159&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)