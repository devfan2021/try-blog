---
title: "2021第2篇ARTS"
date: 2021-12-26T11:15:11+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

算法题908是采用LeetCode随机方式选出来的一道题，题目不难，但是题目本身的描述有点让人感觉困惑，也遭到很多人的吐槽。文章解读和工作中的归纳总结部分，以Prometheus的监控部分为主，自己想暂时想先把微服务相关的中间件整体在虚机上串一遍，后续将整体在k8s的集群模式下再部署一套，技术分享部分本周是耗子叔的一篇经典文章，非常值得细细品味。

## 1.Algorithm
[908]&nbsp;&nbsp;[Smallest Range I](https://leetcode.com/problems/smallest-range-i/)

**Easy** &nbsp;&nbsp; **array** &nbsp;&nbsp; **math**

You are given an integer array nums and an integer k.

In one operation, you can choose any index i where 0 <= i < nums.length and change nums[i] to nums[i] + x where x is an integer from the range [-k, k]. You can apply this operation at most once for each index i.

The score of nums is the difference between the maximum and minimum elements in nums.

Return the minimum score of nums after applying the mentioned operation at most once for each index in it.

Example1:
```
Input: nums = [1], k = 0
Output: 0
Explanation: The score is max(nums) - min(nums) = 1 - 1 = 0.
```

Example2:

```
Input: nums = [0,10], k = 2
Output: 6
Explanation: Change nums to be [2, 8]. The score is max(nums) - min(nums) = 8 - 2 = 6.
```

Example3:

```
Input: nums = [1,3,6], k = 3
Output: 0
Explanation: Change nums to be [4, 4, 4]. The score is max(nums) - min(nums) = 4 - 4 = 0.
```

Constraints:
```
1 <= nums.length <= 104
0 <= nums[i] <= 104
0 <= k <= 104
```

**解答**
采用最简单的解法，寻找数组中最大值和最小值。
这个题目本身不难，刚开始读题的时候，感觉有点困惑，但是细想下来，解题思路还是挺简单的。leetcode官网也有很多人吐槽该题目，读题15分钟，解题2-3分钟。[Very confusing question!](https://leetcode.com/problems/smallest-range-i/discuss/178344/Very-confusing-question!)
##### (1).常规解法, 1 层循环，时间复杂度 O(n)

```
func smallestRangeI(nums []int, k int) int {
  if len(nums) == 0 {
    return 0
  }

  minVal, maxVal := nums[0], nums[0]

  for _, val := range nums {
    if val < minVal {
      minVal = val
    }
    if val > maxVal {
      maxVal = val
    }
  }

  val := maxVal - minVal - 2 * k
  if val < 0 {
    return 0
  }
  return val
}
```

##### (2).类似方法1，网上最优的Go解法，与解法1类似

```
func smallestRangeI(nums[] int, k int) int {
  min, max := nums[0], nums[0]
  for _, x := range nums {
    if x < min {
      min = x
    }
    if x > max {
      max = x
    }
  }

  min += k
  max -= k
  score := max - min
  if score < 0 {
    return 0
  }
  return score
}
```

## 2.Review
以下是几篇有关监控软件Prometheus如何合理设置label的文章，挺不错的
* [Target labels are for life, not just for Christmas](https://www.robustperception.io/target-labels-are-for-life-not-just-for-christmas)
* [Life of a Label](https://www.robustperception.io/life-of-a-label)
* [Controlling the instance label](https://www.robustperception.io/controlling-the-instance-label)

## 3.Tip
* redis主从复制失败(master_link_status:down)(https://www.yht7.com/news/109781)
* Prometheus' file-based service discovery：[Use file-based service discovery to discover scrape targets](https://prometheus.io/docs/guides/file-sd/)
```
Prometheus数据源的配置主要分为静态配置和动态发现
static_configs: 静态服务发现
file_sd_configs: 文件服务发现
dns_sd_configs: DNS 服务发现
kubernetes_sd_configs: Kubernetes 服务发现
consul_sd_configs: Consul 服务发现

目前在自己的实践中，用过static_configs和file_sd_configs两种方式，而用file_sd_configs的方式可以基于扫描文件夹下的匹配文件动态更新抓取数据节点
```
* 使用Nacos已经是比较常见了，尝试去搜索Prometheus基于Nacos的服务发现机制，但是网上大部分是基于Consul的服务发现机制，Prometheus官方文档也是基于Consul的，在github上找到一个[Nacos Consul Adapter](nacos-consul-adapter) for Prometheus项目，[nacos-prometheus-discovery](https://github.com/xmx0632/nacos-prometheus-discovery), 用Go语言开发的，使用nacos的服务发现能力，生成prometheus配置文件。

## 4.Share
* [我做系统架构的一些原则](https://coolshell.cn/articles/21672.html)
这些原则都是非常实用的，当我们读到这些内容的时候，内心都会有一些共鸣，也曾经或多或少的在自己的项目中实践了这里的一些原则，但是感觉有些项目实践的并不是很彻底。记录这些原则，并尝试慢慢在项目实践中慢慢完善。
```
原则一：关注真正的收益而不是技术本身
原则二：以应用服务和API为视角，而不是以资源和技术为视角
原则三：选择最主流和成熟的技术
原则四：完备性会比性能更重要
原则五：制定并遵循服从标准、规范和最佳实践
原则六：重视架构扩展性和可运维性
原则七：对控制逻辑进行全部收口
原则八：不要迁就系统的技术债务
原则九：不要依赖自己的经验，要依赖于数据和学习
原则十：千万要小心X-Y问题，要追问原始需求
原则十一：激进胜于保守，创新与实用并不冲突
```