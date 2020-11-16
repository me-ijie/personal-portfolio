---
layout: post
title:  "哈希表：Map in JavaScript"
date:   2020-11-07 06:38:07 +0800
categories: jekyll update
tags: hash-table, Map
---


哈希表常用方法：

```javascript
for(let char of word) {
  wordMap.set(char, (wordMap.has(char) ? wordMap.get(char) + 1 : 1));
  // 存在char，则获取该char的值并加1，不存在则插入该char，并设置值为1
}
```

插入键值对：`wordMap.set(key, value)`

获取键所对应的值：`wordMap.get(key)`，返回`value`

查询是否存在该键：`wordMap.has(key)`，返回布尔值

枚举创建哈希表：

```javascript
const person = new Map([
        ['age', 4],
        ['name', 'John'],
        ['sex', 'male']
    ]);
```

