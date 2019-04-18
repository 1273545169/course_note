### KMP

KMP可用于解决字符串匹配问题

参考：http://www.cnblogs.com/tangzhengyue/p/4315393.html

#### 匹配策略

str1和str2进行匹配，先找到str1和str2的公共部分，继续查找发现D和' '不匹配

![](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/kmp1.PNG)

找到D的最长前缀；查看最长前缀后面的字符与空格是否匹配，此时依然不匹配

![](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/kmp2.PNG)

再找到C的最长前缀，发现为0，则查看str2的首字符与空格是否匹配，依旧不匹配

![](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/kmp3.PNG)

继续比较空格后面的字符与str2

![](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/kmp4.PNG)

#### next数组

k=next[j]

![](https://github.com/1273545169/course_note/blob/master/%E5%9B%BE%E7%89%87/kmp5.PNG)

要求j+1位置的next，比较$str[j]$与$str[j]$的最长前缀后的字符即$str[next[j]]$是否相等

若相等，$next[j+1]=next[j]+1$;

若不等，再计较$str[j]$与$str[next[k]]（k=next[j]）$；

直到当前位置的next<=0结束，此时$next[j+1]=0$

可以以：$abaabak$ 和 $ababcababtk$为例，自己推导

#### 实现

```python

class KMP:
    # 计算字符串中每个字符的前缀和后缀的最大匹配长度，存放在next中
    def getNext(self, str):
        if len(str) == 1:
            return [-1]
        next = [None] * len(str)
        # cn初始值为next[i-1]；i当前位置
        # 当str[i - 1] ！= str[cn]，cn更新为：next[cn]
        next[0], next[1], cn, i = -1, 0, 0, 2
        while i < len(str):
            if str[i - 1] == str[cn]:
                next[i] = cn + 1
                i += 1
                cn += 1
            elif cn > 0:
                cn = next[cn]
            else:
                next[i] = 0
                i += 1
        return next

    # 判断str2是否为str1的子串，并返回起始索引，相当于str1.index(str2)
    def getIndexOf(self, str1, str2):
        if not str1 or not str2 or len(str1) < len(str2):
            return -1

        next = self.getNext(str2)
        p1, p2 = 0, 0
        while p1 < len(str1) and p2 < len(str2):
            # 找到公共部分时，p1和p2都增加
            if str1[p1] == str2[p2]:
                p1 += 1
                p2 += 1
            # p1更新，p2指向str2的首字符时，说明p1前面找不到与str2可匹配的，p1+1
            elif next[p2] == -1:
                p1 += 1
            # p2更新，p2指向当前字符最长前缀后的那个字符
            else:
                p2 = next[p2]
        return p1 - p2 if p2 == len(str2) else -1


```
