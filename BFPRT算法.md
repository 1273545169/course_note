### BFPRT

partition算法的进阶，不同之处在于划分值pivot的取法不同。

partition通常取最左边的数或者随机在数组中选取

BFPRT取pivot的步骤如下：

 分组（一般每5个一组）-> 组内排序(时间复杂度为O(N)) -> 求每组中位数组成一个新的数组，长度为N/5 —> 求新数组的中位数，以此作为pivot
 

新数组中共有N/5个数，其中有N/10个数比其中位数median大，这N/10个数所在组中又有两个数比median大，所以总共有$\frac{3}{10}$
 
 ### 实现
 
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

            # 只需要增加以下部分即可
            if R == len(ss):
                res = []
                pre = ss[:C - pArray[C]+1]
                for char in pre:
                    if char != "#":
                        res.append(char)
                return ''.join(res)[::-1]


print(Manacher().maxLcpsLength('11131'))

 
 ```
