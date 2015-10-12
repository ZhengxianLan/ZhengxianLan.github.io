---
title: ruby-meta-programing
date: 2015-10-03 21:33:56
categories: ['ruby']
tags: 'meta-programing'
---

1. 实例变量存在对象中，实例方法存在类中。 在对象调用实例方法之前，其实例变量是没有的。
  面向对象，就是将行为和数据用类来组织
    - 行为是一致的，因此行为保存在类中。
    - 而数据，或者说状态,却是独立的，因此保存在对象中。
2. 类也是对象，类只是 Class 的一个实例。Class.class # => Class
3. 开放类很强大，但是又容易受到污染造成 Monky-patch
4. 在类内的self为类本身，而类内的实例方法里面，self表示对象的引用。
4. 继承关系
```ruby
irb(main):011:0> "".class.ancestors
=> [String, Comparable, Object, Kernel, BasicObject]
irb(main):012:0> Class.ancestors
=> [Class, Module, Object, Kernel, BasicObject]
irb(main):014:0> Class.instance_methods false
=> [:allocate, :new, :superclass]  #如此看来，Class 只是多了 3 个实例方法的 Module

#!/usr/bin/env ruby
def genealogy clazz
  out=clazz.to_s
  out<<"-->#{clazz}" while (clazz=clazz.superclass)!=nil
  puts out
end
genealogy String
genealogy Class

#=> String --> Object --> BasicObject
#=> Class  --> Module --> Object      --> BasicObject
```

5. class 名只是个常量，保存一个 Class 实例的引用。
	比如 String 就是一个常量，它保存了 Class 的一个实例，即 String 类。
6. 常量: 任何以大写字母开头的，包括 class 名和 module 名，都是常量。
	通常，我们通过 Module 来包裹并组织常量，即作为常量的 namespace
7. java & c# 中的类事实上也是一个名为 Class 类的实例。C# 甚至允许像 ruby open class 那样向已有类添加方法。
	但是，java，c# 中的类对象受到比普通的实例对象受到更多的限制。不能在运行时创建新类，或者改变类的方法。
	这种受限制的类更像是类的描述而非真正的类。
	而 ruby 中的类就更加灵活，不仅允许读取类相关信息，甚至还允许在运行时对类信息进行修改。
8. 所谓 module 就是一堆实例方法的集中营。而类，只不过多了继承和实例化的功能。
	他们如此接近，但为了保持代码的意图清晰而进行合理分工。

