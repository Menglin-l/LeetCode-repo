#### 513. Find Bottom Left Tree Value - 复习DFS

感想：

1. 第一反应就是层序遍历，从右到左往queue里放，最后一个出来的自然就是最左下的节点。
2. DFS的方法我不会，学习了随想录文档里的方法，就是将问题转化为求最大深度那一层的从左往右第一个节点，所以用一个全局变量更新最大深度，然后保证按照先左子树后右子树的顺序遍历就行了。

---
```java
// BFS
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        if (root == null) return 0;

        Queue<TreeNode> queue = new LinkedList<>();
        TreeNode cur = root;
        queue.offer(cur);
        while (!queue.isEmpty()) {
            cur = queue.poll();
            if (cur.right != null) queue.offer(cur.right);
            if (cur.left != null) queue.offer(cur.left);
        }

        return cur.val;
    }
}
```
T:O(N) S:O(N)

```java
// DFS
class Solution {
    int maxDepth = -1;
    int value = 0;
    public int findBottomLeftValue(TreeNode root) {
        if (root == null) return 0;

        value = root.val;
        findTheValue(root, 0);

        return value;
    }

    private void findTheValue(TreeNode node, int depth) {
        if (node == null) return;
        depth ++;
        if (node.left == null && node.right == null) {
            if (depth > maxDepth) {
                maxDepth = depth;
                value = node.val;
            }
        }

        if (node.left != null) findTheValue(node.left, depth);
        if (node.right != null) findTheValue(node.right, depth);
    }
}
```
T:O(N) S:O(N)

#### 112. Path Sum

感想：

1. 找到了做路径和的套路，在前序遍历的位置上设置好操作的步骤和终止的条件就行了。

---
```java
class Solution {
    boolean findIt = false;
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        findPathSum(root, targetSum);

        return findIt;
    }

    private void findPathSum(TreeNode node, int targetSum) {
        if (node == null) return;
        targetSum -= node.val;
        if (node.left == null && node.right == null && targetSum == 0) {
            findIt = true;
        }

        findPathSum(node.left, targetSum);
        findPathSum(node.right, targetSum);
    }
}
```
T:O(N) S:O(N)

#### 113. Path Sum II

感想：

1. 遍历的时候同时筛选，把所有满足条件的节点放入list里。

---
```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        if (root == null) return res;

        findPaths(root, res, path, targetSum);
        return res;
    }

    private void findPaths(TreeNode root, List<List<Integer>> res, List<Integer> path, int targetSum) {
        if (root == null) return;

        path.add(root.val);
        targetSum -= root.val;

        if (root.left == null && root.right == null && targetSum == 0) {
            res.add(new ArrayList<>(path));
        }

        findPaths(root.left, res, path, targetSum);
        findPaths(root.right, res, path, targetSum);
        path.remove(path.size() - 1);
    }
}
```
T:O(N^2), where N are the number of nodes in a tree. In the worst case, we could have a complete binary tree and if that is the case, then there would be N/2 leafs. For every leaf, we perform a potential O(N) operation of copying over the pathNodes nodes to a new list to be added to the final pathsList. Hence, the complexity in the worst case could be O(N^2)
S:O(N)

#### 105. Construct Binary Tree from Preorder and Inorder Traversal - 需要复习

感想：

1. 不是第一遍做这个题了，确实比第一次有更深的理解。前序遍历里，可以确定第一个节点一定是根节点，然后在中序遍历里找到根节点的位置，从而确定左子树的节点数和右子树的节点数。
2. 以上过程可以重复执行，直至遍历完每一个节点。我选择用递归来做，只要确定好退出条件和单独一次的执行逻辑，剩下的每一次都一样。
3. 这个题困难的地方在于确定每一次递归时，遍历左右子树的范围。这里可以用个map来记录中序遍历数组里的节点和对应索引。

---
```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {

        for (int i = 0; i < inorder.length; i ++) {
            map.put(inorder[i], i);
        }
        return findAllNodes(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode findAllNodes(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        if (preStart > preEnd) return null;

        int rootVal = preorder[preStart];
        int leftLength = map.get(rootVal) - inStart;
        TreeNode root = new TreeNode(rootVal);

        root.left = findAllNodes(preorder, preStart + 1, preStart + leftLength, inorder, inStart, map.get(rootVal) - 1);
        root.right = findAllNodes(preorder, preStart + leftLength + 1, preEnd, inorder, map.get(rootVal) + 1, inEnd);

        return root;
    }
}
```
T:O(N) S:O(N)

#### 106. Construct Binary Tree from Inorder and Postorder Traversal

感想：

1. 理解了上一题后，这题就非常简单了。

---
```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {

        for (int i = 0; i < inorder.length; i ++) {
            map.put(inorder[i], i);
        }
        return createByNodes(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }

    private TreeNode createByNodes(int[] inorder, int inStart, int inEnd, int[] postorder, int postStart, int postEnd) {
        if (postStart > postEnd) return null;

        int rootVal = postorder[postEnd];
        int rootIndex = map.get(rootVal); // get the root index in the inorder array
        int leftLength = rootIndex - inStart;

        TreeNode root = new TreeNode(rootVal);

        root.left = createByNodes(inorder, inStart, rootIndex - inStart - 1, postorder, postStart, postStart + leftLength - 1);
        root.right = createByNodes(inorder, rootIndex + 1, inEnd, postorder, postStart + leftLength, postEnd - 1);

        return root;
    }
}
```
T:O(N) S:O(N)