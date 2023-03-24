### 题目描述

[146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

### 思路解析

[解题思路](https://leetcode.cn/problems/lru-cache/solution/lruhuan-cun-ji-zhi-by-leetcode-solution/)

```python

class DLinkedNode:
    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.next = None
        self.last = None


class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.head = DLinkedNode()
        self.tail = DLinkedNode()
        self.head.next = self.tail
        self.tail.last = self.head
        self.cache = {}

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        else:
            node = self.cache[key]
            self.moveToHead(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.value = value
            self.moveToHead(node)
        else:
            node = DLinkedNode(key, value)
            self.addToHead(node)
            self.cache[key] = node

            self.size += 1
            if self.size > self.capacity:
                self.size -= 1
                key = self.removeTail()
                self.cache.pop(key)

    def addToHead(self, node):
        node.next = self.head.next
        node.last = self.head
        self.head.next.last = node
        self.head.next = node

    def removeNode(self, node):
        node.last.next = node.next
        node.next.last = node.last

    def moveToHead(self, node):
        self.removeNode(node)
        self.addToHead(node)

    def removeTail(self):
        node = self.tail.last
        self.removeNode(node)
        return node.key

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)

```
