---
title: instance_eval & class_eval
date: 2015-10-04 20:50:42
categories: ['ruby']
tags: ['meta']
---


### instance_eval

  在 ruby 中我们通常所说的类其实也是对象，而且是 ruby 库中 Class 类的实例
instance_eval 从方法名上来看就是只能通过对象来调用。
 - 若是普通的对象，那么 eval 出来的是个 singleton_method
 - 若是类调用，因为所有类都是内置 Class 的实例，因此调用的时候形式和我们常见的类方法调用一样
 就是看 eval 后面 block 的接受者是谁
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


Class.hi # => hi Class (All class is instance of the Class,including itself)
P1.hi
# => hi Class  (P1 is a instance of the Class,
# by this way,we define a class-method for class P1)
p2.hi    # => hi P2     (p2 is a instance of P2)
begin
  P2.hi
rescue Exception=>e
  puts "Oops!!! #{e}"  #=> Oops!!! undefined method `hi' for P2:Class
end

puts "p2's singleton_methods:#{p2.singleton_methods} "
# => [:hi] (so,obj.instance_eval define a method only aviable to the obj itself,
# which called singleton_method)
def p2.ho
  puts "ho #{self.class}"
end
puts "after def p2.ho,p2's singleton_methods: #{p2.singleton_methods} "
# => [:hi,:ho] (`obj.instance_eval` just like `def obj.new_method`)

```


### class_eval

  receiver 必须是一个类, 且定义出来的是个实例方法
  又由于对象只保存实例变量，而不保存方法；因此在 class_eval 之前创建的对象，也能获得使用新增的方法。
```

class D
end
d=D.new
p "D.respond_to? :ddd =>  #{D.respond_to? :ddd}" # => false
p "d.respond_to? :ddd =>  #{d.respond_to? :ddd}" # => false
D.class_eval do
  def ddd
    puts "self in ddd => #{self}"
  end
end
puts "after class_eval ddd"
p "D.respond_to? :ddd =>  #{D.respond_to? :ddd}" # => false
p "d.respond_to? :ddd =>  #{d.respond_to? :ddd}" # => true
```
