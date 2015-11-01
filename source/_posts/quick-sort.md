---
title: quick sort
date: 2015-10-18 20:21:05
categories: ['algorithm','quick sort']
tags: ['base']
---

排序总是看了忘
今天就再复习一下，加深印象
### 从两端交替向中间扫描
基本思路就是
1. 确定一个基准值
2. 然后从两边向中间查找
3. 从左边找到一个大于基准值的元素
4. 从右边找到一个小于基准值的元素
5. 交换
6. 不断重复，直至碰头
7. 将基准值居中
8. 递归处理左右子数组

```ruby
def sort(arr,min,max)
  return  if min>=max
  pivot=arr[min]
  i,j=min,max
  while(i<j) do
    j-=1 while(i<j && arr[j]>pivot)
    i+=1 while(i<j && arr[i]<pivot)
    arr[i],arr[j] = arr[j],arr[i] if i < j
  end
  arr[i]=pivot
  sort(arr,min,i-1)
  sort(arr,i+1,max)
end

arr=(1..8).to_a.shuffle
p arr.inspect
sort(arr,0,arr.length-1)
p arr.inspect
```

### 从一端扫描

```ruby
def quicksort(array, min=0, max=array.length-1)
  if min < max
    index = partition(array, min, max)
    quicksort(array, min, index-1)
    quicksort(array, index+1, max)
  end
end

def partition(array, left, right)
  mid = (left + right)/2
  pivot = array[mid]

  array.swap!(mid, right)

  less = left

  (left...right).each do |greater|
    if (array[greater] <= pivot && greater!=less)
      array.swap!(greater, less)
      less+=1
    end
  end

  array.swap!(less, right)

  return less
end

class Array
  def swap!(a,b)
    puts "a,b #{a},#{b}"
    self[a], self[b] = self[b], self[a]
    self
  end
end

test=(1..8).to_a.shuffle
p test.inspect
quicksort test
p test.inspect


```
1. 关键的地方在分区排序
  =begin
  less 用于标记小于 pivot 的右边界 +1
  greater 用于标记值比 pivot 大的数组元素下标
  一旦找到值小于 pivot 时，就进行值的互换
  因此在开始从左边移动时:
  ```
  if (less！=greater && array[greater] <= pivot)
      游标 less+=1
  end
  if array[greater] > pivot then
    less 不变，
    greater 继续向右移动
  if array[greater] <= pivot then
    将 array[greater] 往前扔, 即和 less 指向的 arr[less] 互换
    (ps: arr[less] 在每次循环后总是指向一个大于 pivot 的值，类似数据库中 resultset 中的游标 )
  ```
  =end

[ref](http://codereview.stackexchange.com/questions/43667/quicksort-implementation)
