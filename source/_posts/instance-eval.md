---
title: instance_eval
date: 2015-10-04 20:50:42
categories: ['ruby']
tags: ['meta']
---

1. 初次看到 instance_eval 时是在论坛上看到，同时看到的还有 class_eval。都说 instance_eval 定义出来的是类方法，可是这名字又是 instance，总觉得别扭
  在知晓在 ruby 中我们通常所说的 class 其实也是对象，而且是 ruby 库中 Class 类的实例，之后就很好理解了。

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

p2=P2.new
p2.instance_eval do
  def hi
    puts "hi #{self.class}"
  end
end


Class.hi # => hi Class (All class is instance of Class,including itself)
P1.hi    # => hi Class  (P1 is a instance of Class)
p2.hi    # => hi P2     (p2 is a instance of P2)
begin
  P2.hi
rescue Exception=>e
  puts "Oops!!! #{e}"  #=> Oops!!! undefined method `hi' for P2:Class  
end                    #=> (instance_eval 定义的方法仅对调用它的 obj 可见，而且实际上是绑定到了该对象的 shadow-class 上了)


```
