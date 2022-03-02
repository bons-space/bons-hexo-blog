---
title: Z字形变换
cover: 'https://img.showydream.com/img/oe4Yg6-leetcode.png'
description: Z字形变换
keywords: 'Z字形变换,LeetCode'
categories:
  - LeetCode
tags: LeetCode
abbrlink: e3e2e8e
date: 2022-03-02 11:13:30
---



## 题目描述

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```bash
P   A   H   N
A P L S I I G
Y   I   R
```


之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);


示例 1：

> 输入：s = "PAYPALISHIRING", numRows = 3 输出："PAHNAPLSIIGYIR" 

示例 2：

> 输入：s = "PAYPALISHIRING", numRows = 4 输出："PINALSIGYAHRPI"

解释：

```bash
P     I    N
A   L S  I G
Y A   H R
P     I
```


示例 3：

> 输入：s = "A", numRows = 1 输出："A"


提示：

- 1 <= s.length <= 1000
- s 由英文字母（小写和大写）、',' 和 '.' 组成
- 1 <= numRows <= 1000

## 数学解题思路

这题第一反应是找规律，发现结果字符串的数字下标在原字符串中呈现数学的等差数列
$$
a_n=a+(n-1)d
$$
但是它比等差数列复杂一点的是他是动态的等差数列，我们需要根据numRows动态创建一个二维数组，数组的长度就是numRows，我们先看第一行数字，可以看出它们的下标值的公差`d`是`numRows * 2 - 2`，首项`a`是0，这时候我们看到第二行的竖直列，它们的公差也是`numRows * 2 - 2`，但是他们的`a`是1

<img src="https://img.showydream.com/img/9Rk2SB-image-20220302113242428.png" alt="image-20220302113242428" style="zoom:50%;" />

中间这部分需要特殊处理，因为他跟直线上的数字规律不太一样

<center>每一行的数字下标 + 当前行数-1 = 公差d(numRows * 2 - 2)</center>

由此我们得到代码：

```javascript
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function (s, numRows) {
  const resultArr = []
  for (let j = 0; j < numRows; j++) {
    resultArr[j] = ''
  }
  for (let i = 0; i < s.length; i++) {
    for (let j = 0; j < numRows; j++) {
      if (i % (numRows * 2 - 2) === j || i % (numRows * 2 - 2) === (numRows * 2 - 2 - j)) {
        resultArr[j]+=s[i] // 这里本来创建了一个二维数组，后来发现一维数组字符串就够用了
      }
    }
  }
  return resultArr.join('')
};
```

## 自然解题思路

我们发现当字符串转换的时候是一个一个往下铺的，这个场景有点类似于缝纫机缝衣服，一针一针的缝。字符串是从第一个字母开始一个一个往下放，这时候如果有一个虚拟指针告诉它该放哪一行了，到第numRows行就掉头往上放，到第0行就掉头往下放，以此类推放一遍就OK啦。代码如下：

```javascript
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function (s, numRows) {
  if (s.length === 1 || numRows === 1) {
    return s
  }
  let f = 'up', level = 0
  const resultArr = []
  for (let j = 0; j < numRows; j++) {
    resultArr[j] = ''
  }

  for (c of s) {
    resultArr[level] += c
    if (f === 'up' && level < (numRows - 1)) {
      level++
      f = 'up'
    } else if (f === 'up' && level === (numRows - 1)) {
      level--
      f = 'down'
    } else if (f === 'down' && level > 0) {
      level--
      f = 'down'
    } else if (f === 'down' && level === 0) {
      level++
      f = 'up'
    }
  }
  return resultArr.join('')
}
```

## 总结

这种题需要找规律，找到规律就能从不同维度找出不同的解法
