---
title: "2021第3篇ARTS"
date: 2021-12-31T10:30:20+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

本周对ARTS的每项分类加了个小标题，优化每个分类的关键点，持续优化改进。算法部分是最大连续子序列，基于Kadane's algorithm是比较容易实现的，但是该算法的由来，推导还是没做的。Review部分，对Fluentd的学习，算是对主流日志方案的进一步完善，知识点方面是对Markdown的目录设置的学习，分享部分是多郭东白老师的架构课的学习，有一些架构理念，思想值得自己在工作中不断去尝试，精进。

## 1.Algorithm - Kadane's algorithm
[1749]&nbsp;&nbsp;[Maximum Absolute Sum of Any Subarray](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/)

**Medium** &nbsp;&nbsp; **array** &nbsp;&nbsp; **Dynamic Programming**

You are given an integer array nums. The absolute sum of a subarray [numsl, numsl+1, ..., numsr-1, numsr] is abs(numsl + numsl+1 + ... + numsr-1 + numsr).

Return the maximum absolute sum of any (possibly empty) subarray of nums.

Note that abs(x) is defined as follows:

If x is a negative integer, then abs(x) = -x.
If x is a non-negative integer, then abs(x) = x.

Example1:

```c
Input: nums = [1,-3,2,3,-4]
Output: 5
Explanation: The subarray [2,3] has absolute sum = abs(2+3) = abs(5) = 5.
```

Example2:

```c
Input: nums = [2,-5,1,-4,3,-2]
Output: 8
Explanation: The subarray [-5,1,-4] has absolute sum = abs(-5+1-4) = abs(-8) = 8.
```

Constraints:
```
1 <= nums.length <= 105
-104 <= nums[i] <= 104
```

