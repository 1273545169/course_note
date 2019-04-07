### 题目描述

给定一个数组arr， 值可正， 可负， 可0； 一个整数aim， 求累加和小于等于aim的， 最长子数组， 

要求时间复杂度O(N)

### 思路解析

参考：https://www.wmathor.com/index.php/archives/975/

两个辅助数组 sum 和 ends，sum[i]表示的是以 arr[i]开头 (必须包含 arr[i]) 的所有子数组的最小累加和，

对应的 ends[i]表示的是取得这个最小累加和的右边界。 一开始先求出 sums 数组和 ends数组。

![来自上面的参考](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/%E5%AD%90%E6%95%B0%E7%BB%84%E7%B4%AF%E5%8A%A0%E5%92%8C%E5%B0%8F%E4%BA%8E%E7%AD%89%E4%BA%8Eaim.png)

这个题目最精华的是左右边界不回退，

就是说，如果从 0 位置扩到 T 区间，T+1 区间不能扩了，此时不是回到 1 位置开始扩，而是舍弃 0 位置，看能不能由于舍弃 0 位置把 T+1 位置加进来：

![来自上面的参考](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/%E5%AD%90%E6%95%B0%E7%BB%84%E7%B4%AF%E5%8A%A0%E5%92%8C%E5%B0%8F%E4%BA%8E%E7%AD%89%E4%BA%8Eaim1.png)


```python
class Solution:
    def getMaxSubLength(self, array, aim):
        length = len(array)
        sums = [0] * (length - 1) + [array[-1]]
        ends = [0] * (length - 1) + [length - 1]

        for i in range(length - 2, -1, -1):
            if sums[i + 1] < 0:
                sums[i] = array[i] + sums[i + 1]
                ends[i] = ends[i + 1]
            else:
                sums[i] = array[i]
                ends[i] = i

        right, curSum, res = 0, 0, 0
        for left in range(length):
            # 一块一块向右扩
            while right < length and curSum + sums[right] <= aim:
                curSum += sums[right]
                right = ends[right] + 1
            # 将curSum对应区间的最左边的数移除，窗口内数减一
            curSum -= array[left] if right > left else 0
            res = max(res, right - left)
            # 以[100,200,7,-6,-3],aim=7来理解
            # 当right=left，可以让right+1
            right = max(right, left + 1)

        return res

```
