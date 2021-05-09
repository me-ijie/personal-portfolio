---
title: JavaScript学习
date: 2021-05-04 22:53:17
tags:
  - JavaScript
---
# JavaScript学习



## 正则表达式

### 修饰符 flags

- i - ignore cases
- g - global
- m - multiline
- u - unicode
-   s - strict

### 方法

- Search `str.search(regexp)`  `"I Love JavaScript".search(/LOVE/)`

  如果没找到则返回 `-1`。

- Match `str.match(regexp)`

- 使用 `new RexExp` 创建正则实例

  `let reg = new RegExp("\d\.\d");`

- Test `alert(yourRegExp.test('01:32:54:67:89:AB'))`

  

  

### 字符类

- \d - digit
- \s - space
- \w - word

### 反向类

- \D
- \S
- \W

### 点（.）

- 与 “除换行符之外的任何字符” 匹配

- ```javascript
  alert( "CS 4".match(regexp) ); // CS 4 (space is also a character)
  ```

- 带有“s”标志时点字符类严格匹配任何字符

  `alert( "A\nB".match(/A.B/s) ); // A\nB (match!)`

Unicode

- `alert("A ბ ㄱ".match(/\p{L}/gu) ); // A,ბ,ㄱ`

- ```javascript
  let regexp = /\p{Sc}\d/gu;
  let  str = `Prices: $2, €1, ¥9`;
  alert( str.match(regexp) ); // $2,€1,¥9
  ```

### 锚点 anchors

- ```javascript
  let goodInput = "12:34";
  
  let regexp = /^\d\d:\d\d$/;
  alert( regexp.test(goodInput) ); // true
  ```

- 锚点 `^` 和 `$` 属于测试。它们的宽度为零。

- 唯一一个匹配模式 `^$` 的字符串是空字符串 `""`

- 

### Multiline - m

- 匹配每一行

  ```javascript
  let str =  `1st place: Winnie
  2nd place: Piglet
  33rd place: Eeyore`;
  
  alert( str.match(/^\d+/gm) ); // 1, 2, 33
  ```

- 换行符 \n

  ```js
  let str = `1st place: Winnie
  2nd place: Piglet
  33rd place: Eeyore`;
  
  alert( str.match(/\w+\n/gim) ); // Winnie\n,Piglet\n
  ```

### 词边界：\b

- 和锚点一样，是一种测试、检查

- \w处于：开头，末尾，旁边不是\w时。

- ```javascript
  alert( "Hello, Java!".match(/\bJava\b/) ); // Java
  alert( "Hello, JavaScript!".match(/\bJava\b/) ); // null
  ```

- 也可用于数字：模式 `\b\d\d\b` 查找独立的两位数

### 转义，特殊字符

- 特殊字符：`[ \ ^ $ . | ? * + ( )`

- 例：“/” - `\/`

- 如果使用另一种 new RegExp 方式就不需要转义斜杠：

  `alert( "/".match(new RegExp("/")) ); // '/'`

- 注意⚠️ `alert("\d\.\d"); // d.d`

- 解决：用双斜杠

  ```javascript
  let regStr = "\\d\\.\\d";
  alert(regStr); // \d\.\d (correct now)
  ```

### 集合和范围 [...]

- *集合*：意味着“搜索给定的字符中的任意一个”

- `alert( "Mop top".match(/[tm]op/gi) ); // "Mop", "top"`

- *范围*：在 `[a-z]、[0-9]、[0-9A-F]` 范围内匹配一个字符

- `alert( "Exception 0xAF".match(/x[0-9A-F][0-9A-F]/g) ); // xAF`

- \w - `[a-zA-Z0-9_]`。无法匹配中文象形文字。

- 匹配多语言的通用模式：`[\p{Alpha}\p{M}\p{Nd}\p{Pc}\p{Join_C}]`

- *排除范围*：`[^…]` 

  `alert( "alice15@gmail.com".match(/[^\d\sA-Z]/gi) ); // @, .`

- *在 `[…]` 中不转义*：`[-().^+]` 会查找 `-().^+` 的其中任意一个字符

  ```js
  // 并不需要转义
  let reg = /[-().^+]/g;
  
  alert( "1 + 2 - 3".match(reg) ); // 匹配 +，-
  ```

