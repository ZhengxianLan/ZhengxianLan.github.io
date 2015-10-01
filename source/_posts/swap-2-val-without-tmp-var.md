---
title: swap 2 val without tmp var
date: 2015-10-01 23:47:00
categories: ['miscellaneous','interview']
tags: swap
---

  Sometimes,you may be asked some awful questions which you may never use.Say,how to swap 2 values without new temporary variable.

```ruby
#!/usr/bin/env ruby

a,b=3,4

def swap_with_sum a,b
puts "a,b = # {a},#{b}"
a = a + b   # new_a is sum of a + b
b = a - b   # new_b = sum - b     = origin_a
a = a - b   # new_a = sum - new_b = sum - a  = origin_b
puts "Using sum: a,b=#{a},#{b}"
end

def swap_with_minus a,b
puts "a,b = #{a},#{b}"
a = a - b   # new_a is delta of a - b
b = b + a   # new_b = b + delta        = origin_a
a = b - a   # new_a = origin_a - delta = origin_b
puts "Using minus: a,b=#{a},#{b}"
end

swap_with_sum a,b
swap_with_minus a,b
```
