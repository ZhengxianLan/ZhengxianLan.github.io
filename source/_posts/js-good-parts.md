---
title: javascript-good-parts
date: 2014-10-02 07:15:52
categories: ['javascript','good-parts']
tags: ''
---
1. falsy 值:
```javascript
var arr=[false,null,undefined,0,'',NaN];
// Notice that, `!!' '` => true
arr.forEach(function(e){console.log(e+'=>'+ !!e+'type:'+typeof(e))
})
// false     => false  type: boolean
// null      => false  type: object
// undefined => false  type: undefined
// 0         => false  type: number
// ''        => false  type: string
// NaN       => false  type: number
```

2. 通过 object.hasOwnProperty(var) 来确定这个属性名是否是对象的成员还是从其原型链获取的的
3. Typeof 运算符产生的值有
	- number // typof(NaN) => 'number'
	- string
	- boolean
	- undefined
	- function
	- object // typeof(null) => 'object'
4. js 类型: [see mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
 - Six data types that are primitives:
		Boolean
		Null
		Undefined
		Number
		String
		Symbol (new in ECMAScript 6)
  - and Object
5. js 包括一个原型链特性, 允许对象继承另一对象的属性, 正确的使用它能减少对象初 始化的时间和内存的消耗
6. 对象字面中, 若属性是一个合法的 js 标志符且不是保留字, 并不强制要求用引号括住属性名。
7. 原型连接在对象更新时不会起作用。当我们对某个对象作出更新操作, 不会触及该对象的原型。
8. 反射。检查对象并确定对象有什么属性是很容易的事情, 只要试着去检索该属性并验证取得的值
	- Typeof 操作符对确定属性的类型很有帮助。请务必注意原型链中任何属性也会产 生一个值
	- HasOwnProperty, 检查属性是否为对象独有。HasOwnProperty 不会检查原 型链
9. 使用 for in 遍历对象会把所有属性包括函数和原型链中的属性迭代出来, 并且属性是 不确定的。
每个函数在创建时附有 2 个附加的隐式属性: 函数的上下文和函数体。
12. 每个函数对象在创建时也随带有一个 prototype 属性。它的值是一个拥有 construtor 属性且值即为该函数的对象。
13. 在 js 中一共有四种调用模式: 这些模式在如何初始化关键字参数 this 存在差异
	- 方法调用模式
	当一个函数被保存为对象的一个属性时, 成为方法。当一个方法被调
用时,this 被绑定到该对象。方法可以使用 this 去访问对象, 故它能从对象中取值或
修改对象。This 到对象的绑定发生在调用时, 这种超级迟绑定使得函数对 this 高度复
用。通过 this 可取得它们所属对象的上下文的方法称为公共方法。
	- 函数调用模式
	当函数并非一个对象的属性, 那么它被当作一个函数来调用。

  - 构造器调用模式
  若在一个函数面前带上 new 调用, 那么将创建一个隐式对象连接到
该函数的 prototype 成员的新对象, 同时 this 将会绑定到那个新对象上。
```javascript
var Q=function(str){this.status=str}
Q.prototype.get_status=function(){return this.status}
var myQ=new Q("confused");
console.log(myQ.get_status());// confused
var q2=Q('methodCall'); // 由于是方法调用, 没有隐式返回一个对象。而是像普通函数一样无返回 值则 q2 为 undefined
q2.get_status();        // TypeError: Cannot call method 'get_status' of undefined
```
	- Apply 调用模式
	接收 2 个参数, 第一个是将被绑定给 this 的值, 第二个就是一个参数数组
18. 参数。当函数被调用时, 会得到一个 arguments 类数组对象。
19. 通过给基本类型增加新方法, 我们可以大大提高语言的表现力。因为 js 原型继承的动态本质, 新的方法立刻被赋予到所有的对象实例上, 哪怕对象实例实在方法在添加到 原型之前就创建了。
20. 一些语言提供了尾递归优化, 这意味着若一个函数返回自身调用结果, 那么调用的过程被替换为一个循环, 它可以显著提高速度。
21. 作用域。最好的做法是在函数体的顶部声明函数中可能用到的所有变量。
22. 闭包: 作用域的好处是内部函数可以访问定义它们的外部函数的参数和变量 (except this and arguments)。函数可以访问它被创建时所处的上下文环境, 这成为闭包。 内部函数能访问外部函数的实际变量而无需复制。
23. 在基于类的语言中, 对象是类的实例, 并且类可以从另一个类继承。 Javascript 是一种基于原型的语言, 这意味着对象直接从对象继承。
24. undefined 和 NaN 并非常量, 它们是全局变量, 而且可以更改他们的值。这是违反常 识的设计, 但是, 它确实存在。
25. js 没有真正的数组, 它很容易使用, 但是它的性能不是很好。
26. hasOwnProperty。该方法可以用于 for in 遍历不必要属性的问题, 但是该方法不是运算符, 而是一个函数, 在任何对象中, 它可能被一个不同函数, 甚至非函数值替换。

