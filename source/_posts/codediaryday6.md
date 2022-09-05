---
title: 刷题日记 day6
date: 2021-05-07 12:17:37
tags: LeetCode
categories: 技术
description: 今天刷了不少题，以后还是每天两道三题比较合适。。
---

## LeetCode 1486. XOR Operation in an Array

### 思路

今日的每日一题又是简单题，重拳出击！模拟一遍思路即可过，至于题解里面复杂的数学知识我选择无视。

### Code

```java
    public int xorOperation(int n, int start) {
        int[] nums =new int[n];
        for(int i=0;i<n;i++){
            nums[i] =start +2*i;
        }
        for(int i=1;i<n;i++){
            start = start ^ nums[i];
        }
        return start;
    }
```

## LeetCode 3 Longest Substring Without Repeating Characters

### 思路

不是第一次做了，去年为了学cpp刷题的时候就做过，思路还是和去年一样，使用滑动窗口法。用两个指针指向字符串中的位置，右边的指针不断增大，当右指针指向的字符已经在子串中出现过时，就将左指针不断向右移动，直到两个指针中间的子串无重复字母为止。

### Code

```java
    public int lengthOfLongestSubstring(String s) {
        if(s.length()==0 || s==null) return 0;
        if(s.length()==1) return 1;
        int len = s.length();
        Map<Character,Integer> map =new HashMap<Character,Integer>();
        int maxLength = 0;
        int left = 0;
        for(int i=0;i<len;i++){
            if(map.containsKey(s.charAt(i))){//字符已经在子串中出现过
                int c = map.get(s.charAt(i));
                left = Math.max(left,c+1);
            }
            map.put(s.charAt(i),i);
            maxLength = Math.max(maxLength,i-left +1);
        }
        return maxLength;
    }
```

- 时间复杂度：O(n) 遍历一遍字符串
- 空间复杂度：O(n) 需要将字符串中的每个字符都存到map中

## LeetCode 1 Two Sum

### 思路

去年也做过了，思路至今印象深刻，使用哈希表，key为nums[i]，value为下标，一边遍历一边查找target-nums[i]，遍历不到一遍数组就能解答。

### Code

```java
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<Integer,Integer>();
        for(int i=0;i<nums.length;i++){
            if (map.containsKey(target-nums[i])){
                return new int[]{map.get(target-nums[i]),i};
            }
            map.put(nums[i],i);
        }
        return new int[0];
    }
```

- 时间复杂度：O(N)，其中 N 是数组中的元素数量。对于每一个元素 x，我们可以 O(1) 地寻找 target - x。

- 空间复杂度：O(N)，其中 N是数组中的元素数量。主要为哈希表的开销。

## LeetCode 15  3Sum

### 思路

从两数之和到三数之和，因为要求找出所有的不重复的三个数的和为0的集合，所以难度陡然上升。我一开始的思路是这样的，先排序，然后固定第一个数，接下来就是两数之和了。但是这个思路的问题在于两数之和有且仅有一个解答，而这道题里不止一个解，因此需要优化。受到前面滑动窗口思路的影响，我在固定第一个数以后，对于剩下的数组用两个指针，一个从前往后，另一个从后往前遍历，这样可以找出所有的解了。

### Code

```java
public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null|| nums.length<=2) return res;

        int len = nums.length;
        Arrays.sort(nums);

        for(int first =0;first<len-2;first++){
            if(nums[first]>0) break;
            if (first>0 && nums[first]==nums[first-1]) continue;//保证第一个数字不重复
            int target = -nums[first];
            int second = first + 1;
            int third = len- 1;
            while(second < third){
                if(nums[second] + nums[third] == target){
                    List<Integer> list = new ArrayList<Integer>();
                    list.add(nums[first]);
                    list.add(nums[second]);
                    list.add(nums[third]);
                    res.add(list);

                    second++;
                    third--;
                    while(second<third && nums[second]==nums[second-1]) second++;//如果有重复就继续往后
                    while(second<third && nums[third]==nums[third+1]) third--;//有重复则继续往前
                }
                else if(nums[second] + nums[third] < target){
                    second++;
                }
                else{
                    third--;
                }
            }
        }
        return res;
    }
```

## LeetCode 53 Maximum Subarray

剑指Offer42，上个月做笔试的时候遇到3次，一次是原题，两次是变种，今天来整理一下思路增强应对变形题的能力。

