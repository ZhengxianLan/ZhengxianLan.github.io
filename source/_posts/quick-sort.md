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
    index = reorder_partition(array, min, max)
    quicksort(array, min, index-1)
    quicksort(array, index+1, max)
  end
end

def reorder_partition(array, left_index, right_index)
  middle_index = (left_index + right_index)/2
  pivot_value = array[middle_index]

  array.swap!(middle_index, right_index)

  less_array_pointer = left_index

  (left_index...right_index).each do |greater_array_pointer|
    if (array[greater_array_pointer] <= pivot_value && greater_array_pointer!=less_array_pointer)
      array.swap!(greater_array_pointer, less_array_pointer)
      less_array_pointer+=1
    end
  end

  array.swap!(less_array_pointer, right_index)

  return less_array_pointer
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
  less_array_pointer 用于标记小于 pivot_value 的右边界 +1
  greater_array_pointer 用于标记值比 pivot_value 大的数组元素下标
  一旦找到值小于 pivot_value 时，就进行值的互换
  因此在开始从左边移动时:
  ```
  if (less_array_pointer！=greater_array_pointer && array[greater_array_pointer] <= pivot_value)
      游标 less_array_pointer+=1
  end
  if array[greater_array_pointer] > pivot_value then
    less_array_pointer 不变，
    greater_array_pointer 继续向右移动
  if array[greater_array_pointer] <= pivot_value then
    将 array[greater_array_pointer] 往前扔, 即和 less_array_pointer 指向的 arr[less_array_pointer] 互换
    (ps: arr[less_array_pointer] 在每次循环后总是指向一个大于 pivot_value 的值，类似数据库中 resultset 中的游标 )
  ```
  =end

[ref](http://codereview.stackexchange.com/questions/43667/quicksort-implementation)
