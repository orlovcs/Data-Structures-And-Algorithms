
A tree is a hierarchical data structure consisting of nodes, where each node has a value and zero or more child nodes, with a single root node from which all other nodes descend. Can also be called a connected graph without cycles.

## Implementation

```
class TreeNode:
    def __init__(self, value=0, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

```

## Algorithms

```
def in_order_traversal(root):
    if root:
        in_order_traversal(root.left)
        print(root.value, end=' ')
        in_order_traversal(root.right)
```

```
def pre_order_traversal(root):
    if root:
        print(root.value, end=' ')
        pre_order_traversal(root.left)
        pre_order_traversal(root.right)
```

```
def post_order_traversal(root):
    if root:
        pre_order_traversal(root.left)
        pre_order_traversal(root.right)
        print(root.value, end=' ')
```