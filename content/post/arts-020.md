---
title: "2022第5篇ARTS"
date: 2022-02-06T13:58:00+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

本周Review部分主要了解了下HTAP数据库方面的知识，特别是TiDB；Share部分分享了一个数据架构的演进阶段，对项目架构中的数据架构部分有一定的指导意义；Algorithm部分，是随机选择的，是一道比较简单的数组相关的算法题；而题解的简练解法值得参考学习；Tip部分主要是总结最近学习几个开源项目的一点想法，对我们怎样展示的工作成果有一定的借鉴意义。

## 1.Algorithm

[717]&nbsp;&nbsp;[1-bit and 2-bit Characters](https://leetcode.com/problems/1-bit-and-2-bit-characters/)

**Easy** &nbsp;&nbsp; **array** &nbsp;&nbsp;

We have two special characters:

The first character can be represented by one bit 0.
The second character can be represented by two bits (10 or 11).
Given a binary array bits that ends with 0, return true if the last character must be a one-bit character.

Example 1:

```
Input: bits = [1,0,0]
Output: true
Explanation: The only way to decode it is two-bit character and one-bit character.
So the last character is one-bit character.
```

Example 2:

```
Input: bits = [1,1,1,0]
Output: false
Explanation: The only way to decode it is two-bit character and two-bit character.
So the last character is not one-bit character.
```

Constraints:

```
1 <= bits.length <= 1000
bits[i] is either 0 or 1.
```

**解答**
本题可以采用常规的遍历方式，主要是对题意的理解，默认只会有10，11，0这三种形式出现

##### (1).常规解法, 1 层循环，时间复杂度 O(n)

```golang
func isOneBitCharacter(bits []int) bool {
 if len(bits) == 0 {
  return false
 }
 index := 0
 for index < len(bits) {
  if bits[index] == 1 {
   index += 2
   continue
  }
  if bits[index] == 0 {
   if index == len(bits)-1 {
    return true
   }
   index += 1
   continue
  }
 }
 return false
}
```

##### (2).对第一种方式的简化写法，算法复杂度与（1）类似，巧妙判断最后一个位（题意最后一位永远是0）

```golang
func isOneBitCharacter(bits []int) bool {
 l := len(bits)
 for index := 0; index < l; index++ {
  if index == l-1 {
   return true
  }
  if bits[index] == 1 {
   index += 1
  }
 }
 return false
}
```

## 2.Review
* [HTAP(Hybird transaction/analytical processing)](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing)
* [How We Build an HTAP Database That Simplifies Your Data Platform](https://en.pingcap.com/blog/how-we-build-an-htap-database-that-simplifies-your-data-platform/)
* [The Past, Present, and Future of TiDB as an HTAP Database](https://pingcap.medium.com/the-past-present-and-future-of-tidb-as-an-htap-database-b9fa96ecd1c7)
* [Making an HTAP Database a Reality: What I Learned From PingCAP’s VLDB Paper](https://medium.com/swlh/making-an-htap-database-a-reality-what-i-learned-from-pingcaps-vldb-paper-6d249c930a11)

## 3.Tip

* 最小Demo体验与完善的清理机制
最近在看TiDB的相关资料，再结合之前的SkyWalking的show case，[Quick Start Guide for the TiDB Database Platform](https://docs.pingcap.com/tidb/stable/quick-start-with-tidb)和[SkyWalking showcase](https://skywalking.apache.org/docs/skywalking-showcase/latest/readme/)；由此想到一个官方的起步教程已经非常简练完善，再结合官方提供的相关工具，就可以快速搭建起一个包括数据与UI界面的体验环境，极大了降低了学习者的初次使用的学习成本，同时也非常友好的向学习者展示了项目的功能，非常容易引入学习者去尝试使用项目。

## 4.Share

* [传统行业数据架构发展变化](https://asktug.com/t/topic/513467)
文章梳理了数据架构演进的几个阶段，非常值得参考借鉴
  * 单一数据库集群
  * TP和AP分离
  * 大数据的引入
  * HTAP+云原生