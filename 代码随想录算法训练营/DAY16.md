#### 104. Maximum Depth of Binary Tree 和 111. Minimum Depth of Binary Tree 昨天写过了，记录在DAY15

---
#### 559. Maximum Depth of N-ary Tree

感想：

1. 这个题和求二叉树的最大深度是一个套路，用for循环遍历一下各个子树就行了。

---
```java
class Solution {
    int depth = 0;
    int maxDepth = 0;
    public int maxDepth(Node root) {
        if (root == null) return 0;

        travserseTree(root);
        return maxDepth;
    }

    private void travserseTree(Node node) {
        if (node == null) return;

        depth ++;
        for (int i = 0; i < node.children.size(); i ++) {
            travserseTree(node.children.get(i)); 
        }
        maxDepth = Math.max(maxDepth, depth);
        depth --;
    }
}
```
T:O(N) S:O(N)

#### 222. Count Complete Tree Nodes

感想：

1. 在前序遍历的位置上计数就行了。

---
```java
class Solution {
    int cnt = 0;
    public int countNodes(TreeNode root) {
        if (root == null) return 0;

        cnt ++;
        countNodes(root.left);
        countNodes(root.right);

        return cnt;
    }
}
```
T:O(N) S:O(N)