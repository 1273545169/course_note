### Morris遍历

利用Morris遍历实现二叉树的先序， 中序， 后续遍历， 时间复杂度O(N)， 额外空间复杂度O(1)。

### 流程

当前节点记为cur

1、若cur`无左`孩子，cur向`右`移动，cur=cur.right

2、若cur`有左`孩子，找到cur左子树上的最右节点`mostRight`

  - 若mostRight.right指向`空`，让其指向`cur`,cur向`左`移动
  
  - 若mostRight.right指向`cur`，让其指向`空`，cur向`右`移动
  
 一个节点有`左`子树可以来到这个节点`2`次，以`mostRight`来判断是第几次来到此节点
  
 ### 实现
 
 ```python
 
 class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
 
 ``` 
 ##### 先序遍历
 
 第一次来到一个节点时打印
 
 节点没有左子树，直接打印
 
 节点有左子树，`mostRight.right`为空，即第一次来到此节点时打印

```python

class Morris:
    def MorrisPre(self, head):
        cur = head
        while cur:
            mostRight = cur.left
            # 当前节点有左子树
            if mostRight:
                while mostRight.right and mostRight.right != cur:
                    mostRight = mostRight.right

                if not mostRight.right:
                    # 当前节点有左子树，在第一次来到此节点时打印
                    print(cur.value)
                    mostRight.right = cur
                    cur = cur.left
                    continue
                else:
                    mostRight.right = None
            # 当前节点无左子树，直接打印当前节点
            else:
                print(cur.value)

            cur = cur.right

```

##### 中序遍历

第二次来到此节点时打印此节点，而此时cur准备右移

节点没有左子树时打印此节点，此时cur也准备右移

总之，节点准备右移时打印 

```python

class Morris:
    def MorrisIn(self, head):
        cur = head
        while cur:
            mostRight = cur.left
            # 当前节点有左子树
            if mostRight:
                while mostRight.right and mostRight.right != cur:
                    mostRight = mostRight.right

                if not mostRight.right:
                    mostRight.right = cur
                    cur = cur.left
                    continue
                else:
                    mostRight.right = None
            # 节点准备右移时打印，此时节点左子树已打印完
            print(cur.value)
            cur = cur.right

```

##### 后序遍历

只考虑第二次来到的节点，即有左子树的节点

在第二次来到某节点时，将此节点的左子树的右边界逆序打印

在打印完毕后，再将整棵树的右边界逆序打印

```python

class Morris:
    def MorrisPos(self, head):
        cur = head
        while cur:
            mostRight = cur.left
            if mostRight:
                while mostRight.right and mostRight.right != cur:
                    mostRight = mostRight.right

                if not mostRight.right:
                    mostRight.right = cur
                    cur = cur.left
                    continue
                else:
                    mostRight.right = None
                    # 第二次来到某节点时，逆序打印其左子树的右边界
                    self.PrintEdge(cur.left)

            cur = cur.right
        # 退出时单独逆序打印整棵树的右边界
        self.PrintEdge(head)
    
    # 打印节点
    def PrintEdge(self, node):
        temp = tail = self.reverseEdge(node)
        while tail:
            print(tail.value)
            tail = tail.right
        # 打印结束将树恢复
        self.reverseEdge(temp)
    
    # 逆序
    def reverseEdge(self, node):
        pre = None
        while node:
            temp = node.right
            node.right = pre
            pre = node
            node = temp
        return pre

```
