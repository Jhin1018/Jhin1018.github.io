---
title: 刷题日记 day5
date: 2021-05-06 00:20:14
tags: LeetCode
categories: 技术
description: 
---

## LeetCode 1720 Decode XORed Array

### 思路

今日每日一题，简单题重拳出击！解码异或数组，其实很简单，只需要知道若a XOR b =c，则c XOR a =b即可，既然encoded[i]=arr[i] XOR arr[i+1],那么arr[i+1] = encoded[i] XOR a[i],即arr[i] = encoded[i-1] XOR arr[i-1]

### Code

```java
 public int[] decode(int[] encoded, int first) {
        int[] res = new int[encoded.length+1];
        res[0] =first;
        for(int i=1;i<res.length;i++){
            res[i] =res[i-1]^encoded[i-1];
        }
        return res;
    }
```

