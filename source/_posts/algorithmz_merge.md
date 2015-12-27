---
title: algorithm_merge
layout: post
date: 2015-12-27 23:34:09
categories: ['algorithm']
tags: ['merge sort']
---
ref [归并排序](https://zh.wikipedia.org/wiki/%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F)
归并排序（英语：Merge sort，或mergesort），是创建在归并操作上的一种有效的排序算法，效率为O(n log n)。1945年由约翰·冯·诺伊曼首次提出。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用，且各层分治递归可以同时进行。





```ruby
def merge(left, right)
  final = []
  until left.empty? or right.empty?
    final << (left.first < right.first ? left.shift : right.shift)
  end
  final + left + right
end

def merge_sort(array)
  return array if array.size < 2
  pivot = array.size / 2
  merge(merge_sort(array[0...pivot]), merge_sort(array[pivot..-1]))
end
```
