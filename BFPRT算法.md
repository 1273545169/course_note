### BFPRT

BFPRT常用于求解top-k问题

partition算法的进阶，不同之处在于划分值pivot的取法不同。

partition通常取最左边的数或者随机在数组中选取

BFPRT取pivot的步骤如下：

 分组（一般每5个一组）-> 组内排序(时间复杂度为O(N)) -> 求每组中位数组成一个新的数组，长度为N/5 —> 求新数组的中位数，以此作为pivot
 

新数组中共有N/5个数，其中有N/10个数比其中位数median大，这N/10个数所在组中又有两个数比median大，所以总共至少有$\frac{3}{10} * N $比median大
 
 ### 实现
 
 ```python
 


 
 ```