- *范围和标志“u”*：如果有代理对，需要标志u使其正常工作

  ```javascript
  alert( '𝒳'.match(/[𝒳𝒴]/) ); // 显示一个奇怪的字符，像 [?]
  
  alert( '𝒳'.match(/[𝒳𝒴]/u) ); // 𝒳
  ```

### 量词，缩写

- \d{5} - \d\d\d\d\d 。匹配一个 5 位数

- \d{3,5} - 3 至 5 位的数字

- \d{3,} - 匹配位数大于或等于 3。的数字

- “+” - {1,} 。代表“一个或多个”。

  `alert( "+7(903)-123-45-67".match(/\d+/g) ); // 7,903,123,45,67`

- “?” - {0,1} 。代表“ 0 个或 1 个”。

  `alert( "Should I write color or colour?".match(/colou?r/g) ); // color, colour`

- “*” - {0,} 。代表“任意个”。

  `alert( "100 10 1".match(/\d0*/g) ); // 100, 10, 1`

### 贪婪量词和惰性量词

- *贪婪搜索*：获取尽可能多的字符，再一步步筛选。**回溯**

  ```javascript
  let reg = /".+"/g;
  
  let str = 'a "witch" and her "broom" is one';
  
  alert( str.match(reg) ); // "witch" and her "broom"
  ```

- *懒惰模式*：重复最少次数。“?”

  ```js
  let reg = /".+?"/g;
  
  let str = 'a "witch" and her "broom" is one';
  
  alert( str.match(reg) ); // witch, broom
  ```

- 懒惰模式只能够通过带 `?` 的量词启用

- 替代方法：` "[^"]+"`
- 查找 HTML 注释：`reg = <!--[\s\S]*?-->/g`
- 查找 HTML 标签：`reg = /<[^<>]+>/g`

### 捕获组 capturing group

- `(...)`

- (go+) 匹配`go`，`gogo`，`gogogo`等

- 域名的正则表达式：`/(\w+\.)+\w+/g`或`/([\w-]+\.)+\w+/g`

- 邮箱地址的正则表达式：`/[-.\w]+@([\w-]+\.)+[\w-]+/g`

- *嵌套组*匹配的结果：以数组返回。

- 例：`reg = /<(.*?)>/` 索引 `0` 处为完全匹配。索引 `1` 处为第一个括号的内容

  ```javascript
  let str = '<h1>Hello, world!</h1>';
  
  let tag = str.match(/<(.*?)>/);
  
  alert( tag[0] ); // <h1>
  alert( tag[1] ); // h1
  ```

- *可选组*：不存在也具有相应的结果项。等于 `undefined`

  ```javascript
  let match = 'ac'.match(/a(z)?(c)?/)
  
  alert( match.length ); // 3
  alert( match[0] ); // ac（完全匹配）
  alert( match[1] ); // undefined，因为 (z)? 没匹配项
  alert( match[2] ); // c
  ```

  *(数组长度恒为 3 )*

- 方法 `matchAll`

  `let results = '<h1> <h2>'.matchAll(/<(.*?)>/gi);`

- 方法 `Array.from`

  `results = Array.from(results); // let's turn it into array`

- 解构：

  `let [tag1, tag2] = '<h1> <h2>'.matchAll(/<(.*?)>/gi);`

- *命名组*：`?<name>` 给括号起名字

  ```javascript
  let dateRegexp = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/;
  let str = "2019-04-30";
  
  let groups = str.match(dateRegexp).groups;
  
  alert(groups.year); // 2019
  alert(groups.month); // 04
  alert(groups.day); // 30
  ```

- *替换捕获组*：`str.replace(regexp, replacement)`

- *非捕获组*：`?:`

  ```javascript
  let str = "Gogogo John!";
  
  // ?: 从捕获组中排除 'go'
  let regexp = /(?:go)+ (\w+)/i;
  
  let result = str.match(regexp);
  
  alert( result[0] ); // Gogogo John（完全匹配）
  alert( result[1] ); // John
  alert( result.length ); // 2（数组中没有更多项）
  ```

  <!--/^([0-9a-f]{2}:){5}[0-9a-f]{2}$/i-->

  <!--/#([0-9a-f]{3}){1,2}/gi-->

  <!--/-?\d+(\.\d+)?/g-->

  ```js
  Function parse (expr) {
  let regexp = /(?<a>\b-?\d+(\.\d+)?\b)\s?(?<op>[-+*/])\s?(?<b>\b-?\d+(\.\d+)?\b)/
  let groups = expr.match(regexp).groups
  }
  ```

  

