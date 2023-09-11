#### 654. Maximum Binary Tree

感想：

1. 将问题拆分成找最大值和创建节点就行了

---
```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return createBinaryTree(nums, 0, nums.length - 1);
    }

    private TreeNode createBinaryTree(int[] nums, int left, int right) {
        if (left > right) return null;

        int rootIndex = findMax(nums, left, right);
        TreeNode root = new TreeNode(nums[rootIndex]);

        root.left = createBinaryTree(nums, left, rootIndex - 1);
        root.right = createBinaryTree(nums, rootIndex + 1, right);

        return root;
    }

    private int findMax(int[] nums, int left, int right) {
        int max = Integer.MIN_VALUE;
        int index = 0;
        for (int i = left; i <= right; i ++) {
            if (nums[i] > max) {
                max = nums[i];
                index = i;
            }
        }
        return index;
    }
}
```
T:O(N^2) S:O(N)

#### 617. Merge Two Binary Trees

感想：

1. 两棵树都要遍历一次，最后返回其中一棵就行了，把另一棵的节点值加过来，如果有不一样的节点，返回对方的就行。

---
```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;

        root1.val += root2.val;

        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);

        return root1;
    }
}
```
T:O(N) S:O(N)

#### 700. Search in a Binary Search Tree - 复习

感想：

1. 我一开始用遍历去做这个题，但其实题目里已经写明了是一个bst树，应该用bst的特性去做。
2. 但我比较迷糊的一点是递归调用时，到底该不该加return。[随想录的文档](https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html#%E6%80%9D%E8%B7%AF:~:text=1-,%E7%A1%AE%E5%AE%9A%E5%8D%95%E5%B1%82%E9%80%92%E5%BD%92%E7%9A%84%E9%80%BB%E8%BE%91,-%E7%9C%8B%E7%9C%8B%E4%BA%8C%E5%8F%89)里有总结，递归函数是有返回值的，如果不用一个变量接住，那返回值就会丢失。所以本题中一旦搜到了，就立刻返回，不需要额外的变量了。

---
```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) return root;

        if (root.val > val) return searchBST(root.left, val);
        else return searchBST(root.right, val);
    }
}
```
T:O(logN) S:O(N)

#### 98. Validate Binary Search Tree

感想：

1. 这题和上一题逻辑上很像，但是如果完全采用上一题的方法，就会做错下面这个例子：</br>
<pre>
     5
    / \ 
   4   6 
      / \ 
     3   7
</pre>
2. 所以必须把最大值和最小值作为参数传入递归调用，对每一个节点进行判断。

---
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValid(root, null, null);
    }

    private boolean isValid(TreeNode root, TreeNode max, TreeNode min) {
        if (root == null) return true;

        if (max != null && root.val >= max.val) return false;
        if (min != null && root.val <= min.val) return false;

        return isValid(root.left, root, min) && isValid(root.right, max, root);
    }
}
```
T:O(N) S:O(N)