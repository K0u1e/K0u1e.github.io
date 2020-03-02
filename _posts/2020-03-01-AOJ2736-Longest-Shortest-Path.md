---
layout:     post   				    # 使用的布局（不需要改）
title:      AOJ2736-Longest Shortest Path   # 标题 
date:       2020-03-01 				# 时间
author:    K0u1e					# 作者
header-img: img/post-bg-desk.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 费用流
    - 线性规划
---

## 题意
[传送门](https://onlinejudge.u-aizu.ac.jp/challenges/sources/JAG/Regional/2736?year=2015)

给定一张$n$个点$m$条边的带权的有向图，边$i$的权值为$d_i$，你可以以$c_ix$的代价使边$i$的权值增加$x$，但是总代价不得超过$P$，问增加边权后从点$s$到点$t$的最短路的最大值为多少。

## 分析

首先可以根据原问题列出如下线性规划问题：

$$
\max \quad p_t-p_s\\
s.t. \quad p_v-p_u\le x_e+d_e	\\
\sum c_ex_e \le P	\\
x_e\ge0
$$

令$f_e$为第一类限制的对偶变量，$y$为第二类限制的对偶变量。

原问题可化成其对偶问题：

$$
\min \quad \sum d_ef_e+Py\\
s.t. \quad \sum_{(v,s)\in G}f_e-\sum_{(s,v)\in G} f_e \ge -1\\
\quad \sum_{(v,t)\in G}f_e-\sum_{(t,v)\in G} f_e \ge 1\\
\quad \sum_{(v,u)\in G}f_e-\sum_{(u,v)\in G} f_e \ge 0\\
\quad c_ey-f_e\ge0\\
\quad f_e\ge 0
$$

可以发现是一个类似费用流的形式。令$y=\frac{1}{m}，f_e=\frac{f_e}{m}$：

$$
\min \quad (\sum d_ef_e+P)/m\\
s.t. \quad \sum_{(v,s)\in G}f_e-\sum_{(s,v)\in G} f_e \ge -m\\
\quad \sum_{(v,t)\in G}f_e-\sum_{(t,v)\in G} f_e \ge m\\
\quad \sum_{(v,u)\in G}f_e-\sum_{(u,v)\in G} f_e \ge 0\\
\quad 0 \le f_e \le c_e\\
\quad m>0
$$

前三个限制的左边相加等于$0$，所以都只能取等号，即流量为$m$。

$$
\min \quad (\sum d_ef_e+P)/m\\
s.t. \quad 从s到t的流量为m\\
\quad 0 \le f_e \le c_e\\
\quad m>0
$$

令$g(m)=\sum d_ef_e+P$，$g(m)$为流量-费用函数，显然是一个下凸的函数。

我们的目标函数即为$(0,0)$到$g(m)$上一点$(m,g(m))$的直线斜率，最小值即为切线斜率，一定出现在端点上，每次增广完更新答案即可。
