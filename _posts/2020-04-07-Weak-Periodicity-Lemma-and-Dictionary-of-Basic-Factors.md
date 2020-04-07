---
layout:     post   				    # 使用的布局（不需要改）
title:      WPL与基本子串字典			# 标题
date:       2020-04-07 				# 时间
author:    K0u1e					# 作者
header-img: img/post-bg-desk.jpg 	# 这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 字符串周期
    - 基本子串字典
---

呐，在 cf 板刷的时候遇到了类似的题，又被 @huanggs 问起了这道题，想着之前只是草草过了一遍挑了需要的一部分看,现在也忘得差不多了，就来重新捋一捋，大部分都是摘自陈孙立的集训队论文，~~证明就慢慢咕了吧~~。

## 一些定义

**定义 1：**若串 $S$ 和整数 $x$ 满足 $1\le x \le \vert S \vert $，且对于所有的 $1 \le i \le \vert S \vert - x$，都有 $S[i]=S[i+x]$，则称 $x$ 为串 $S$ 的周期或周期长度。特别地，若 $x$ 还是 $\vert S \vert$ 的因数，则称 $x$ 是 $S$ 的整周期，$S$ 是整周期串。

**定义 2：**$per(S)$ 表示 $S$ 所有周期组成的集合，$minper(S)$ 表示 $per(S)$ 中的最小值。显然对于非空串有 $\vert S\vert \in per(S)$，$\vert per(S) \vert >0$。

**定义 3：**若串 $S$ 和整数 $x$ 满足 $0 \le x < \vert S\vert$，且满足 $pref(S,x)=suf(S,x)$，则称 $pref(S,x)$ 是 $S$ 的 border​。

**推论 1：**$\vert S \vert-x$ 是 $S$ 的周期当且仅当 $pref(S,x)$ 是 $S$ 的 border。

**推论 2：**若 $p$ 是 $S$ 的周期，且某个满足 $r-l+1\ge p$ 的子串 $S[l...r]$ 有周期 $q$，并满足 $q\mid p$，则 $q$ 也是 $S$ 的周期。

## 问题引入

给定一个串 $S$，若干次询问，每次给出 $l,r$，询问 $per(S[l...r])$。

## 一些性质

**引理 1 (Weak Periodicity Lemma)：**若$p,q\in per(S)$，且$p+q\le \vert S \vert$，则$\gcd(p,q) \in per(S)$。

**推论 3：**若 $p=minper(S) \le \frac{\vert S \vert}{2}$，则对于任意 $x\in per(S),x+p\le \vert S \vert$，有 $p \mid x$。

**引理 2：**若串 $S,T$ 满足 $2 \vert S \vert \ge \vert T \vert$，则 $S$ 在 $T$ 中的出现位置构成一个等差数列。

**引理 3：**串 $S$ 的所有不小于 $\frac{\vert S \vert}{2}$ 的 border 长度构成一个等差数列。

## 基本子串字典

一种数据结构，有 $\log_2{\vert S \vert}$ 个数组组成，如果用 $name_k$ 表示第 $k$ 个数组，那么 $name_k$ 的长度为 $\vert S \vert-2^k+1$，满足 $name_k[i] \le name_k[j]$ 当且仅当 $S[i...i+2^k-1]\le S[j...j+2^k-1]$。可以在倍增求后缀数组的同时求得。

**拓展：**维护一个有序表，记录 $name_t[i]=x$ 的所有 $i$。

## 子串周期查询

### 基本算法

**定义 5：**对于两个串 $S,T$ 满足 $\vert S \vert=\vert T \vert$，定义 $PS(S,T)=\{k \mid pref(S,k)=suf(T,k)\}$，$LargePS(S,T)=\{k \mid k\in PS(S,T) , k\ge\frac{\vert S \vert}{2} \}$。

**推论 4：**$LargePS(S,T)$ 的元素构成一个等差数列。

**推论 5：**对于一个串 $S$ 和整数 $k\le\frac{\vert S \vert}{2}$，所有满足 $k\le x < 2k$ 的 $S$ 的 border 长度 $x$ 构成等差数列。