**解答**
最大连续子序列，非常经典的问题，是[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)变形题目，主要采用暴力破解和基于Kadane's algorithm的解法
wikipedia上关于该类型提的说明：[Maximum subarray problem](https://en.wikipedia.org/wiki/Maximum_subarray_problem)
##### (1).基于Kadane's algorithm, 1 层循环，时间复杂度 O(n)

但是算法的执行时间不一定比第一种高，因为在for循环中多了很多次的abs方法执行

```
func solution1(nums []int) int {
	bestMaxSum, curMaxSum := 0, 0
	for _, val := range nums {
		curMaxSum = maxVal(curMaxSum+val, val)
		bestMaxSum = maxVal(bestMaxSum, curMaxSum)
	}

	bestMinSum, curMinSum := 0, 0
	for _, val := range nums {
		curMinSum = minVal(curMinSum+val, val)
		bestMinSum = minVal(bestMinSum, curMinSum)
	}

	if bestMinSum < 0 {
		if abs(bestMinSum) > bestMaxSum {
			return abs(bestMinSum)
		}
	}
	return bestMaxSum
}

func abs(val int) int {
	if val < 0 {
		return -val
	}
	return val
}

func maxVal(val1, val2 int) int {
	if val1 > val2 {
		return val1
	}
	return val2
}

func minVal(val1, val2 int) int {
	if val1 < val2 {
		return val1
	}
	return val2
}
```

##### (2).优化的方法，将查找最大值和最小值合并到一次for循环中

```
func solution2(nums []int) int {
	result, curMaxSum, curMinSum := 0, 0, 0
	for _, val := range nums {
		curMaxSum = maxVal(curMaxSum+val, val)
		curMinSum = minVal(curMinSum+val, val)
		result = maxVal(result, maxVal(abs(curMaxSum), abs(curMinSum)))
	}
	return result
}
```

## 2.Review - Fluentd
以下几篇有关Fluentd入门的文章，ELK是非常传统的日志收集方式，而在演进的过程中往往会引入Fluentd作为数据的采集端，演变成EFK的架构模式。（再加上轻量级的Grafana+Loki的方式，几种主流的日志方案都算齐了）
* [What is Fluentd?](https://www.fluentd.org/architecture)
* [Why Use Fluentd?](https://www.fluentd.org/why)
* [Unified Logging Layer: Turning Data into Action](https://www.fluentd.org/blog/unified-logging-layer)
* [Frequently Asked Questions](https://www.fluentd.org/faqs)
* [Fluentd vs Logstash: A Comparison of Log Collectors](https://logz.io/blog/fluentd-Logstash/)
* [Kubernetes Logging: Comparing Fluentd vs. Logstash](https://platform9.com/blog/kubernetes-logging-comparing-fluentd-vs-logstash/)
* [5 ELK Stack Pros and Cons](https://www.chaossearch.io/blog/elk-stack-pros-and-cons)

![Before Fluentd](../../image/arts-015-fluentd-before.png)

![After Fluentd](../../image/arts-015-fluentd-architecture.png)

摘录几个比较重要的点：
* Fluente is an open source data collector, which let you unify the data collection and comsumption for a better use and understanding of data.
* the key features:
  * Unified Logging with JSON
  * Pluggable Architecture
  * Minimum Resources Required
  * Built-in Reliability

## 3.Tip - Markdown生成目录
Markdown生成目录的技巧，之前一直会Markdown写一些文档，但是自己很少关注Markdown生成文档的效果是否有目录结构。
### 方式1. 自动生成目录
在Markdown中，自动生成目录非常简单，只需要在恰当的位置上添加[TOC]符合（包括两个大括号），凡是以 **#** 定义的标题都会编排到目录中
### 方式2. 手动书写目录
将[列表]和[页内超级链接]相结合
- [标题一](#一级标题)
  - [标题1-1](#二级标题 1-1)

参考资料：
* [Markdown 生成目录](http://www.imooc.com/wiki/markdownlesson/markdowntoc.html)
* [Markdown语法整理](https://guo365.github.io/study/Markdown.html#41)

## 4.Share - 郭东白老师的架构课
今天主要分享郭东白老师的架构课中的架构师的生成法则三：架构师永远需要在有限资源下最大化商业价值。架构师的可用资源，包括商业成本、研发成本、时间成本、迁移成本等等。

* 《架构师如何找到自己的商业模式？》
  * 为自己所在的企业创造出可度量的商业价值，这是我们获得有质量的长期收入的重要前提。
  * 为公司赚钱的三个路径：(1).实现一个商业模式，(2).提升一个商业模式的效率 (3).加速一个商业模式的收敛速度
  * 理解一个企业或者团队的商业模式：理解自己所处的工作环境
  * 每个人都要有自己的商业模式：为公司、部门或团队提供可量化的增量价值
  * 架构师如何创造自己的增量价值：
    * 架构师的产出不完全靠自己，而是靠撬动一群人来完成架构目标
    * 一个架构师想要创造长期的商业价值，就必须同时满足三个条件：
      * 确保最终架构方案的可行性
      * 确保参与方达成一个合理的实施路径，最终能够完成实施
      * 确保设计方案可以最大化解决方案的结构性
    * 做架构和做业务一样，都不能靠饱和攻击取胜，而是靠对阶段性精确目标的最大化投入来取得进步。
* 《架构师如何在一定时间内最大化自己的增量价值？》
  * 如何寻找扩大收入的机会？-》在小数据里看大机会，在大数据里看小机会
    * 靠数据来打磨用户体验。也就是通过数据分析找到机会点，然后通过产品和技术的改进，不断提升转化减少损失。
    * 看到了别人忽略的小痛点，或者在别人不去排查的小异常上执着探索，才最终跨越了现实的障碍。
    * 在别人容易忽视的痛点和异常点上，深度挖掘可能大规模复制的机会，不失为扩大收入的好方法。
  * 如何寻找减少成本的机会？
    * 在一个企业中，追求完美必须以成本可控为前提。

其实不管作为架构师或者个人，都要考虑自己的价值，作为企业的一员，如果你不能为公司创造价值，不能为部门，不能为团队创建价值，那你就是一个多余的人，很可能处于边缘或者即将被淘汰的角色。最近一年多在外面进行项目实施的架构，但是整个项目的定制化开发成本太高，人力成本，沟通成本等都非常巨大，而如何快速的演进，打磨成一款标准化的，可快速实施落地的产品是非常值得研究的事情，当然这其中也会有很多阻力，如何平衡各种利益的关联方，能否标准化的架构理念能推行，必将是一项艰巨的任务。