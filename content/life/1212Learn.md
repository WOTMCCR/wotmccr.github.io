---
title: "1212Log"
tags: ["日记"]
categories: ["life"]
date: 2025-12-12
draft: "false"
---

## Day Log

复习了一下昨天的python的基本操作感觉熟练了很多，但是还有一些小的操作不够熟悉会卡顿

写了七道算法题，还可以

## Execution Log

- Linux下的
	- 复制粘贴：
		- `cp file1.txt file2.txt /target/directory/`
	- 复制文件夹：
		- `cp -r source_folder/ destination_folder/`
	- 剪切：
		- `file1.txt file2.txt /target/directory/`
- `sorted(string)` 返回的是 `list[str]`（字符列表）
- `dict.setdefault(key,[]).append()`
- 去除空格标点：
	-  `"".join(ch.lower() for ch in s if ch.isalnum())`
- 判断是否在集合字典直接使用 `in / not in`
## Thinking Log

- 移除元素
	- 应该：两个指针都从前往后遍历，如果后一个指针数在符合要求则直接覆盖前一个指针位置即可
	- 我想的是前后两个指针想向移动，将前面的不符合要求的swap到后面
- 回文串：
	- 用字符串操作，然后reverse比较
- 无重复字符的最长子串，滑动窗口，把窗口内的字符放到：set 或者 dict
	- 使用dict：
		- 可以将 left 直接跳到 重复的 字符的位置
		- 注意要 +1
		- right处元素的判断也要保证 得到的 位置 大于 left 要保证再滑动窗口内
	- 使用set
		- 需要left + 1 循环删除，直到不含有 right 处的字符
- 242 有效的字母异位词：
	- 两个字典，统计每个词中字母的数量
	- 一个字典，加减最后为0
	- python可以直接 dict1 == dict2 判断是否相等
	- 或者用 all()判断是否全为 0
- 49 字母异位词分组：
	- 先想到应该用什么来存储 `key -> List[str]`
	- 然后确定key，注意sort后是字符数组，需要join
	- `strs_dict.setdefault(word_key,[]).append(word)`
	- 列表输出也可以直接 `list(strs_dict.values())`
- 1 两数之和:
	- 可以边遍历边计算是否符合条件，因为可以立即返回并不需要找到所有，
		- 使用dict，key = num ，val = index
	- 我想的是先遍历完再处理，但这里并不需要找到所有
- 128. 最长连续序列：
	- 先全部添加到set中
		- 再遍历set找到 没有 val - 1 存在的 头 元素
		- 再向后遍历计算长度
	- 我受两数之和的影响想着边加数边处理，不行