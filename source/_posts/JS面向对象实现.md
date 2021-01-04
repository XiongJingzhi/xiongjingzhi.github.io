---
title: JS面向对象实现
date: 2018-08-10 07:18:54
tags: front-end
---

## JS 语法前提

面向对象实现无非三个特征，封装，继承，多态。

JS 实现基于构造函数（constructor）和原型链（prototype）。

JavaScript 引擎先寻找对象本身的属性，如果找不到，就到它的原型去找，如果还是找不到，就到原型的原型去找。

关于 this 知识：

- 创建一个空对象，作为将要返回的对象实例。
- 将这个空对象的原型，指向构造函数的 prototype 属性。
- 将这个空对象赋值给函数内部的 this 关键字。
- 开始执行构造函数内部的代码。
- if Constructor returns object, instanceObject will get the object, or else get 'this object'.

## JS 实现

构造函数实现封装：

```javascript
function Person(name, age) {
  // 优先级最高
  this.name = name
  this.age = age
}
// prototype 实例共享的属性和方法，节省内存
Person.prototype.type = 'human'

Person.prototype.sayName = function() {
  console.log(this.name)
}

var instanceObject = new Person('张三', 22)
```

实现继承：

```javascript
// 构造继承
function Student (name, age, gender) {
  // 借用构造函数继承属性成员
  Person.call(this, name, age)
  // 可以覆盖，重写
  this.gender = gender
}
// 复制父构造函数的原型对象
// 也可以使用Student.prototype = new Person()，但会有Person的实例属性
Student.prototype = Object.create(Person.prototype)
// 单个方法的继承
|| Student.prototype.print = function() { Person.prototype.print.call(this) }
// 指定原型对象的构造函数
Student.prototype.constructor = Student
// 可以覆盖，重写
Student.prototype.method = '...'

var instanceObject = new Student('张三', 22, 'man')
// 此时，instanceObject instanceof Student || Person 都为true
```

实现多重继承 Mixin：

```javascript
function M1() {
  this.hello = 'hello'
}
function M2() {
  this.world = 'world'
}
function S() {
  M1.call(this)
  M2.call(this)
}
// 继承 M1
S.prototype = Object.create(M1.prototype)
// 继承链上加入 M2， 对象覆盖
Object.assign(S.prototype, M2.prototype)
// 指定构造函数
S.prototype.constructor = S

var s = new S()
s.hello // 'hello：'
s.world // 'world'
```

## ES6 实现

可以看作是 Syntactic sugar

类的声明：

```javascript
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  sayName() {
    console.log(this.name)
    return this.age
  }
}
```

继承的实现：

```JavaScript
class Student extends Person {
  constructor(name, age, gender) {
    // super函数必不可少,调用父类的constructor(x, y)
    // 在es6内部实现中，子类的this必须通过父类的构造函数完成塑造
    super(name, age)
    // 由super返回了Student的实例，相当于Person.prototype.constructor.call(this)
    this.gender = gender
  }
  toString() {
    return this.gender + ' ' + super.sayName() // 调用父类的toString()
  }
}

var instanceObject = new Student('张三', 22, 'man')
// instanceObject的__proto__指向Student的prototype, 有toString方法，
// 它的__proto__指向Person的prototype, 有sayName方法
```
