---
title: JavaScript踩坑之parseInt
date: 2021-5-6 00:00:00
cover: https://img.showydream.com/img/ZrtTjb-javascript.jpg
description: 今天在群里看到一个奇怪的问题，parseInt(0.00000005)输出是5，这个一看不会，引发了我的好奇心。
keywords: parseInt(0.0000005),parseInt,JavaScript,js
categories: 
 - JavaScript
---



先看效果：

<img src="http://img.showydream.com/img/2tGWv2-image-20210429190541592.png" alt="image-20210429190541592" style="zoom:40%;" />

## MDN复习知识点：

> #### [语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt#语法)
>
> ```
> parseInt(string, radix);
> ```
>
> #### [参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt#参数)
>
> - `string`
>
>   要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用  `ToString `抽象操作)。字符串开头的空白符将会被忽略。
>
> - `radix` 可选
>
>   从 `2` 到 `36`，表示字符串的基数。例如指定 16 表示被解析值是十六进制数。请注意，10不是默认值！
>
>   文章后面的[描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt#描述)解释了当参数 `radix` 不传时该函数的具体行为。
>
> #### [返回值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt#返回值)
>
> 从给定的字符串中解析出的一个整数。
>
> 或者 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)，当
>
> - `radix` 小于 `2` 或大于 `36` ，或
> - 第一个非空格字符不能转换为数字。
>
> ```
> parseInt('123', 5) // 将'123'看作5进制数，返回十进制数38 => 1*5^2 + 2*5^1 + 3*5^0 = 38
> ```

### 提示和注释

- 只有字符串中的第一个数字会被返回。


- 开头和结尾的空格是允许的。


- 如果字符串的第一个字符不能被转换为数字，那么 parseFloat() 会返回 NaN。


- 在字符串以"0"为开始时旧的浏览器默认使用八进制基数。ECMAScript 5，默认的是十进制的基数。

#### 特殊情况

​		如果 `radix` 是 `undefined`、`0`或未指定的，JavaScript会假定以下情况：

1. 如果输入的 `string`以 "`0x`"或 "`0x`"（一个0，后面是小写或大写的X）开头，那么radix被假定为16，字符串的其余部分被当做十六进制数去解析。
2. 如果输入的 `string`以 "`0`"（0）开头， `radix`被假定为`8`（八进制）或`10`（十进制）。具体选择哪一个radix取决于实现。ECMAScript 5 澄清了应该使用 10 (十进制)，但不是所有的浏览器都支持。**因此，在使用 `parseInt` 时，一定要指定一个 radix**。
3. 如果输入的 `string` 以任何其他值开头， `radix` 是 `10` (十进制)。

**具体示例：**

```javascript
console.log(parseInt("   12", 10)); // 12
console.log(parseInt("12  ", 10)); // 12
console.log(parseInt("12*******", 10)); // 12
console.log(parseInt("d12*******", 10)); // NaN
console.log(parseInt("012")); // 12
console.log(parseInt("12.34", 10)); // 12
console.log(parseInt(12.34, 10)); // 12

// 特殊情况
console.log(parseInt(011)); // 9
console.log(011.toString()); // 9
console.log(parseInt(0x1a)); // 26
console.log(0x1a.toString()); // 26
```

**坑来了：**

```javascript
console.log(parseInt(0.000005, 10)) // 0，小数点后有5个0
console.log(parseInt(0.0010005, 10)) // 0，小数点后不能直接用科学计数法表示
console.log(parseInt(0.0000005, 10)) // 5，小数点后有6个0

console.log(parseInt(500000000000000000000, 10)) // 500000000000000000000
console.log(parseInt(5000000000000000000000, 10)) // 5，小数点前有22位数
console.log(parseInt(5000000000000100000000, 10)) // 5，小数点前有22位数,科学计数法以后的表达式就不被解析了
```


​		其实是这样的，当小数点后的0的个数小于等于5个时，会采用字面量形式直接表示，当小数点后0的个数大于5个时，会采用科学计数法来表示，即：0.000005不会采用科学计数法，而0.0000005则会转换为5e-7，parseInt方法不会将"e"视为数字，因此只是将5转换为10进制，还是5。**parseInt**不应替代**Math.floor()**。

　　同理，当小数点前数字位数为21及以下的时候，会采用字面量形式直接表示，而当小数点前数字位数大于21的时候。会采用科学计数法，因此6000000000000000000000会转换为科学计数法，为6e+21，将6转换为10进制还是6
