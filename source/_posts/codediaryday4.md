---
title: 刷题日记 day4
date: 2021-05-05 11:18:00
tags: LeetCode
categories: 技术
---

## LeetCode 198 House Robber

### 思路

比较典型的动态规划题目，主要思路如下。对于第K间屋子，如果不偷它，收益为前K-1间屋子的收益；如果偷它，收益为前K-2间屋子的收益+第K间屋子的收益。于是乎动态转移方程就得到了:

- dp[i] =max(dp[i-1],dp[i-2]+nums[i])

同时考虑一下初始条件，如果只有一件屋子，那就是只有这一件屋子的收益；如果有两间屋子，那就是选择这两间屋子中收益大的那个。

### Code

```java
    public int rob(int[] nums) {
        if(nums.length==0 || nums ==null){
            return 0;
        }
        int l = nums.length;
        if(l==1) return nums[0];
        int[] dp = new int[l];
        dp[0] =nums[0];
        dp[1] =Math.max(nums[0],nums[1]);
        for(int i=2;i<l;i++){
            dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[l-1];
    }
```

- 时间负责度：O(n)
- 空间复杂度：O(n)

实际上因为对于第k间屋子来说只需要看前k-1和前k-2的收益，所以可以把dp数组优化为滚动数组。

```java
    public int rob(int[] nums) {
        if(nums.length==0 || nums ==null){
            return 0;
        }
        int l = nums.length;
        if(l==1) return nums[0];
        int first =nums[0];
        int second =Math.max(nums[0],nums[1]);
        for(int i=2;i<l;i++){
            int temp = second;
            second = Math.max(second,first+nums[i]);
            first = temp;
        }
        return second;
    }
```

- 空间复杂度：O(1)

但很神秘的是，优化以后内存消耗变多了？？？目前推测中间交换时使用新变量temp导致内存消耗变大。

## LeetCode 213 House Robber II

### 思路

上一题的升级版，加入了首尾相连的限制条件，导致第一间和最后一间房子不能同时抢了。由于这个限制条件并没有为这个问题增加新的维度，而是多了两种需要讨论的情况，一种是必须有第一间房子，另一种是必须有最后一间房子，那么思路已经很清晰了，就是做两遍DP，第一遍是从0-n-1，第二遍是从1-n，而这两遍dp的过程，单独来看就和上一题的过程相同。

### Code

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==0 || nums ==null){
            return 0;
        }
        if(nums.length==1) return nums[0];
        return Math.max(
            myrob(Arrays.copyOfRange(nums, 0, nums.length-1)),
            myrob(Arrays.copyOfRange(nums, 1, nums.length)));
    }

    public int myrob(int[] nums) {
        int first =0;
        int second =0;
        int temp =0;
        for(int i:nums){
            temp = second;
            second = Math.max(second,first+i);
            first = temp;
        }
        return second;
    }
}
```

代码里的myrob函数再次优化了上一题中的滚动数组，考虑dp[-1]和dp[-2]，那么当这两个为0时，

对于第一间和第二间房子也可以直接套用状态转移公式。

### JAVA知识

java Arrays.copyOfRange的使用方法

- original：第一个参数为要拷贝的数组对象
- from：第二个参数为拷贝的开始位置（包含）
- to：第三个参数为拷贝的结束位置（不包含）

该方法的源代码中的实现是使用了System.arraycopy数组拷贝方法，同时为了防止越界，只会拷贝from位置到源数组最后一个元素。

## LeetCode 740 Delete and Earn

### 思路

今天的每日一题，这道题可以转化成198。因为选择了x就不能选择x-1和x+1，这个条件就等同于198中的不能选择相邻两个。那么我们只要题目给的数组，转化为代表着数字x的权重数组即可。

例如官方例子nums = [2,2,3,3,3,4]

我们将其转化为sum=[0,0,4,9,4]。sum数组代表数组下标在nums中出现的次数×下标的大小，即2在nums中出现2次，于是sum[2]=2×2;3在nums中出现3次，于是sum[3]=3×3等等

然后我们就可以把sum交给198的函数了。

### Code

```java
class Solution {
    public int deleteAndEarn(int[] nums) {
        int maxVal = 0;
        for(int i:nums){
            maxVal =Math.max(maxVal,i);
        }
        int[] sum = new int[maxVal+1];
        for(int i :nums){
            sum[i] +=i;
        }
        return rob(sum);
    }

    public int rob(int[] nums) {
        int first =0;
        int second =0;
        int temp =0;
        for(int i:nums){
            temp = second;
            second = Math.max(second,first+i);
            first = temp;
        }
        return second;
    }
}
```

