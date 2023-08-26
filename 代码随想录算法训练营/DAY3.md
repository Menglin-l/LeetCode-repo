#### 203. Remove Linked List Elements - Easy - 需要复习

读题：链表基本操作

感想：

1. 因为头节点可能被删除，最好加一个dummy node
2. 链表的题还是得画图

---
```
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) return head;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        ListNode cur = dummy;
        while (cur != null && cur.next != null) {
            if (cur != null && cur.next.val == val) {
                cur.next = cur.next.next;
            } else if (cur != null && cur.next.val != val) {
                cur = cur.next;
            }
        }
        return dummy.next;
    }
```
T:O(N) S:O(1)


#### 707. Design Linked List - Medium - 需要复习

感想：

1. 是个很考验基本功的题，看着常规，但有不少edge case需要考虑
2. 看了眼题解发现原来设置一个size就不会那么痛苦了

---
```
class MyLinkedList {
    class ListNode {
        int val;
        ListNode next;

        ListNode() {}
        ListNode(int val, ListNode next) {
            this.val = val;
            this.next = next;
        }
    }

    private ListNode dummy;
    private int size;

    public MyLinkedList() {
        this.dummy = new ListNode(-1, null);
        this.size = 0;
    }
    
    // T:O(N) S:O(1)
    public int get(int index) { 
        if (index < 0 || index >= size) return -1;

        ListNode tmp = dummy.next;

        for (int i = 0; i < index; i ++) {
            tmp = tmp.next;
        }

        return tmp.val;
    }
    // T:O(1) S:O(1)
    public void addAtHead(int val) {

        dummy.next = new ListNode(val, this.dummy.next);
        this.size ++;
    }
    // T:O(N) S:O(1)
    public void addAtTail(int val) {
        ListNode cur = dummy;

        while (cur.next != null) {
            cur = cur.next;
        }

        cur.next = new ListNode(val, null);
        this.size ++;
    }
    // T:O(N) S:O(1)
    public void addAtIndex(int index, int val) {
        if (index < 0 || index > this.size) return;

        ListNode cur = this.dummy;
        for (int i = 0; i < index; i ++) {
            cur = cur.next;
        }

        cur.next = new ListNode(val, cur.next);
        this.size ++;
    }
    // T:O(N) S:O(1)
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return;

        ListNode cur = this.dummy;

        for (int i = 0; i < index; i ++) {
            cur = cur.next;
        }

        cur.next = cur.next.next;
        this.size --;
    }
}
```

#### 206. Reverse Linked List - Easy - 比较简单

读题：基本操作，可以用穿针法一边遍历一边翻转

---
```
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode dummy = new ListNode(Integer.MAX_VALUE, head);
        ListNode cur = head.next;
        ListNode pos = cur.next;

        dummy.next.next = null;

        while (cur != null) {
            cur.next = dummy.next;
            dummy.next = cur;
            cur = pos;
            if (pos != null) pos = pos.next;
        }
        return dummy.next;
    }
```
T:O(N) S:O(1)