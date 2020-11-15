---
layout: post
title:  "递归/前序遍历，整数反转，位运算"
date:   2020-11-10 06:38:07 +0800
categories: jekyll update
---

# 递归/前序遍历，整数反转，位运算

涉及题号：

7. 整数反转

129. 求根到叶子结点数字之和

144. 二叉树的前序遍历

递归和前序遍历很像，都是要深入到最底层，再一层一层退出，所以第7题和第144题的解题思路差不多。

第7题和第129题则是要达到的目的差不多。

要点：

result = result * 10 + x % 10

`result*10`获取高位，`x%10`获取个位的数值

位运算：`（x/10） | 0` 取整

`(result | 0) === result ? result : 0` 做溢出判断，若溢出则返回 0


遍历数组或字符串：

```javascript
for(let char of chars) {}
```

# 哈希表

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

# 迪克斯特拉算法

# 回溯算法

涉及题号：

127. 单词接龙

搜索时含多条支线。若使用递归，根据某些判断条件进入某一条支线后，数据有更改，递归结束，需返回上一层/若干层尝试另一条支线，需要复原数据有变化的部分，再进入新的支线，称为回溯法。

```javascript
        while(arr) {
            person = arr.pop()
            if (person == undefined) {
                break
            } else {
                step++
              // 检查过的人，记录在ed数组中
                ed.push(person)
              // 递归直到最深层
                good(person,step,ed)
              // 进行下一次循环(走另一条支线)之前，取消之前做的记录
                ed.pop()
              // 取消增加
                step--
            }
        }
```

