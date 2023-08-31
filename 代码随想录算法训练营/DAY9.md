#### 28. Find the Index of the First Occurrence in a String - Easy

感想：

1. 学习了kmp算法
2. [随想录文档](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E6%80%9D%E8%B7%AF:~:text=%23-,%E4%BB%80%E4%B9%88%E6%98%AFKMP,-%E8%AF%B4%E5%88%B0KMP)
---
```java
class Solution {
    public int strStr(String haystack, String needle) {
        int m = needle.length();
        int n = haystack.length();

        if (n < m) return -1;

        // create a lps array
        int[] lps = new int[m];
        int prev = 0;
        int idx = 1;
        while (idx < m) {
            if (needle.charAt(idx) == needle.charAt(prev)) {
                prev += 1;
                lps[idx ++] = prev;
            } else {
                if (prev == 0) {
                    lps[idx ++] = 0;
                } else {
                    prev = lps[prev - 1];
                }
            }
        }

        // searching
        int haystackPointer = 0;
        int needlePointer = 0; // It also indicates number of characters matched in current window.
        while (haystackPointer < n) {
            if (haystack.charAt(haystackPointer) == needle.charAt(needlePointer)) {
                needlePointer ++;
                haystackPointer ++;
                if (needlePointer == m) return haystackPointer - m;
            } else {
                if (needlePointer == 0) haystackPointer ++;
                else needlePointer = lps[needlePointer - 1];
            }
        }

        return -1;
    }
}
```
T:O(N) S:(N)

#### 459. Repeated Substring Pattern - Easy

感想：

1. 这题应用了kmp算法前面建立前缀数组的过程，后面比较的部分简化了

---
```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        if (s == null || s.length() == 0) return false;

        // create a lps array
        int[] lps = new int[s.length()];
        int prev = 0;
        int idx = 1;
        while (idx < s.length()) {
            if (s.charAt(idx) == s.charAt(prev)) {
                prev += 1;
                lps[idx ++] = prev;
            } else {
                if (prev == 0) {
                    lps[idx ++] = 0;
                } else {
                    prev = lps[prev - 1];
                }
            }
        }

        int last = lps[s.length() - 1];

        // check if all patterns are valid
        // 1. lps[s.length() - 1] = s.length() - patternLength
        // 2. s.length() % patternLength == 0
        for (int i = 1; i <= s.length() / 2; i ++) {
            if (s.length() % i == 0) {
                if (last == s.length() - i) return true;
            }
        }

        return false;
     }
}
```
T:O(N) S:(N)