有了这个结论我们可以 $S$ 的 border 分成 $\log \vert S\vert$ 类，第 $i$ 类为长度在 $[2^{i-1},2^i)$ 中的所有 border，可以用一个等差数列表示。于是我们有了一个用 $O(\log \vert S \vert)$ 空间表示 $per(S)$ 的方案。

而对于 $[2^{i-1},2^i)$ 这一类的 $border$，我们要求的其实就是 $Large(pref(S,2^i),suf(S,2^i))$。

**引理 4：**对于 $2^{k-1}\le x < \vert S\vert=\vert T\vert=2^k$，$x\in LargePS(S,T)$ 当且仅当 $suf(pref(S,x),2^{k-1})=suf(T,2^{k-1})$，且对应地 $pref(suf(T,x),2^{k-1})=pref(S,2^{k-1})$。

有了这个引理，我们可以先求出 $pref(S,2^{i-1})$ 在 $suf(S,2^i)$ 的所有出现位置，以及 $suf(S,2^{i-1})$ 在 $pref(S,2^i)$的所有出现位置，均用等差数列表示。为了得到 $LargePS(pref(S,2^i),suf(S,2^i))$，把这两个等差数列移位后求交即可。

求 $pref(S,2^{i-1})$ 在 $suf(S,2^i)$ 的所有出现位置，即求第一次、第二次和最后一次出现位置，显然知道这个以后就很好做了，由于这两个串都是母串的一个子串，只要实现 $succ(S,i)$ 和 $pred(S,i)$ 两个操作，分别找到 $S$ 在 $i$ 之前与之后（包括 $i$）第一个出现位置。这一步只需要对之前由基本子串字典拓展出的有序表二分即可。

### 差数列求交优化

我们用 $(s,d,k)$ 三个参数描述一个等差数列，$s,s+d,...,s+(k-1) \cdot d$。对于 $(s1,d1,k1)$ 和 $(s_2,d_2,k_2)$ 两个等差数列的公共部分，即求出所有 $s_1+xd_1=s_2+yd_2$ 的整数解并适当截取一段，一般问题只能用拓展欧几里德算法 $O(\max(d_1,d_2))$ 求解，然而本问题下可以利用一些特殊定理 $O(1)$ 求解。

**引理 5：**若四个字符串满足 $\vert x_1\vert=\vert y_1\vert\ge\vert x_2\vert=\vert y_2\vert$，且 $x_1$ 在 $y_2y_1$ 中匹配至少 $3$ 次，$y_1$ 在 $x_1x_2$ 中至少匹配 $3$ 次，则$minper(x_1)=minper(y_2)$。

若 $k_1<3$ 或 $k_2<3$，可以直接枚举对应等差数列里的所有元素并判断是否在另一个等差数列内，否则根据**引理 5**有 $d_1=d_2$。显然这部分的复杂度是 $O(1)$ 的。

### 出现位置查询优化

我们要知道的仅仅是在某个长度不超过 $2^{k}+1$ 的区间内某个长度为 $2^{k}$ 的子串的出现位置。

对于一个长度为 $2^k$ 次的串，我们将其出现位置划分成几个段，其中第 $i$ 个段存该串出现位置在 $[(i-1)\cdot 2^k+1,i\cdot 2^k]$ 

区间内的信息，显然这一部分可以用一个等差数列表示。

这部分硬做的话空间复杂度会有 $O(\vert S\vert ^2\log\vert S\vert)$，但是每位置只会出现一次，最多只会贡献给一个等差数列，所以如果只存有效的，总的空间复杂度是$O(\vert S \vert\log\vert S \vert)$，这部分可以用哈希表实现。

这样一来如果我们要查询 $S[i...i+2^k-1]$ 在 $[j,j+2^k-1]$ 的所有出现次数，只需要查询第 $j/(2^k)$ 段与第 $j/(2^k)+1$ 段即可，截取有效的一段然后合并成一个等差数列。这样我们就能做到 $O(1)$ 查询出现次数了。

结合上面两个优化，我们可以做到 $O(\vert S\vert\log \vert S\vert)$ 预处理以及 $O(\log \vert S\vert)$ 查询。
