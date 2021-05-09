---
title: vue生命周期
date: 2021-05-04 22:52:22
tags:
   - vue
---
# Vue生命周期

## 过程

一个组件**创建、数据初始化、挂载、更新、销毁**。

## 方法

- beforeCreate
- created
- beforeMount
- mounted
- (beforeUpdate
- updated)
- beforeDestroy
- destroyed

## 生命周期图

[【Vue】详解Vue生命周期]: https://www.cnblogs.com/penghuwan/p/7192203.html



1. new Vue

   生成vue实例

2. Init Events & Lifecycle

   事件和生命周期钩子初始化

3. beforeCreate

   在data初始化之前、event/watcher事件配置前调用

4. Init injections & reactivity

   初始化，注入data，形成双向绑定和响应式系统

5. created

   data、计算属性、event/watcher事件回调已初始化，DOM树未挂载

6. Has "el" option

   提供`"el"`选项意义在于，通过`el`找到将要渲染的模板，再将模板编译成`render函数`。

7. no-> `vm.$mount(el)`is called

   没有该选项则不进行下一步的挂载，此时也不执行`beforeMount`及以后的函数。

8. Yes 

   直到手动挂载，`vm.$mount('#app')`，则把实例挂载到`app`上。

9. has "template" option

10. no-> compile el's outerHTML as template

    如果没有`template`参数选项，则将外部的HTML作为模板编译（template），也就是说，`template`参数选项的优先级要比外部的HTML高

11. Yes-> compile template into render function 

    如果Vue实例对象中有`template参数选项`，则将其作为模板编译成render函数。模版可以卸载`template参数选项`中，也可写在外部`HTML`中

    *在Vue中，有render函数这个选项,它以createElement作为参数，做渲染操作*

    *Vue的编译过程实际上是指Vue把模板编译成 `render函数`的过程*

12. beforeMount

13. Create vm.$el and replace "el" with it

14. mounted

15. changes->beforeUpdate->virtual DOM re-render and patch->updated

16. beforeDestroy

17. Teardown waters, child components and event listeners

18. Destroyed 

![img](https://upload-images.jianshu.io/upload_images/13119812-eaf493b1b2050a93.png?imageMogr2/auto-orient/strip|imageView2/2/w/720/format/webp)