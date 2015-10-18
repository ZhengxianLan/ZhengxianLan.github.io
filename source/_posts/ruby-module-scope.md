---
title: ruby-module-scope
date: 2015-10-04 20:42:09
categories: ['ruby']
tags: ['module','scope']
---
 当方法自定义 module 中有 class 与系统自带 class 重名
 则在使用标准库时需要在前面加上 ::
```ruby
module M1
  class File

  end
  class T
    def exists? path
      File.exists? path
    end
  end
end


module M2
  class File

  end
  class T
    def exists? path
      ::File.exists? path
    end
  end
end

f='/etc/hosts'
t1=M1::T.new
t2=M2::T.new
begin
  puts t1.exists? f
rescue Exception => e
  puts e
end
puts t2.exists? f

# undefined method `exists?' for M1::File:Class
# true
```
