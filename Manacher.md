### 题目描述

求一个字符串的最大回文半径

### 思路解析

```python

class Manacher:
    # 131 ->  #1#3#1#
    def getManacherString(self, s):
        res = [None] * (len(s) * 2 + 1)
        for i in range(len(res)):
            res[i] = '#' if i & 1 == 0 else s[i // 2]
        return res

    # 求字符串的最长回文子串；所需变量：辅助数组pAarray，C，R
    def maxLcpsLength(self, s):
        if not s:
            return 0
        ss = self.getManacherString(s)
        # 存储每个字符的回文半径
        pArray = [0] * len(ss)
        # R表示最靠右的回文边界；C表示回文边界最靠右的那个字符；
        C, R = -1, -1
        maximum = float('-inf')
        for i in range(len(ss)):
            # case2-1 和 case2-1
            pArray[i] = min(R - i, pArray[2 * C - i]) if R > i else 1
            # case1 和 case2-3
            while i + pArray[i] < len(ss) and i - pArray[i] > -1:
                if ss[i + pArray[i]] == ss[i - pArray[i]]:
                    pArray[i] += 1
                else:
                    break
            # i的回文右边界大于R时，R和C均更新
            if i + pArray[i] > R:
                R = i + pArray[i]
                C = i

            maximum = max(maximum, pArray[i])
        return maximum - 1



```
