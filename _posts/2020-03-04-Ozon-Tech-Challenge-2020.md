---
layout:     post   				    # 使用的布局（不需要改）
title:      Ozon Tech Challenge 2020   # 标题 
subtitle: Div.1 + Div.2				#副标题
date:       2020-03-04 				# 时间
author:    K0u1e					# 作者
header-img: img/post-bg-desk.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - codeforces
---

## 题解

### A - Kuroni and the Gifts

因为保证了$a$ 、$b$数组本身并没有相同元素，所以排序后输出即可。

### B - Kuroni and Simple Strings

从左往右一直拿左括号从右往左一直拿右括号，显然到最后左边只剩右括号右边只剩左括号，只要这么拿一次就够了。

### C - Kuroni and Impossible Calculation

当$n>m$时必有一对$|a[i]-a[j]| \mod m=0$，此时答案为0，否则$n^2$暴力做就好了。

### D - Kuroni and the Celebration 

每次询问一对叶子，如果他们的$lca$不是两者之一，那就把这两个叶子删掉，问题规模减小，否则根节点就是这个$lca$。

### E - Kuroni and the Score Distribution

先考虑按照$1.2.3...$这样构造。发现每往后加一个数$i$，$m$最多可以增加加$\frac{(i-1)}{2}$，并且这个增加量是可以通过增大$i$来减小的，那么只需要控制某个数即可，之后的数就尽可能大且保持$2e5$的间距就好了。

最终的构造方案大概长这样

$$
(1)- (2)-(3)-(4)...-(X)-...-(1e9-2e4)-(1e9)
$$

## 小结

明明前五题都憨得一匹啊为什么我能打成这个样子啊，C题上手写了个$O(nm)$的担心T一个小时候补交了少了300多分，D题看错题没看到题目给树了空想了好久才回头重新看题。一直想着哪次能一飞冲天上一波菊粉但是好像次次都是在挽回一个崩溃的局面，不过仔细想想好像也不是啥坏事，崩着崩着好像下限也跟着提上来了。
