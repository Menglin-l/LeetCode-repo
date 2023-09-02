#### 20. Valid Parentheses - Easy

感想：

1. 我的做法是用一个辅助栈。首先把所有元素放入主栈中，然后挨个pop，如果pop出的元素是"{,[,("就去辅助栈中找有没有对应的"},],)"。如果都对上了那就valid，反之则invalid。
2. 做的过程中发现只需要用到一个栈，当current element是"{,[,("就入栈，当current element是"},],)"就和栈顶元素比较。最后当指针遍历完整个字符串，看此时栈是否为空。
---
```java
// 我的做法
class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() < 2) return false;

        Stack<Character> stack = new Stack<>();

        char[] arr = s.toCharArray();
        int i = 0;
        while (i < arr.length) {
            if (arr[i] == '[' || arr[i] == '(' || arr[i] == '{') {
                stack.push(arr[i]);
            } else if (stack.isEmpty()) {
                return false;
            } else {
                if (arr[i] == ']') {
                    if (!stack.isEmpty() && stack.peek() != '[') return false;
                    else if (!stack.isEmpty() && stack.peek() == '[') {
                        stack.pop();
                    }
                } else if (arr[i] == '}') {
                    if (!stack.isEmpty() && stack.peek() != '{') return false;
                    else if (!stack.isEmpty() && stack.peek() == '{') {
                        stack.pop();
                    }
                } else if (arr[i] == ')') {
                    if (!stack.isEmpty() && stack.peek() != '(') return false;
                    else if (!stack.isEmpty() && stack.peek() == '(') {
                        stack.pop();
                    }
                }
            }
            i ++;
        }
        return stack.isEmpty();
    }
}
```
T:O(N) S:(N)

3. 不过我的做法还是麻烦了，参照[随想录文档](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E6%80%9D%E8%B7%AF:~:text=%E5%85%A8%E9%83%BD%E5%8C%B9%E9%85%8D%E4%BA%86%E3%80%82-,%E5%88%86%E6%9E%90%E5%AE%8C%E4%B9%8B%E5%90%8E%EF%BC%8C%E4%BB%A3%E7%A0%81%E5%85%B6%E5%AE%9E%E5%B0%B1%E6%AF%94%E8%BE%83%E5%A5%BD%E5%86%99%E4%BA%86,-%EF%BC%8C)的做法，当current element是"{,[,("时，入栈"},],)"；而当current element是"},],)"，直接和栈顶元素比较就行。

```java
// 参考文档的做法，可读性更好
class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() < 2) return false;

        Stack<Character> stack = new Stack<>();
        char[] arr = s.toCharArray();
        int i = 0;
        while (i < arr.length) {
            char c = arr[i];
            if (c == '(') {
                stack.push(')');
            } else if (c == '[') {
                stack.push(']');
            } else if (c == '{') {
                stack.push('}');
            } else if (stack.isEmpty() || stack.peek() != c) {
                return false;
            } else {
                stack.pop();
            }
            i ++;
        }

        return stack.isEmpty();
    }
}
```
T:O(N) S:(N)

#### 1047. Remove All Adjacent Duplicates In String - Easy - 需要复习

感想：

1. 和有效的括号一个套路，当前元素与栈顶元素比较，相同就出栈，并进行下一次的比较；不同就入栈。
2. 我的做法有点啰嗦了，学习了[随想录文档](https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC:~:text=20%0A21%0A22-,%E6%8B%BF%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9B%B4%E6%8E%A5%E4%BD%9C%E4%B8%BA%E6%A0%88,-%EF%BC%8C%E7%9C%81%E5%8E%BB%E4%BA%86%E6%A0%88)里双指针和用StringBuilder来模拟的做法，复习的时候重写一次。

---
```java
class Solution {
    public String removeDuplicates(String s) {
        if (s == null || s.length() < 2) return s;

        char[] arr = s.toCharArray();
        Stack<Character> stack = new Stack<>();
        int idx = 1;
        stack.push(arr[0]);
        while (idx < arr.length) {
            if (!stack.isEmpty() && stack.peek() == arr[idx]) {
                stack.pop();
            } else {
                stack.push(arr[idx]);
            }
            idx ++;
        }

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }

        return sb.reverse().toString();
    }
}
```
T:O(N) S:(N)
#### 150. Evaluate Reverse Polish Notation - Medium

感想：

1. 我的想法是只把数字压入栈中，遇到运算符就依次出栈两个数字进行计算，然后再把结果压入栈中，直至遍历完所有的tokens数组。
2. 忽略了比较两个string不能用==，需要用built method equals()
3. 也不需要很费劲地去做string到integer的转换，用Integer.valueOf()就行了
4. 随想录的方法确实很好，学习了，需要复习。

---
```java
class Solution {
    public int evalRPN(String[] tokens) {
        if (tokens == null || tokens.length < 1) return -1;

        Stack<Integer> stack = new Stack<>();
        int idx = 0;
        int result = 0;
        while (idx < tokens.length) {
            if (tokens[idx].equals("+")) stack.push(stack.pop() + stack.pop());
            else if (tokens[idx].equals("-")) stack.push(-stack.pop() + stack.pop());
            else if (tokens[idx].equals("*")) stack.push(stack.pop() * stack.pop());
            else if (tokens[idx].equals("/")) {
                int tmp1 = stack.pop();
                int tmp2 = stack.pop();
                stack.push(tmp2 / tmp1);
            } else {
                stack.push(Integer.valueOf(tokens[idx]));
            }
            idx ++;
        }
        
        return stack.pop();
    }
}
```
T:O(N) S:(N)