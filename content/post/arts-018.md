---
title: "2022第3篇ARTS"
date: 2022-01-23T22:43:00+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

本周算法部分是一个相对比简单的字符串比较，当时自己做的时候没考虑清楚，看完解法后其中的解题技巧还是挺不错的。英语文章部分，主要是关注了大数据的两种架构模式：Lambda和Kappa，当下由于对实时数据的展示要求，大部分大数据平台都会基于流式进行数据处理，主要还是基于Kafaka和Flink；刚好最近也在看《数据密集型应用系统设计》，才体会到这本书确实不错。实操和分享部分都是关于阿里的一个画图工具和银行核心转型的介绍，都是非常不错的，如果当前自己的环境没有实际的案例，跟踪大厂的解决方案，也是不错的一种方式。

## 1.Algorithm

[796]&nbsp;&nbsp;[Rotate String](https://leetcode.com/problems/rotate-string/)

**Easy** &nbsp;&nbsp; **String** &nbsp;&nbsp; **String Matching**

Given two strings s and goal, return true if and only if s can become goal after some number of shifts on s.
A shift on s consists of moving the leftmost character of s to the rightmost position.
For example, if s = "abcde", then it will be "bcdea" after one shift.

Example 1:

```
Input: s = "abcde", goal = "cdeab"
Output: true
```

Example 2:

```
Input: s = "abcde", goal = "abced"
Output: false
```

Constraints:

```
1 <= s.length, goal.length <= 100
s and goal consist of lowercase English letters.
```

**解答**

采用字符串比较的方式，比如将字符进行合并后进行对比

##### (1).采用将原字符进行合并，再采用内置的Contains方法进行比较

```golang
func rotateString(s string, goal string) bool {
	return len(s) == len(goal) && strings.Contains(s+s, goal)
}
```

##### (2).对方法1进行改进，遍历进行比较

```golang
func rotateString(s string, goal string) bool {
	if len(s) != len(goal) {
		return false
	}

	if s == goal {
		return true
	}

	for i := 0; i <= len(s); i++ {
		if strings.Compare(s[i:]+s[:i], goal) == 0 {
			return true
		}
	}
	return false
}
```

## 2.Review
* [Multi-Runtime Microservices Architecture](https://www.infoq.com/articles/multi-runtime-microservice-architecture/)
![Distributed application needs](/img/arts/1Multi-Runtime-Microservices-Architecture-1-1582629228248.webp)
* [Lambda architecture](https://en.wikipedia.org/wiki/Lambda_architecture)
  * Lambda architecture is a data-processing architecture designed to handle massive quantities of data by taking advantage of both batch and stream-processing methods
  * Batch layer: using a distributed processing system that can handle very large quantities of data
  * Speed layer: processes data streams in real time and without the requirements of fix-ups or completeness
  * Serving layer: Output from the batch and speed layers are stored in the serving layer, which responds to ad-hoc queries by returning precomputed views or building views from the processed data.
![Lambda architecture](/img/arts/Diagram_of_Lambda_Architecture_(generic).png)
* [What is Kappa architecture](https://www.educative.io/edpresso/what-is-kappa-architecture)
  * Lambda vs. Kappa architecture
  * Lambda
    * Separate layers for batch and steaming
    * Higher code complexity,i.e.,maintain 2 technology stacks for batch and stream
    * Faster performance with batch and stream layer
  * Kappa
    * Unified layer for both batch and stream
    * Lower code complexity,i.e,maintain single technology stack for batch and stream layer
    * Processing large amounts of data from database would be expensive
* [Questioning the Lambda Architecture](https://www.oreilly.com/radar/questioning-the-lambda-architecture/)
* [Kappa Architecture is Mainstream Replacing Lambda](https://www.kai-waehner.de/blog/2021/09/23/real-time-kappa-architecture-mainstream-replacing-batch-lambda/)
* [Big data architectures](https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/)

## 3.Tip
* 架构流程工具的思考
  我是一个ProcessOn的重度用户者，平时画架构图，流程图都会使用这个工具，但是因为这个是收费工具，也是网络版的，自己一直想往Draw.io和亿图上转，这周打开亿图，发现他们也采用ProcessOn的会员模式，会员提供一批模板可以使用，同时会员也可以制作模板供别人购买使用。而Draw.io上的图例比较少，之前一直是基于ProcessOn上阿里云那套图例去画的，“将ProcessOn上经典的图例放到Draw.io上”，这样就可以完美的进行切换，接下来会尝试慢慢适应Draw.io的使用方式。
  [阿里云的云速搭 CADT](https://www.aliyun.com/product/developerservices/cadt)，也是一个基于阿里云服务的架构管理工具，里面提供了一些经典的架构部署案例，同时可以对云上架构方案的成本、部署、运维、回收进行全生命周期的管理。非常值得好好研究学习的一个架构案例。

## 4.Share
* [“核”聚变 核心系统转型之路](https://mp.weixin.qq.com/s/P3mOjOxxQ9C4lr3eDTFPEA)
  银行核心系统转型，当前在国家数字化转型的大背景推动下，可以加快各大银行的核心系统转型。以前都吐槽银行的技术比较落后，系统老旧，随着阿里提出的“金融级云原生”概念，对整个金融行业引入互联网的一些技术具有一定的指导意义，同时随着各大银行核心系统的升级，随之会带来核心系统周边系统的升级改造，也将促使传统集成商对自己的技术进行升级改造，才能适应新时代的要求。