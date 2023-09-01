#### 232. Implement Queue using Stacks - Easy - 需要复习

感想：

1. 我的做法是，只要保证行为永远一致就行了。永远在stack1里peek, push, 和pop,stack2永远只做为辅助。但是这样，如果碰到连续的pop或者peek，总是需要把元素在stack1和stack2之间转来转去。
2. [随想录题解里](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E6%8B%93%E5%B1%95:~:text=%23-,Java,-%EF%BC%9A)的做法是stack1只负责进，stack2只负责出。这样的确更好读易懂。
---
```java
class MyQueue {

    private Stack<Integer> stack1;
    private Stack<Integer> stack2;

    public MyQueue() {
        this.stack1 = new Stack<>();// in
        this.stack2 = new Stack<>();// out
    }
    
    public void push(int x) {//O(1)
        stack1.push(x);
    }

    public int pop() {//O(N)
        inToOut();
        return stack2.pop();
    }
    
    public int peek() {//O(N)
        inToOut();
        return stack2.peek();
    }
    
    public boolean empty() {//O(1)
        return stack1.isEmpty() && stack2.isEmpty();
    }

    private void inToOut() {//O(N)
        if (!stack2.isEmpty()) return;
        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }
    }
}
```
S:O(N)

#### 225. Implement Stack using Queues - Easy

感想：
1. 和前一题很像，不过这里既可以用两个队列来做，也可以用一个队列来做。

---
```java
// 用两个队列
class MyStack {

    LinkedList<Integer> que1;
    LinkedList<Integer> que2;

    public MyStack() {
        this.que1 = new LinkedList<>(); // in
        this.que2 = new LinkedList<>(); // out
    }
    // q1: 3
    // q2: 2 1
    public void push(int x) {//O(1)
        que1.add(x);
    }
    
    public int pop() {//O(N)
        inToOut();
        return que2.remove();
    }
    
    public int top() {//O(N)
        inToOut();
        return que2.peek();
    }
    
    public boolean empty() {//O(1)
        return que1.isEmpty() && que2.isEmpty();
    }

    private void inToOut() {//O(N)
        if (que1.isEmpty()) return;
        while (!que1.isEmpty()) {
            que2.addFirst(que1.remove());
        }
    }
}
```
```java
//用一个队列
class MyStack {

    Deque<Integer> que;

    public MyStack() {
        que = new LinkedList<>();
    }
    // q1: 3
    // q2: 2 1
    public void push(int x) {//O(1)
        que.addFirst(x);
    }

    public int pop() {//O(1)
        if (que.isEmpty()) return -1;
        return que.pollFirst();
    }

    public int top() {//O(1)
        if (que.isEmpty()) return -1;
        return que.peekFirst();
    }

    public boolean empty() {//O(1)
        return que.isEmpty();
    }
}
```