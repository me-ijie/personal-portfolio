---
title: 一些JS方法总结
date: 2021-05-04 22:49:23
tags:
  - JavaScript 
---
# Math.sqrt 开平方根

```javascript
let numbers = [1, 4, 9]
let roots = numbers.map(function(num) {
    return Math.sqrt(num)
})
// roots is now     [1, 2, 3]
// numbers is still [1, 4, 9]
```

# parseInt

接受两个参数，第二个参数是转换的基数

# call

- 使别的对象也拥有某个对象的某个函数/方法
- 调用这个方法时，将`this`绑定到指定的对象



```javascript
// 接受name 和price两个参数分别作为name和price属性的值
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) { // 传入 name 和 price 的值
	// Product 拥有name 和 price 属性，
	// 将this 换成 Food，Food 也拥有了 name和price属性
	Product.call(this, name, price);
}
// 创建一个新的food实例对象，继承了Food的name和price属性
console.log(new Food('cheese', 5).name)
```

案例：

- 调用匿名函数

- 调用父构造函数

- 调用函数并指定`this`

  

# apply

与`call`不同的地方在于它接收一个参数数组。`call`接收一个参数列表

语法：

`func.apply(thisArg, [argsArray])`

案例：

- 将数组各项添加到另一个数组
- 使用`apply`以避免遍历数组（注意数组长度）
- 将参数列表转成类数组对象
- 链接构造器

# this

This 取决于函数调用时的上下文环境。

（箭头函数的`this`在建立时决定，而非调用时）

函数在创建时，`this`是不明确的：

```javascript
function Person(){
         alert(this);
    } 
```

## constructor 与 prototype 的关系

```javascript
function Person(name, age){
        this.name = name;
        this.age = age;
    }
// 用 Person 构造器函数创建 obj 实例对象
// 创建时同时会隐性定义 obj.constructor: Person
    var obj = new Person();
// obj 实例对象拥有 constructor 属性，指向 Person
// 即：obj 实例对象的构造函数为 Person 
    alert(obj.constructor == Person);// true
```

上面提到`隐性定义 obj.constructor: Person`，但其实，`obj`的`constructor`属性存于原型链中。有可能该`constructor`的值会被重写覆盖：

```javascript
function Person(name, age){
        this.name = name;
        this.age = age;
    }
    // 将prototype重新指向下面的匿名对象
    Person.prototype = {
        getName: function(){
           alert(this.name);
        },
        getAge: function(){
           alert(this.age);
        }
    }
    var obj = new Person();
    alert(obj.constructor == Person);// false
```

强制指定`constructor`：

```javascript
Person.prototype = {
        constructor: Person, //强制指向Person
        getName: function(){
           alert(this.name);
        },
        getAge: function(){
           alert(this.age);
        }
    }
```

```javascript
new Person ={
this.color = color,
setColor= function() {},
getColor = function() {}
}
var p = new Person('yellow')
var test = p.setColor
test() // this 指向 window
// this 指向调用环境，而非是谁的属性
function fun1() {
  funnction funn2() {
    connsoloelogg(this)
  }
  fun2() // this 是 window
  // 函数都是通过对象来调用的
}
fun1（） // this 是 window
```

# 数据类型的判断

1. typeof：六种结果

   undefined

   boolean

   Number (NaN)

   string

   object (object, [], null)

   function

2. instanceOf

3. ===

   undefined

   Null

