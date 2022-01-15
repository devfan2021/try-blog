---
title: "2022第2篇ARTS"
date: 2022-01-15T21:40:00+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

本周关注的比较凌乱，主要是看了2篇Github上开发者的调查报告，对2021年的技术趋势总结及程序员比较关注的一些技术；另外一大块，就是微服务，结合阿里的最新微服务的趋势报告，其实微服务的实践理论已经很多了，各行各业都在尝试，但是结合不同公司，不同项目的特点，微服务的实践形式会有多种多样，可能只有到了部分的中间件，或者基于传统的项目，采用http restful的形式。但是基于运行时的多语言解决方案Dapr，是微服务未来的趋势，比如阿里，从Dapr一开始，就投入了人员进行社区的开发支持，也在自己对外的不同产品中，都看到过Dapr的出现。Dapr是值得我们去尝试了解下。

## 1.Algorithm

[383]&nbsp;&nbsp;[Ransom Note](https://leetcode.com/problems/ransom-note/)

**Easy** &nbsp;&nbsp; **String** &nbsp;&nbsp; **hash-table**

Given two stings ransomNote and magazine, return true if ransomNote can be constructed from magazine and false otherwise.

Each letter in magazine can only be used once in ransomNote.

Example 1:

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

Example 2:

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

Example 3:

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

**解答**

##### (1).基于统计每个字母的数量，时间复杂度为O(n), 借助map进行统计

```
func canConstruct(ransomNote string, magazine string) bool {
	vMap := make(map[rune]int)
	for _, val := range magazine {
		vMap[val] += 1
	}

	for _, val := range ransomNote {
		if vMap[val] > 0 {
			vMap[val] -= 1
		} else {
			return false
		}
	}

	return true
}
```

##### (2).与(1)的解法类似，只是提前将2个字符串构造成两个数组，巧妙的采用与字符串'a'相减获取到数组索引下标

```
func canConstruct(ransomNote string, magazine string) bool {
	cmpArr1 := make([]int, 26)
	cmpArr2 := make([]int, 26)

	for _, val := range ransomNote {
		cmpArr1[val-'a'] += 1
	}

	for _, val := range magazine {
		cmpArr2[val-'a'] += 1
	}

	for i := 0; i < 26; i++ {
		if cmpArr1[i] > cmpArr2[2] {
			return false
		}
	}
	return true
}
```

## 2.Review
* [Stack Overflow Annual Developer Survey - 2021](https://insights.stackoverflow.com/survey/2021)
从这份报告里获取到一些对自己有帮助的信息：
  * 数据库：MySQL，PostgreSQL和SQLite使用占比非常高
  * 云平台：AWS占绝对地位，或许由于k8的容器编排技术的发展，Google Cloud和Microsoft Azure市场份额也越来越高了
  * Web frameworks：React.js超过jQuery， Express，Angular, Vue.js都是主流的前端框架
  * 开发者的基础工具：Git占比高达93.43%，Docker占比48.85%，Yarn和Kubenetes占比都超过了15%
  * 最想学的编程语言：Rust占比高达86.98%, 排在前列的分别是：Rust，Clojure,TypeScript,Elixir,Julia,Python,Dart,Swift,Node.js,Go
  * 最想学的数据库：Redis和PostgreSQL占比都高达70%多，其次是MongoDB, Elasticsearch,Firebase
  * 最喜欢的前端框架：Svelte，ASP.NET Core,FastAPI排在榜首，其次是React.js,Vue.js,Express,Spring; 或许是许久没关注前端了，Svelte这个框架之前一直也没关注听过。
  * 工资，经验与开发者的对应关系：![Salary and experience by developer type](/img/arts/Salary-experience-developer-type.jpg)
  * 工资，经验与开发语言的关系：![Salary and experience by language](/img/arts/Salary-experience-developer-type.jpg)

   以上两张对应关系图，还是比较有意思，或许对我们如何选择新的学习语言，有一定参考价值
* [The 2021 State of the Octoverse](https://octoverse.github.com/)
全球最大开发者社区Github的2021 Octoverse报告, 报告总体数据汇总：[octoverse-report-2021.pdf](https://octoverse.github.com/static/octoverse-report-2021.pdf)
  * Writing and shipping code faster
    * Improving developer productivity
    * Scale through automation
    * Reusing code without the friction
    * Search improves software development
    * Where work will happen next
    * Merging pull requests
    * Coordinating pull requests
    * New contributors affect time to merge
    * Day-or-less club
  * Creating documentation to support developers
    * Critical documentation
    * Contribution guidelines
    * README- or not?
    * Issues as documentation
    * Good first issues
    * Docs for productivity and culture
  * supporting sustainable communities
    * Mentorship at work and open source
    * Trust and respect
    * Safe and welcoming communities
    * Gateways to open source
    * Stronger communities
* [Disasters I've seen in a microservices world](https://world.hey.com/joaoqalves/disasters-i-ve-seen-in-a-microservices-world-a9137a51)
  * Disaster #1: too small services
  * Disaster #2: development environments
  * Disaster #3: end-to-end tests
  * Disaster #4: huge, shared database
  * Disaster #5: API gateways
  * Disaster #6: Timeouts, retries, and resilience

* [Software Architecture Patterns: 5 minute read](https://orkhanscience.medium.com/software-architecture-patterns-5-mins-read-e9e3c8eb47d2)
* [Software architecture patterns](https://www.oreilly.com/content/software-architecture-patterns/)
  * Layered Architecture
  * Event-driven Architecture
  * Microkernel Architecture (or Plugin architecture)
  * Microservices Architecture
  * Space-based Architecture (or Cloud architecture pattern)

## 3.Tip
### 3.1 Global .gitignore
在mac系统中默认每个文件下有个.DS_Store，如果用代码管理工具进行管理时，默认就会将.DS_Store文件提交到代码库中，必须在每个项目的.gitignore文件中进行排除

git通过[core.excludesfile](https://git-scm.com/docs/gitignore)定义全局的gitignore文件来忽略一些不需要提交的文件
```
git config --global core.excludesfile ~/.gitignore
```

## 4.Share
* [云原生微服务技术趋势解读](https://mp.weixin.qq.com/s/wHJ1BJlWeJYHVgShHGlF7A), 挺不错的一篇分析文章，其中Dapr的多语言解决方案也被引入，而类似Dapr这种运行时的微服务框架是以后的趋势
  * 云原生微服务作为核心的技术保持着20%左右的高速增长
  * 随着微服务技术的成熟，门槛大幅降低，开始渗透到各行各业
  * 人力成本不断上涨，采用微服务提高研发效率势在必行
  * 90后成为研发主力，微服务独立、敏捷的优势更受年轻人欢迎
  * 微服务技术逐渐成熟，微服务核心架构分层愈加清晰，技术标准化和产业化正在形成，火热的服务网格技术逐渐回归理性，云原生网关作为下一代网关技术逐步成型，微服务技术整体进入深水区