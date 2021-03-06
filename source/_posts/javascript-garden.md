---
title: javascript-garden
date: 2015-10-15 11:45:33
categories: ['javascript']
tags: ['garden']
---


### [javascript 花园](http://bonsaiden.github.io/JavaScript-Garden/)

1. 以前难以理解的 prototype，在玩了 ruby 之后，再去看反而容易多了
而且，也有提到 monkey patching。之前是看 pdf，现在看 html，而且很多东西还是英文描述准确一些，翻译总有一些偏差。
2. 在脚本运行之前，所有的声明都会被置顶，但是赋值操作却是在函数执行到所在行时才会执行。这样会造成错误。用 coffeescrip 就好了
```javascript
foo; // 'undefined'
foo(); // this raises a TypeError
var foo = function() {};
```
3. 赋值给变量的命名函数，在函数外部并不可见
```javascript
var foo = function bar() {bar(); // Works
}
bar(); // ReferenceError
```
4. 闭包是 js 比较难理解的部分。强大又容易出错。
  - 闭包的要点
    1. 作用域： 内部域可访问外部域的属性，而 js 的域只有一个，即 function
    2. 生命周期： 即使外部域生命周期已经结束，而内部域却仍然持有外部域的属性引用
  - 好处：可以用来保护变量不受外界干扰
```javascript
function Counter(start) {
  var count = start;
  return {increment: function() {count++;},

      get: function() {return count;}
  }
}

var foo = Counter(4);
foo.increment();
foo.get(); // 5
```

  - 陷阱： 引用错误
    ```javascript
    for(var i = 0; i < 5; i++) {setTimeout(function() {console.log(i);
        }, 1000);
    }
    // 匿名函数分享了外部作用域的 i
    // 而我们需要函数的执行来激发新的作用域链
    // 所以
    for(var i = 0; i < 5; i++) {(function(e) {setTimeout(function() {console.log(e);
            }, 1000);
        })(i);
    }
    // 不一定需要通过函数传参，我们想要的只是通过执行一个匿名函数来激活作用域
      for(var i = 0; i < 5; i++) {(function() {
              var copy=i; # create a copy
              setTimeout(function() {console.log(copy);
              }, 1000);
          })();}

    ```
