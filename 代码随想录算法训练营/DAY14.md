
#### 144. Binary Tree Preorder Traversal - 复习统一迭代法

感想：

1. 递归方法中，前中后序的区别就是处理节点顺序的不同。基本操作。
2. 迭代方法中，前序遍历需要调整入栈顺序为根->右->左，这样在处理节点时就能按照根->左->右来操作。
3. 学习了代码随想录的统一迭代法，对我是一种新的方法，用一个空节点来标记每一层的根节点。我的操作还不熟练，有空再dry run理解。

---
```java
// recursive approach
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) return new ArrayList<>();

        List<Integer> res = new ArrayList<>();
        preorderTraverse(root, res);
        return res;
    }

    private void preorderTraverse(TreeNode root, List<Integer> res) {
        if (root == null) return;

        res.add(root.val);
        preorderTraverse(root.left, res);
        preorderTraverse(root.right, res);
    }
}
```
T:O(N) S:O(N)

```java
// iterative approach
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();

            if (cur.right != null) stack.push(cur.right);
            if (cur.left != null) stack.push(cur.left);

            res.add(cur.val);
        }

        return res;
    }
}
```
T:O(N) S:O(N)

#### 94. Binary Tree Inorder Traversal - 需要复习迭代法和统一迭代法

1. 中序遍历的迭代方法也是在用自己创建的栈来模拟递归调用，先把所有左子树上的节点入栈，一直到最左节点。然后弹出，处理该节点，然后对它的右节点重复以上操作。
2. 这题我做的不太好，因为用了好几个辅助节点把自己绕昏了，其实只用一个cur就可以。

---
```java
// recursive approach
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) return new ArrayList<>();

        List<Integer> res = new ArrayList<>();
        inorderTraverse(root, res);
        return res;
    }

    private void inorderTraverse(TreeNode root, List<Integer> res) {
        if (root == null) return;

        inorderTraverse(root.left, res);
        res.add(root.val);
        inorderTraverse(root.right, res);
    }
}
```
T:O(N) S:O(N)

```java
// iterative approach
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            res.add(cur.val);
            cur = cur.right;
        }
        return res;
    }
}
```
T:O(N) S:O(N)
#### 145. Binary Tree Postorder Traversal - 需要复习迭代法和统一迭代法

1. 后续遍历的迭代我没有用最后翻转结果的那种方法。首先还是把所有左节点入栈，然后pop出最左节点开始处理，然后在它的右子树重复同样的操作。
2. 这个方法我也没有做好，需要判断一下当前节点是不是stack.peek()的左孩子，如果是，说明stack.peek()的左边已经全部访问结束，开始处理右边。
---
```java
// recursive approach
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) return new ArrayList<>();

        List<Integer> res = new ArrayList<>();
        postorderTraverse(root, res);
        return res;
    }

    private void postorderTraverse(TreeNode root, List<Integer> res) {
        if (root == null) return;

        postorderTraverse(root.left, res);
        postorderTraverse(root.right, res);
        res.add(root.val);
    }
}
```
T:O(N) S:O(N)
```java
// iterative approach
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()) {
            // find left nodes
            while (cur != null) {
                stack.push(cur);
                if (cur.left != null) {
                    cur = cur.left;
                } else {
                    cur = cur.right;
                }
            }

            TreeNode node = stack.pop();
            res.add(node.val);

            // check if current node is the left child of the one in the stack, 
            // if yes, that mean the current node and his child all visited and the next to be visited going to be his sibling 
            // (so next root is get the first node from stack)
            if (!stack.isEmpty() && stack.peek().left == node) {
                cur = stack.peek().right;
            }
        }
        return res;
    }
}
```
T:O(N) S:O(N)

#### 统一迭代法：
```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.peek();
            if (cur != null) {
                stack.pop();
                // postorder traversal
                stack.push(cur);
                stack.push(null);
                if (cur.right != null) stack.push(cur.right);
                // inorder traversal
                // stack.push(cur);
                // stack.push(null);
                if (cur.left != null) stack.push(cur.left);
                // preorder traversal
                // stack.push(cur);
                // stack.push(null);
            } else {
                stack.pop();
                cur = stack.pop();
                res.add(cur.val);
            }
        }
        return res;
    }
}
```