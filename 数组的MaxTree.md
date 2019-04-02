### 题目描述

一个数组的MaxTree定义：

1、数组必须没有重复元素

2、MaxTree是一棵二叉树，数组的每一个值对应一个二叉树节点

3、包括MaxTree树在内且在其中的每一棵子树上，值最大的节点都是树的头

给定一个没有重复元素的数组arr，写出生成这个数组的MaxTree的函数，要求如果数组长度为N，则时间负责度为O(N)、额外空间负责度为O(N)。


### 思路解析

#### 构建成大根堆

```python

class Solution:
    def getMaxTree(self, array):
        for i in range(len(array)):
            self.heapInsert(array, i)
        return array

    def heapInsert(self, array, index):
        while index > 0 and array[index] > array[(index - 1) // 2]:
            array[index], array[(index - 1) // 2] = array[(index - 1) // 2], array[index]
            index = (index - 1) // 2


```

#### 单调栈

