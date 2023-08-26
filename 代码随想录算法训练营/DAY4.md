#### 24. Swap Nodes in Pairs - Medium - 比较简单

感想：

1. 第一次尝试用了两个辅助指针，发现不好做，于是改用三个辅助指针
2. 还是要画图
3. 注意对空指针的判断
4. 所有可能引起头指针变动的题都最好用个dummy node

---
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode dummy = new ListNode(-1, head);
        ListNode cur = dummy.next;
        ListNode pre = dummy;
        ListNode pos = cur.next.next;

        while (cur != null && cur.next != null) {
            cur.next.next = cur;
            pre.next = cur.next;
            cur.next = pos;

            pre = cur;
            cur = pos;
            if (pos != null && pos.next != null) pos = pos.next.next;
        }

        return dummy.next;
    }
}
```
T:O(N) S:O(1)

#### 19. Remove Nth Node From End of List - Medium - 需要复习

感想：

1. 直观的做法是先遍历一次，记录整个链表的长度，然后计算出从左往右方向上需要删除的节点的index
2. 但我觉得可以在第一次遍历的时候就找到那个节点，用双指针来做
3. 第一次做时出错了，应该从dummy node开始而不是head，因为head有可能被删除

---
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null || head.next == null) return null;

        ListNode dummy = new ListNode(-1, head);
        ListNode cur = dummy;
        ListNode pos = dummy;
        for (int i = 0; i < n; i ++) {
            pos = pos.next;
        }

        while (pos.next != null) {
            pos = pos.next;
            cur = cur.next;
        }

        cur.next = cur.next.next;
        return dummy.next;
    }
}
```
T:O(N) S:O(1)

#### 160. Intersection of Two Linked Lists - Easy - 需要复习

感想：

1. 对于链表a和b来说，虽然 a != b，但是a + b == a + b，所以两条链表分别从各自的头节点开始遍历，当其中短的那一条走到最后一个节点，再回头从对方的头节点开始遍历。这样在第二次遍历时一定会相遇（只要没环），相遇节点即为intersection. 但是如果再次走到了尾节点也没有相遇，那说明两条链表不相交。
2. 我的做法复杂了，可以优化为判断遍历两条链表的指针什么时候相等，因为如果它们不相交，则最后会同时等于null。

---
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;

        ListNode curA = headA;
        ListNode curB = headB;

        while (curA != curB) {
            curA = curA == null ? headB : curA.next;
            curB = curB == null ? headA : curB.next;
        }

        return curA;
    }
}
```
T:O(N) S:O(1)

#### 142. Linked List Cycle II - Medium - 需要复习

感想：

1. 用快慢指针遍历，如果相交则说明有环；如果走到空指针，则说明无环
2. 当两个指针相交时，快指针走了2k个节点，慢指针只走了k个节点。此时让慢指针从头节点开始重新遍历，而快指针以单步遍历，再次相交时就是环开始的位置。
3. [文档里](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)的讲解非常详细，需要稍微记一下，就不用从数学开始推导了

---
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) return null;

        ListNode fast = head;
        ListNode slow = head;

        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                slow = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```
T:O(N) S:O(1)



