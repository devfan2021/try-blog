---
title: "2021第1篇ARTS"
date: 2021-12-19T11:40:11+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

本周ARTS部分，还是以Golang语言来解算法题(断断续续学Go，但是工作中一直没用起来)，先进行热身，后续算法部分会将java和c语言的解法也加进来。英文部分，近期会尽量以阅读Redis官方文档为主，将redis的一些重要原理性的知识点啃一啃，分享部分，这周主要还是非技术文章的分享。

## 1.Algorithm

[228]&nbsp;&nbsp;[Summary Ranges](https://leetcode.com/problems/summary-ranges/description/)

**Easy** &nbsp;&nbsp; **array**

You are given a sorted unique integer array nums.

Return the smallest sorted list of ranges that cover all the numbers in the array exactly. That is, each element of nums is covered by exactly one of the ranges, and there is no integer x such that x is in one of the ranges but not in nums.

Each range [a,b] in the list should be output as:

"a->b" if a != b
"a" if a == b

Example1:

```
Input: nums = [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: The ranges are:
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

Example2:

```
Input: nums = [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: The ranges are:
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

**解答**
本题主要考察处理数组和数字转换成字符串的方法

##### (1).常规解法, 循环比较，时间复杂度 O(n)
方法代码冗余

```golang
func solution1(nums []int) []string {
 var retVals []string
 if len(nums) == 0 {
  return retVals
 }

 if len(nums) == 1 {
  return append(retVals, fmt.Sprintf("%d", nums[0]))
 }

 for begin, cmpVal, index := nums[0], nums[0], 1; index <= len(nums)-1; index++ {
  if (cmpVal + 1) == nums[index] {
   cmpVal = nums[index]
  } else {
   if begin == cmpVal {
    retVals = append(retVals, fmt.Sprintf("%d", begin))
   } else {
    retVals = append(retVals, fmt.Sprintf("%d->%d", begin, cmpVal))
   }
   begin, cmpVal = nums[index], nums[index]
  }

  if index == len(nums)-1 {
   if begin == cmpVal {
    retVals = append(retVals, fmt.Sprintf("%d", begin))
   } else {
    retVals = append(retVals, fmt.Sprintf("%d->%d", begin, cmpVal))
   }
  }
 }
 return retVals
}
```

##### (2).简化的写法，提取head, 巧用数组索引

```golang
func solution2(nums []int) []string {
 if len(nums) == 0 {
  return nil
 }

 var retVals []string
 head := 0
 for index := range nums {
  if index < len(nums)-1 && (nums[i]+1) == nums[i+1] {
   continue
  }

  if head == index {
   retVals = append(retVals, strconv.Itoa(nums[index]))
  } else {
   retVals = append(retVals, strconv.Itoa(head)+"->"+strconv.Itoa(nums[index]))
  }
  head = i + 1
 }
 return retVals
}
```

## 2.Review
最近项目中自己动手搭建了几套redis sentinel集群，包括调整成带密码访问等。之前看过一些中文的介绍文档，现在重新读下官方的文档，摘录部分重要部分如下；sentinel充当分布式系统中协调者的角色，故障转移，选主，脑裂等问题，都是非常基础的问题。

[Redis Sentinel Documentation](https://redis.io/topics/sentinel)

Redis Sentinel provides high availability for Redis. Using Sentinel you can create a Redis deployment that resists without human intervention certain kinds of failures.

This is the full list of Sentinel capabilities at a macroscopic level (i.e. the big picture):

**Monitoring**: Sentinel constantly checks if your master and replica instances are working as expected.
**Notification**: Sentinel can notify the system administrator, or other computer programs, via an API, that something is wrong with one of the monitored Redis instances.
**Automatic failover**: If a master is not working as expected, Sentinel can start a failover process where a replica is promoted to master, the other additional replicas are reconfigured to use the new master, and the applications using the Redis server are informed about the new address to use when connecting.
**Configuration provider**: Sentinel acts as a source of authority for clients service discovery: clients connect to Sentinels in order to ask for the address of the current Redis master responsible for a given service. If a failover occurs, Sentinels will report the new address.

**Distributed nature of Sentinel**
Redis Sentinel is a distributed system:

Sentinel itself is designed to run in a configuration where there are multiple Sentinel processes cooperating together. The advantage of having multiple Sentinel processes cooperating are the following:

Failure detection is performed when multiple Sentinels agree about the fact a given master is no longer available. This lowers the probability of false positives.
Sentinel works even if not all the Sentinel processes are working, making the system robust against failures. There is no fun in having a failover system which is itself a single point of failure, after all.

## 3.Tip

前段时间在测试环境上搭建的监控日志(Grafan+Loki)突然不能登录了，界面能打开，但是一直提示登录失败，上服务器查看，确认是软件运行的目录空间满了，当时执行cd命令一直不能补全文件路径，后来通过直接敲打完整路径才能进入，后续用重新规划了程序启动与数据存储的目录，主要有以下几点可以总结的：
* 如果磁盘满了，cd自动补全路径会失效，不要慌，直接敲全路径进行操作
* 规划好软件的安装目录，程序运行的目录与数据存储的路径尽量分开，可以做软链接，方便后续磁盘的扩展
* 采用nohup方式启动的程序，处理好日志的重定向输出，比如 nohup java -jar xxx.jar > log.log 2>&1 &

#### shell管道
* [In the shell, what does " 2>&1 " mean?](https://stackoverflow.com/questions/818255/in-the-shell-what-does-21-mean)
* [What are the shell's control and redirection operators?](https://unix.stackexchange.com/questions/159513/what-are-the-shells-control-and-redirection-operators)
* [What is the difference between &> and >& in bash?](https://superuser.com/questions/335396/what-is-the-difference-between-and-in-bash)
* [Linux shell中2>&1的含义解释](https://blog.csdn.net/zhaominpro/article/details/82630528)

## 4.Share
[杨绛：丰富自己，胜过取悦别人](https://mp.weixin.qq.com/s/agcylLFXz4KOdclokliW4Q)
介绍钱钟书夫人的一篇鸡汤文，文中一些观点和杨先生的做法值得我们学习
* 人这一生，最重要的不是取悦别人，而是丰富自己。不要去追一匹马，你用追马的时间去种草。等到春暖花开的时候，自然会有一群骏马供你选择。
* 你读过的每一本书，学到的每一分知识，都将成为你乘风破浪最大的底气。
* 真正成熟的人，都懂得把眼光放长远，不再拘泥于眼前的鸡毛蒜皮。
* 所谓生活情趣，就是用意趣之心去对待生活中的万事万物，并挖掘其中的美好，为我们所用。

[成不成认命](https://mp.weixin.qq.com/s/aoTi9ipJqZIPpwL__ZbUlA)
二爷鉴书公众号里的一篇文章，是介绍李祥访谈新荣记的张勇。当时看到这篇文章，也是深有感触，当即在淘宝上下单买了这本书《详谈06：张勇（在质上进取 而非在量上贪婪）》
* 自己划定舒适区和擅长区
* 人要认命，要知深浅。命运给你饭吃，年轻的时候要多尝试，找到这口饭是什么，然后不管丰盛，都踏踏实实端好，别再瞎折腾。
* 海底捞我只能羡慕，但是我自己在我的事情里面也挺开心的，最起码我也觉得自己的价值得到了实现。
* 开的每个店都要亲自去：亲自选址、亲自确定菜单、亲自试菜。
* 努力去做一个自己喜欢的、能代表自己、符合自己审美的和价值观的产品。至于能不能被追捧，能不能成功，能大成功还是小成功，就交给命吧。