#### 110. Balanced Binary Tree - 复习

感想：

1. 一开始我把它和求最大深度弄混了，重新温习了[随想录的文档](https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE:~:text=%E6%9C%89%E5%BE%88%E5%A4%A7%E5%8C%BA%E5%88%AB%E3%80%82-,%E8%BF%99%E9%87%8C%E5%BC%BA%E8%B0%83%E4%B8%80%E6%B3%A2%E6%A6%82%E5%BF%B5%EF%BC%9A,-%E4%BA%8C%E5%8F%89%E6%A0%91)里的定义，节点的深度和高度是不一样的。
2. 所以从下往上的话，这个题要用后序遍历来做。

---
```java
class Solution {

    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;

        return heightBalanced(root) != -1;
    }

    private int heightBalanced(TreeNode node) {
        if (node == null) return 0;

        int left = heightBalanced(node.left);
        if (left == -1) return -1;

        int right = heightBalanced(node.right);
        if (right == -1) return -1;

        if (Math.abs(left - right) > 1) return -1;
        else return Math.max(left, right) + 1;
    }
}
```
T:O(N) S:O(N)

#### 257. Binary Tree Paths - 复习

感想：

1. 是我最不擅长的路径和。。。
2. 这个题用回溯来做，在前序遍历的位置上处理节点，但是不要忘记最后有一步要撤销动作。
3. 我是用StringBuilder来做的，但我对它的操作还不太熟，需要多做几道类似的题。

---
```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        if (root == null) return res;

        StringBuilder sb = new StringBuilder();

        findPaths(root, res, sb);
        return res;
    }

    private void findPaths(TreeNode node, List<String> res, StringBuilder sb) {
        if (node == null) return;

        int len = sb.length();

        if (node.left == null && node.right == null) {
            sb.append(String.valueOf(node.val));
            res.add(sb.toString());
            sb.delete(len, sb.length());
        }

        sb.append(String.valueOf(node.val)).append("->");

        findPaths(node.left, res, sb);
        findPaths(node.right, res, sb);
        sb.delete(len, sb.length());
    }
}
```
T:O(N) S:O(N)

#### 404. Sum of Left Leaves

感想：

1. 这个题我自己做对了，就是需要一个全局变量去记录所有的累加和，不然在回溯的时候，和会回到上一层的状态。

---
```java
class Solution {
    int sum = 0;
    public int sumOfLeftLeaves(TreeNode root) {
       if (root == null) return 0;
       
       traverseTree(root);
       return sum; 
    }

    private void traverseTree(TreeNode node) {
        if (node == null) return;
        if (node.left != null && node.left.left == null && node.left.right == null) sum += node.left.val;

        traverseTree(node.left);
        traverseTree(node.right);
    }
}
```
T:O(N) S:O(N)