---
title: coffee-script
date: 2015-10-15 01:48:22
categories: ['javascript']
tags: ['coffee']
---

### intro

coffeescript,真是听有意思的，ruby的甜美，python的工整，可读性，消除二义性都比原生的js要舒服多了
- Optional parentheses if args exists
- Lambda from ruby `->`
- From python import comprehension list
- Conveninent extends
- All instance method will be added to prototype
- Get @ from ruby
- String interpolation seems more beautiful

### some demo

1. 灵活的 ?
  - Test whether exists
```javascript
solipsism = true if mind? and not world?
---
if ((typeof mind !== "undefined" && mind !== null)
&& (typeof world === "undefined" || world === null)) {
  solipsism = true;
}
  ```
  - More than exists,determined by the very followed operator, ? can generator the code you want
    - Assignment
```javascript
speed ?= 15 // work like or= , just like ||= in ruby
```
     - Ternary Conditional Operator
```javascript
footprints = yeti ? "bear"
---
footprints = typeof yeti !== "undefined" && yeti !== null ? yeti : "bear";
```

     - Simplicity is power.
```javascript
 var ref, zip;
zip = lottery.drawWinner?().address?.zipcode

--- if drawWinner is func ,then if address exists ,then zipcode

zip = typeof lottery.drawWinner === "function"
  ? (ref = lottery.drawWinner().address) != null
     ? ref.zipcode : void 0
  : void 0;
```
5. Really cool,just like open class in ruby ,using ruby module syntax
 ```javascript
  String::dasherize = ->     | String.prototype.dasherize = function() {
    this.replace /_/g, "-"   | return this.replace(/_/g, "-");
                             | };
 ```
6. Swap variables is easier
  [theBait, theSwitch] = [theSwitch, theBait]
  and this make possible for  dealing with functions that return multiple values.
7. 最后 close=ref[i++],里面的i++到底有几毛钱的必要？
```javascript
tag = "<impossible>"
[open, contents..., close] = tag.split("")

---
var close, contents, i, open, ref, tag,
slice = [].slice;
tag = "<impossible>";

ref = tag.split(""),
open = ref[0],
contents = 3 <= ref.length
  ? slice.call(ref, 1, i = ref.length - 1)
  : (i = 1, []),
close = ref[i++];
```
7. 胖箭头 => 让内部函数使用外部this更方便;妈妈再也不用担心我绑错this了
