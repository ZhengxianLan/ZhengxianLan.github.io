---
title: coffee-script
date: 2015-10-15 01:48:22
categories: ['js']
tags: ['coffee']
---

coffeescript,真是听有意思的，ruby的甜美，python的工整，可读性，消除二义性都比原生的js要舒服多了
optional parentheses if args exists
lambda from ruby
from python import comprehension list
conveninent extends
all instance method will be added to prototype
get @ from ruby
string interpolation seems more beautiful



1. 加了问号,借鉴ruby中返回布尔值方法
  solipsism = true if mind? and not world?
  --->
  if ((typeof mind !== "undefined" && mind !== null) && (typeof world === "undefined" || world === null)) {
    solipsism = true;
    }
2. speed ?= 15 类似 ruby的 ||=, 好吧，其实coffeescrip有 or=
3. footprints = yeti ? "bear" 
  --->  
  直接就变3元表达式 
  footprints = typeof yeti !== "undefined" && yeti !== null ? yeti : "bear";
4. 这么狂野，真的好理解么,andand gem in ruby. if drawWinner is func ,then if address exists ,then zipcode
 var ref, zip;
zip = lottery.drawWinner?().address?.zipcode | zip = typeof lottery.drawWinner === "function" ? (ref = lottery.drawWinner().address) != null ? ref.zipcode : void 0 : void 0;
5. really cool,just like open class in ruby ,using ruby module syntax 
 ```javascript
  String::dasherize = ->     | String.prototype.dasherize = function() {
    this.replace /_/g, "-"   | return this.replace(/_/g, "-");
                             | };
 ```
6. swap variables is easier 
  [theBait, theSwitch] = [theSwitch, theBait] 
  and this make possible for  dealing with functions that return multiple values.
7. 这个 close=ref[i++],里面的i++到底有几毛钱的必要？
  tag = "<impossible>"                       | var close, contents, i, open, ref, tag,
  [open, contents..., close] = tag.split("") | slice = [].slice;
                                             | tag = "<impossible>";
                                             |
                                             | ref = tag.split(""), open = ref[0], contents = 3 <= ref.length ? slice.call(ref, 1, i = ref.length - 1) : (i = 1, []), close = ref[i++];

7. 胖箭头 => 让内部函数使用外部this更方便;妈妈再也不用担心我绑错this了
