---
layout:     post   				    # 使用的布局（不需要改）
title:      Codeforces Round 625   # 标题 
subtitle: Div. 1, based on Technocup 2020 Final Round
date:       2020-03-02 				# 时间
author:    K0u1e					# 作者
header-img: img/post-bg-desk.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - codeforces
---

## 题解

### A - Journey Planning

因为要保证$c_{i+1}-c_i=b_{c_{i+1}}-b_{c_i}$，即要满足$c_{i+1}-b_{c_{i+1}}=c_i-b_{c_i}$，那我们把所有$c-b_c$相等的$b_c$加到一起再找个最大值即可，注意$c-b_c$可能为负数。

### B - Navigation System

反向建图，$bfs$求$t$到每个点的最短路并去掉原图中所有$d_u \ne d_v+1$的边，结果即为正向建图每个点到$t$的最短路和下一步可选的最短路方案。

要让重构次数最少，必然是能不重构就不重构，只在$d_{p_i}\ne d_{p_{i+1}}$的时候重构。

要让重构次数最多，那必然是能重构就重构，除了上述情况，当$d_{p_i}=d_{p_{i+1}}+1$但可选方案数不止一个时也可重构。

### C -  World of Darkraft: Battle for Azathoth

将装备按照攻击/防御排序，怪物按照防御排序，装备的价值取反。

枚举武器，并将这把武器能破防的怪物的奖励加到那些能格挡他们攻击的防具的价值上，因为防具防御力有序，显然每只怪物都会令盾牌的一个后缀整体升值，加完以后从这么多盾牌中找到价值最大的那个再算上装备的成本用于更新答案。

每只怪物修改盾牌的后缀左端点可以二分得到。区间修改维护全局最大值可以用线段树实现。

### D -  Reachable Strings

### E -  Treeland and Viruses

### F - Blocks and Sensors