### 思路

记得第一次在笔试中遇到这个题目，我的思路十分简单就是暴力搜索，我按照某个节点开头为标杆搜索了所有可能的子序列。然后今天我在看题解的时候，看到了这一篇感觉对我十分有启发，[详细解读动态规划的实现](https://leetcode-cn.com/problems/maximum-subarray/solution/xiang-xi-jie-du-dong-tai-gui-hua-de-shi-xian-yi-li/)。这篇题解中归纳了三种遍历子序列的方式，我在下面复述一下，例子是[a,b,c,d,e]。

- 以某个节点为开头的子序列，[a],[a,b],[a,b,c]....然后是[b],[b,c],[b,c,d]....
- 根据长度遍历子序列，先遍历长度为1的子序列，再遍历长度为2的...
- **以子序列的结束节点为基准遍历**，例如以a为结束节点的子序列有[a],以b为结束节点的子序列有[b],[a,b]。

对于方法三，我们不难想到{以某个节点为结束的子序列集合}={以上一节点为结束的子序列加上该节点} +{该节点本身},这就形成了一种递推的关系，这样就有了使用动态规划来解决的思路基础。

于是回到这道题中，这道题动态规划的状态转移方程也就可以用这个思路得出，设f(i)是第i个节点的最大子序列和，那么f(i+1)=Max{f(i)+nums[i+1],nums[i+1]}。考察一下起始条件，f(0) =nums[0]

### Code

```java
public int maxSubArray(int[] nums) {
    if(nums ==null || nums.length==0) return 0;
    if(nums.length ==1) return nums[0];
    int len = nums.length;
    int[] f= new int[len];
    f[0]=nums[0];
    int max = f[0];
    for(int i=1;i<len;i++){
    f[i] =Math.max(f[i-1]+nums[i],nums[i]);
    max = Math.max(max,f[i]);
    }
    return max;
}
```

- 时间复杂度:O(n)只遍历了一遍数组
- 空间复杂度:O(n) 储存了一个长度为n的f(n)

空间复杂度还可以优化，因为每个f(i)显然只和f(i-1)有关，因此我们可以考虑滚动数组的方式来优化。

```java
public int maxSubArray(int[] nums) {
    if(nums ==null || nums.length==0) return 0;
    if(nums.length ==1) return nums[0];
    int pre =nums[0];
    int max = pre;
    for(int i=1;i<nums.length;i++){
    int cur =Math.max(pre+nums[i],nums[i]);
    max = Math.max(max,cur);
    pre =cur;
    }
    return max;
}
```

- 空间复杂度:O(1)

## LeetCode 121 Best Time to Buy and Sell Stock

### 思路

这题本质上就是上一题，我们求一个两天间价格差值的数组，然后把他丢给上一题解决，如果求出来最大子序列和小于0就返回0，于是就有了下面解法一的做法。但是解法一空间太浪费了，函数调用也造成时间的浪费。于是我们考虑更简单的解法，我们需要维护一个当前时间节点历史最小价格，然后收益即为当前价格-当前历史最小值，这个收益就类似于上一题中f(i),因此是可以用滚动数组优化的，于是解法二就出来了。

### Code

#### 解法一

```java
public int maxProfit(int[] prices) {
    if(prices.length ==1) return 0;
    int len = prices.length;
    int[] d=new int[len-1];
    for(int i=1;i<len;i++){
    d[i-1] = prices[i]-prices[i-1];
    }
    int res = maxSubArray(d);
    return res>0?res:0;
}

public int maxSubArray(int[] nums) {
    if(nums ==null || nums.length==0) return 0;
    if(nums.length ==1) return nums[0];
    int pre =nums[0];
    int max = pre;
    for(int i=1;i<nums.length;i++){
    int cur =Math.max(pre+nums[i],nums[i]);
    max = Math.max(max,cur);
    pre =cur;
    }
    return max;
}
```

#### 解法二

```JAVA
public int maxProfit(int[] prices) {
    if(prices.length ==1) return 0;
    int len = prices.length;
    int min =prices[0];
    int max =0;
    for(int i=1;i<len;i++){
    min =Math.min(min,prices[i]);
    max =Math.max(max,prices[i]-min);
    }
    return max;
}

```

- 时间复杂度O(n)
- 空间复杂度O(1)

