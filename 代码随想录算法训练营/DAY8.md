#### 344. Reverse String - Easy

感想：

1. 用双指针

---
```java
class Solution {
    public void reverseString(char[] s) {
        if (s == null || s.length == 0) return;

        int left = 0, right = s.length - 1;
        while (left < right) {
            swap(s, left, right);

            left ++;
            right --;
        }
    }

    private void swap(char[] s, int l, int r) {
        char t = s[l];
        s[l] = s[r];
        s[r] = t;
    }
}
```
T:O(N) S:(1)
#### 541. Reverse String II - Easy

感想：

1. 这个题加了一些限制，需要考虑当剩下的字符数不足k时的情况
2. 我做的不好，把条件设置得复杂了，错了两次
---
```java
class Solution {
    public String reverseStr(String s, int k) {
        if (s == null) return s;

        char[] arr = s.toCharArray();
        for (int i = 0, j = 0; i < arr.length; i += 2 * k) {
            if (k >= arr.length - i) reverseString(arr, i, arr.length - 1);
            else reverseString(arr, i, i + (k - 1));
        }

        return String.valueOf(arr);
    }

    private void reverseString(char[] arr, int left, int right) {
        while (left < right) {
            char t = arr[left];
            arr[left] = arr[right];
            arr[right] = t;

            left ++;
            right --;
        }
    }
}
```
T:O(N) S:(N)
#### 剑指 Offer 05. 替换空格

感想：
1. 很直接的一个题，也因为Java里的String是immutable的，用StringBuilder就行了。
---
```java
class Solution {
    public String replaceSpace(String s) {
        if (s == null || s.length() == 0) return s;

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i ++) {
            if (s.charAt(i) == ' ') {
                sb.append("%20");
            } else {
                sb.append(s.charAt(i));
            }
        }

        return sb.toString();
    }
}
```
T:O(N) S:(N)
#### 151. Reverse Words in a String - Medium - 需要复习

感想：

1. 我的方法就是先去掉字符串里所有多余的空格，然后翻转整个字符串，最后翻转每个单词
2. 对边界条件的取值操作不熟练，写得太慢了。。。
---
```java
class Solution {  
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) return s;

        char[] arr = s.toCharArray();
        // trim all spaces using an array
        int left = 0, right = 0;
        while (right < arr.length && left < arr.length) {
            while (right < arr.length && arr[right] == ' ') right ++;

            if (right < arr.length && left != 0) {
                arr[left] = ' ';
                left ++;
            }

            while (right < arr.length && arr[right] != ' ') {
                arr[left] = arr[right];

                right ++;
                left ++;
            }
        }

        int end = left - 1;
        // reverse the entire string
        left = 0;
        right = end;
        reverseString(arr, left, right);

        // reverse each word in the string
        left = 0;
        right = 0;
        while (right <= end) {
            while (right <= end && arr[right] != ' ') right ++;
            reverseString(arr, left, right - 1);

            right ++;
            left = right;
        }

        return new String(arr, 0, right - 1);
    }

    private void reverseString(char[] arr, int left, int right) {
        while (left < right) {
            char t = arr[right];
            arr[right] = arr[left];
            arr[left] = t;

            left ++;
            right --;
        }
    }
}
```
T:O(N) S:(N)
#### 剑指 Offer 58 - II. 左旋转字符串 LCOF

感想：

1. 初看就是很简单的题，但是我用了substring()的方法，空间复杂度为N
2. 学习了[随想录的文档](https://programmercarl.com/%E5%89%91%E6%8C%87Offer58-II.%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E9%A2%98%E5%A4%96%E8%AF%9D:~:text=%23-,Java%EF%BC%9A,-class%20Solution%20%7B)上把空间复杂度优化到1的方法，重做了一次
---
```java
// 我的方法
class Solution {
    public String reverseLeftWords(String s, int n) {
        if (s == null || s.length() <= n) return s;

        StringBuilder sb = new StringBuilder();
        sb.append(s.substring(n, s.length())).append(s.substring(0, n));

        return sb.toString();
    }
}
```
```java
// 随想录的方法
class Solution {
    public String reverseLeftWords(String s, int n) {
        if (s == null || s.length() <= n) return s;

        char[] arr = s.toCharArray();
        reverseString(arr, 0, arr.length - 1);
        reverseString(arr, 0, arr.length - n - 1);
        reverseString(arr, arr.length - n, arr.length - 1);

        return new String(arr);
    }

    private void reverseString(char[] arr, int left, int right) {
        while (left < right) {
            char c = arr[right];
            arr[right] = arr[left];
            arr[left] = c;

            left ++;
            right --;
        }
    }
}
```
T:O(N) S:(1)