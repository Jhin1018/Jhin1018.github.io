---
title: 刷题日记day2 
date: 2021-05-03 20:17:01
tags: LeetCode
categories: 技术
description: LeetCode 7
---

## 每日一题 LeetCode 7 翻转整数

说实话这个题目太过简单，只需要考虑溢出时的情况，不过这里有趣的是C++溢出时会报错而Java不会，因此java中只需要判断每次乘10加上个位数字后再整除10还等不等于原数的情况，如果不等那就是溢出了，溢出就返回0。

Code：

```
public int reverse(int x) {
        int res = 0;
        while (x != 0) {
            int tmp = res * 10 + x % 10;
            if (tmp / 10 != res) { // 溢出
                return 0;
            }
            res = tmp;
            x /= 10;
        }
        return res;
    }
```

