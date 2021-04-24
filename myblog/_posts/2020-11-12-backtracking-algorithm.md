---
layout: post
title:  "回溯算法（backtracking algorithm）
date:   2020-11-12 06:38:07 +0800
tags: algorithm backtracking
---

# 回溯算法

涉及题号：

127. 单词接龙

在解题时， 可能含有多条解题途径，涉及多条分支；而需要尝试每一条路径后，才找到最优解。
根据判断条件，进入某一条支线后，发现无法继续前进或走到尽头后，需返回上一层/若干层尝试另一条支线。
进入此条支线时，数据有更改，那么返回上一层时，需要复原数据有变化的部分，再进入新的支线。因而称为回溯法。

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




