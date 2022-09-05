---
title: 刷题日记day1 —— LeetCode 554 砖墙
date: 2021-05-03 14:33:03
tags: LeetCode
categories: 技术
description: 刷题日记Day1

---

# 每日一题 LeetCode 554

## 思路一

最初想法，模拟整个砖墙，用一个二维数组储存整个墙，然后判断各个缝隙。

```java
 public int leastBricks(List<List<Integer>> wall) {
        int height = wall.size();
        int width =0;
        for(int i:wall.get(1)){
            width += i;
        }
        if(width ==1) { return height;}
        int[][] map = new int[height][width];
        for(int i=0;i<height;i++){
            List<Integer> line = wall.get(i);
            int l =0;
            int num =0;
            for(int j:line){
                int end =l+j;
                for(;l<end;l++){
                    map[i][l] =num;
                }
                num ++;
            }
        }

        int res =height;
        for(int i=0;i<width-1;i++){
            int cnt =0;
            for(int j=0;j<height;j++){
                if(map[j][i] ==map[j][i+1])cnt++;
            }
            res =Math.min(res,cnt);
        }
        return res;
    }
```

结果：内存爆了，输入为[[100000000],[100000000],[100000000]]

## 思路二

从刚才的思路里可以发现，我们用这个二维数组主要保存的信息是每一行砖块之间的缝隙，因此我们可以考虑用哈希表来记录这样一对数据<从左往右缝隙出现的距离，从上到下该距离上有缝隙的次数>,考虑到穿过的最小的砖块数=行数-缝隙出现的最大次数，所以可以得到解答。

Code：

```
public int leastBricks(List<List<Integer>> wall) {
        Map<Integer,Integer> map = new HashMap<Integer,Integer>(); 
        
        for(List<Integer> list:wall){
        	int cur = 0;
        	for(int i=0;i<list.size()-1;i++){
        		
        		cur+=list.get(i);//缝隙从左到右的距离
        		
            	if(map.containsKey(cur)){//如果该距离上出现过缝隙
            		map.put(cur,map.get(cur)+1);//给value+1
            	}else{
            		map.put(cur,1);//该距离上第一次出现缝隙
            	}
            }
        }
        
        int max = 0;
        for(Map.Entry<Integer, Integer> entry:map.entrySet()){
        	max = max>entry.getValue()?max:entry.getValue();
        }
        
        return wall.size()-max;
}
```

# 知识点总结

## HashMap中的Entry

HashMap的主干是一个Entry数组。Entry是HashMap的基本组成单元，每一个Entry包含一个key-value键值对。

