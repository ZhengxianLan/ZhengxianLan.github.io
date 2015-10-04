---
title: instance_eval
date: 2015-10-04 20:50:42
categories: ['ruby']
tags: ['meta']
---

1. 初次看到instance_eval时是在论坛上看到，同时看到的还有class_eval。都说instance_eval定义出来的是类方法，可是这名字又是instance，总觉得别扭
  在知晓在ruby中我们通常所说的class其实也是对象，而且是ruby库中Class类的实例，之后就很好理解了。

```ruby

#/usr/bin/env ruby

Class.instance_eval do
  def hi
    puts "hi #{self.class}"
  end
end

class P1
end
P1.instance_eval do
  def hi
    puts "hi #{self.class}"
  end
end

class P2
end
p2.instance_eval do
  def hi
    puts "hi #{self.class}"
  end
end

p2=P2.new
Class.hi # => hi Class (All class is instance of Class,including itself)
P1.hi    # => hi Class  (P1 is a instance of Class)
p2.hi    # => hi P2     ( p2 is a instance of P2 )
```
