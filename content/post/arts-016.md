---
title: "2022第1篇ARTS"
date: 2022-01-09T18:21:00+08:00
tags: ["ARTS"]
categories: ["ARTS"]
---

本周Review部分阅读了几篇关于Rust的文章，Rust语言慢慢在前端基建方面得到不错应用场景，之前只是停留在Rust与WebAssembly的了解；Share部分分享了D2C相关的知识，对自己来说算是一个比较新的知识盲区，基于模板的定制开发方式将下沉成为一项基础标配功能，而对照着设计稿，将设计稿直接手撕代码的时代可能不会太久了，毕竟公司，组织要讲究效率，而前端又是需求变更非常频繁的地方，各种营销活动，UI改版等都是非常强的需求。

## 1.Algorithm - Vowels, Two Pointers

[345]&nbsp;&nbsp;[Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)

**Easy** &nbsp;&nbsp; **Two Pointers** &nbsp;&nbsp; **String**

Given a string s, reverse only all the vowels in the string and return it.

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both cases.

Example 1:

```
Input: s = "hello"
Output: "holle"
```

Example 2:

```
Input: s = "leetcode"
Output: "leotcede"
```

**解答**

##### (1).常规解法, 双指针遍历，时间复杂度 O(n)
注意byte与string的互相转换

```
func reverseVowels(s string) string {
	if len(s) <= 1 {
		return s
	}

	b := []byte(s)
	begin, end := 0, len(b)-1
	for begin < end {
		if !isVowel(b[begin]) {
			begin++
			continue
		}

		if !isVowel(b[end]) {
			end--
			continue
		}
		b[begin], b[end] = b[end], b[begin]
		begin++
		end--
	}

	return string(b)
}

func isVowel(c byte) bool {
	if c < 'a' {
		c += 32
	}
	return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u'
}
```

##### (2).方法类似1, 双指针遍历，构造map进行遍历比较，时间复杂度 O(n)
注意byte与string, byte与rune之间的互相转换

```
func reverseVowels(s string) string {
	if len(s) <= 1 {
		return s
	}

	vowels := "aeiouAEIOU"
	vowelMap := make(map[byte]bool)
	for _, val := range vowels {
		vowelMap[byte(val)] = true
	}

	b := []byte(s)
	begin, end := 0, len(s)-1
	for begin < end {
		if !vowelMap[b[begin]] {
			begin++
			continue
		}

		if !vowelMap[b[end]] {
			end--
			continue
		}

		b[begin], b[end] = b[end], b[begin]
		begin++
		end--
	}
	return string(b)
}
```

## 2.Review - It's Rust all the way down
Rust 是写前端基建，是当下趋势；Rust 语言在前端工具链的影响越来越大
* [Rust Is The Future of JavaScript Infrastructure](https://leerob.io/blog/rust)
* [Hacker News: Rust Is the Future of JavaScript Infrastructure (leerob.io)](https://news.ycombinator.com/item?id=29192088)
* [rust-fe](https://github.com/i5ting/rust-fe)

## 3.Tip - byte and rune(golang)
### 3.1 byte and rune
* Golang has integer types called byte and rune that are aliases for uint8 and int32 data types
* The byte data type represents ASCII characters while the rune data type represents a more broader set of Unicode characters that are encoded in UTF-8 format.
* The default type for character values is rune

### 3.2 deploy local jar to local maven repository
* mvn install:install-file -Dfile=/path/XXX-1.0.0.jar -DgroupId=com.XX -DartifactId=XXX -Dversion=1.0.0 -Dpackaging=jar
* mvn install:install-file -Dfile=/path/XXX-1.0.0-source.jar -DgroupId=com.XX -DartifactId=XXX -Dversion=1.0.0 -source -Dpackaging=jar

参考
* [Strings, bytes, runes and characters in Go](https://go.dev/blog/strings)
* [byte and rune](https://www.bogotobogo.com/GoLang/GoLang_byte_and_rune.php)
* [What is a rune?](https://stackoverflow.com/questions/19310700/what-is-a-rune)
* [mvn install本地安装jar到指定仓库](https://www.cnblogs.com/littleorange7/p/14741827.html)

## 4.Share - D2C(Design To Code)
前端从最初的脚手架工具、组件库、持续集成体系、自动化测试、多端适配到现在的全面低代码平台、前端智能化、在线IDE，大前端一直朝着提高生产力，提高研发效能的方向演进。

在D2C解决方案上，目前有以下几种开源方案：[CodeFun](https://code.fun/), [imgcook](https://www.imgcook.com/),[58 Picasso](https://github.com/wuba/Picasso)，具体对比可以参考以下几个附件。可能方案有更多，只是自己刚刚了解这块，待后续学习后补充。通过去这三种方案的相关官网了解，觉得阿里的imgcook在文档，使用教程方面都做的非常不错，特别是关联博客中的智能生成代码2020，2019系列文章，都非常不错，比较容易入门。

* [智能UI：面向未来的UI开发技术](https://mp.weixin.qq.com/s/1RNEQb8N68Muu6YmFa-QrQ?spm=taofed.bloginfo.blog.5.63245ac8SddpBJ)
* [前端智能化实践— P2C 从需求文档生成代码](https://fed.taobao.org/blog/taofed/do71ct/ffeogu/)
* [前端智能化D2C到底怎么样了](https://segmentfault.com/a/1190000040559232)
* [人机协同时代，AI助力90.4%双11前端模块自动生成](https://www.imgcook.com/blog/2020-foreword)