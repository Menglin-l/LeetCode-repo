#### 102. Binary Tree Level Order Traversal - 复习一下DFS做法

感想：

1. 用一个queue去维护每一层添加的子节点。基本操作。
2. 学习了[代码随想录](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE:~:text=102.-,%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86,-%E5%8A%9B%E6%89%A3%E9%A2%98%E7%9B%AE)上的DFS做法，在那基础上我自己改了一下，和图的DFS真是很相似。

---
```java
// BFS
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            List<Integer> path = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i ++) {
                TreeNode cur = queue.poll();
                path.add(cur.val);
                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
            res.add(path);
        }
        return res;
    }
}
```
T:O(N) S:O(N)

```java
// DFS
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        traverseTree(res, root, 0);

        return res;
    }

    private void traverseTree(List<List<Integer>> res, TreeNode node, int level) {
        if (node == null) return;

        if (res.size() == level) {
            res.add(new ArrayList<>());
        }

        res.get(level).add(node.val);

        traverseTree(res, node.left, level + 1);
        traverseTree(res, node.right, level + 1);
    }
}
```
T:O(N) S:O(N)

#### 107. Binary Tree Level Order Traversal II - 复习一下DFS做法

感想：

1. 这题继续练习DFS做法。思路和上一题很像，要注意的是放入结果list里的顺序，和取值的顺序。

---
```java
// DFS
class Solution {

    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;
        traverseTree(res, root, 0);

        return res;
    }

    private void traverseTree(List<List<Integer>> res, TreeNode node, int level) {
        if (node == null) return;

        if (res.size() == level) {
            res.add(0, new ArrayList<>());
        }

        res.get(res.size() - 1 - level).add(node.val);

        traverseTree(res, node.left, level + 1);
        traverseTree(res, node.right, level + 1);
    }
}
```
T:O(N) S:O(N)

#### 199. Binary Tree Right Side View

感想：

1. 层序遍历，res list里取每一层的最后一个元素，加个if条件筛一下就行了，简单粗暴。

---
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i ++) {
                TreeNode cur = queue.poll();
                if (i == size - 1) res.add(cur.val);

                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
        }

        return res;
    }
}
```
T:O(N) S:O(N)

#### 637. Average of Levels in Binary Tree

感想：

1. 层序遍历，遍历到每一层的最后一个元素时，求一下平均数就行了。

---
```java
class Solution {
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            double sum = 0.0;
            for (int i = 0; i < size; i ++) {
                TreeNode cur = queue.poll();
                sum += (double) cur.val;
                if (i == size - 1) {
                    res.add(sum / size);
                }

                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
        }

        return res;
    }
}
```
T:O(N) S:O(N)

#### 429. N-ary Tree Level Order Traversal

感想：

1. 也是一个标准的层序遍历，简单粗暴的套路。

---
```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) return res;

        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> path = new ArrayList<>();
            for (int i = 0; i < size; i ++) {
                Node cur = queue.poll();
                path.add(cur.val);

                for (int j = 0; j < cur.children.size(); j ++) {
                    queue.offer(cur.children.get(j));
                }
            }
            res.add(path);
        }

        return res;
    }
}
```
T:O(N), N is the number of nodes 
S:O(N)

#### 515. Find Largest Value in Each Tree Row

感想：

1. 层序遍历，每一层求最大值就行。

---
```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) return res;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            int max = Integer.MIN_VALUE;
            for (int i = 0; i < size; i ++) {
                TreeNode cur = queue.poll();
                max = Math.max(max, cur.val);

                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
            res.add(max);
        }
        return res;
    }
}
```
T:O(N) S:O(N)

#### 116. Populating Next Right Pointers in Each Node - 需要复习

感想：

1. 虽然也是层序遍历，但是做法不像前几个题那么直接了，需要判断一下当前的节点是不是最后一个节点。这里我做的有点复杂了，我以为要特别执行一下cur.next = null;但其实什么都不用做，next就指向null。。。

---
```java
class Solution {
    public Node connect(Node root) {
        if (root == null) return root;

        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i ++) {
                Node cur = queue.poll();

                if (i < size - 1) {
                    cur.next = queue.peek();
                }

                if (cur.left != null) queue.offer(cur.left);
                if (cur.right != null) queue.offer(cur.right);
            }
        }
        return root;
    }
}
```
T:O(N) S:O(N)

#### 117. Populating Next Right Pointers in Each Node II

感想：

1. 用层序遍历，和116一模一样的代码就行。因为不管有没有空节点，每一层都在size范围里，一定可以串在一起。
2. 代码省略。

#### 104. Maximum Depth of Binary Tree

感想：

1. 这个题的解法有多种，我先写了遍历的做法，用一个全局变量max去更新最大高度，另一个全局变量depth去维护每一层的高度。
2. 第二种是递归，把问题分解成子问题，先解决一个子问题的做法。因为对于每一个子问题，要考虑的都是“用左右子树的最大高度加上自己”。

---
```java
// the first solution
class Solution {
    int depth = 0;
    int max = 0;
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        traverseTree(root);

        return max;
    }

    private void traverseTree(TreeNode node) {
        if (node == null) return;

        depth ++;
        traverseTree(node.left);
        traverseTree(node.right);

        if (node.left == null && node.right == null) max = Math.max(depth, max); // update the max value
        depth --;

    }
}
```
T:O(N) S:O(N)

```java
// the second solution
class Solution {

    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        int left = maxDepth(root.left);
        int right = maxDepth(root.right);

        return Math.max(left, right) + 1;
    }
}
```
T:O(N) S:O(logN)

#### 111. Minimum Depth of Binary Tree - 需要复习

感想：

1. 和最小深度那题一样的做法，但是我没做好，忽略了只有当左右孩子都为空时再去取最小值。因为只有这个时候，才说明走到了叶子节点，可以计算高度了。
2. 这题我是用dfs来做的。

---
```java
class Solution {
    int depth = 0;
    int min = Integer.MAX_VALUE;
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        
        traverseTree(root);
        return min;
    }

    private void traverseTree(TreeNode node) {
        if (node == null) return;
        
        depth ++;
        traverseTree(node.left);
        traverseTree(node.right);

        if (node.left == null && node.right == null) {
            min = Math.min(min, depth);
        }

        depth --;
    }
}
```
T:O(N) S:O(N)

#### 101. Symmetric Tree - 需要复习

感想：

1. 一边遍历一边比较就可以，但要注意比较的时候，是内侧节点与内侧比，外侧节点与外侧比。这点我没做好。

---
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {

        return traverseTree(root.left, root.right);
    }

    private boolean traverseTree(TreeNode left, TreeNode right) {
        if ((left == null && right != null) || (left != null && right == null)) return false;
        if (left == null && right == null) return true;
        if (left.val != right.val) return false;

        return traverseTree(left.left, right.right) && traverseTree(left.right, right.left);
    }
}
```
T:O(N) S:O(N)

#### 226. Invert Binary Tree

感想：

1. 只要头节点不为空，每一次遍历都把左右子树swap一下就行了。

---
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return root;

        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
```
T:O(N) S:O(N)