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

##### 中序遍历

```python

class Morris:
    def MorrisIn(self, head):
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

            print(cur.value)
            cur = cur.right

```

##### 先序遍历

```python

class Morris:
    def MorrisPre(self, head):
        cur = head
        while cur:
            mostRight = cur.left
            if mostRight:
                while mostRight.right and mostRight.right != cur:
                    mostRight = mostRight.right

                if not mostRight.right:
                    print(cur.value)
                    mostRight.right = cur
                    cur = cur.left
                    continue
                else:
                    mostRight.right = None
            else:
                print(cur.value)

            cur = cur.right

```


##### 后序遍历

```python


```
