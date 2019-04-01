### BFPRT

BFPRT常用于求解top-k问题

partition算法的进阶，不同之处在于划分值pivot的取法不同。

partition通常取最左边的数或者随机在数组中选取

BFPRT取pivot的步骤如下：

 分组（一般每5个一组）-> 组内排序(时间复杂度为O(N)) -> 求每组中位数组成一个新的数组，长度为N/5 —> 求新数组的中位数，以此作为pivot
 

新数组中共有N/5个数，其中有N/10个数比其中位数median大，这N/10个数所在组中又有两个数比median大，

所以总共至少有$\frac{3}{10} * N $比median大
 
 ### 实现
 
 ```python
 
 import math

class BFPRT:
    def getMinK(self, array, k):
        if not array or k <= 0 or k > len(array):
            return
        return self.bfprt(array, 0, len(array) - 1, k - 1)

    def bfprt(self, array, left, right, i):
        if left == right:
            return array[left]
        pivot = self.medianOfMedians(array, left, right)
        index = self.partition(array, left, right, pivot)
        if i >= index[0] and i <= index[1]:
            return array[i]
        elif i < index[0]:
            return self.bfprt(array, left, index[0] - 1, i)
        else:
            return self.bfprt(array, index[1] + 1, right, i)

    def partition(self, array, left, right, pivot):

        mid = left
        left -= 1
        right += 1

        while mid < right:
            if array[mid] > pivot:
                right -= 1
                array[mid], array[right] = array[right], array[mid]
            elif array[mid] < pivot:
                left += 1
                array[left], array[mid] = array[mid], array[left]
                mid += 1
            else:
                mid += 1
        return left, right

    def InsertSort(self, array, left, right):
        for i in range(left + 1, right + 1):
            for j in range(i, left, -1):
                if array[j] < array[j - 1]:
                    array[j], array[j - 1] = array[j - 1], array[j]
                else:
                    break
        return array

    def getMedian(self, array, left, right):
        self.InsertSort(array, left, right)
        sum = left + right
        mid = sum // 2 + sum % 2
        return array[mid]

    def medianOfMedians(self, array, left, right):
        num = right - left + 1
        mArray = [0] * int((math.ceil(num / 5)))
        for i in range(len(mArray)):
            mArray[i] = self.getMedian(array, i * 5, min((i + 1) * 5 - 1, right))
        return self.bfprt(mArray, 0, len(mArray) - 1, len(array) // 2)

 
 ```
