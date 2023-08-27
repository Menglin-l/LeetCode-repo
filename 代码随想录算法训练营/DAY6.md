#### 242. Valid Anagram - Easy

感想：

1. 直接的方法是用个HashMap来做，先记录s里的字母与相应出现的频率，再去比较t里的，space complexity为O(N)
2. 可以用一个字母数组来代替哈希表，优化空间复杂度为O(1)

---
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s == null || t == null || s.length() != t.length()) return false;

        int[] alph = new int[26];
        char[] arrS = s.toCharArray();
        char[] arrT = t.toCharArray();

        for (int i = 0; i < arrS.length; i ++) {
            alph[arrS[i] - 'a'] ++;
            alph[arrT[i] - 'a'] --;
        }

        for (int i : alph) {
            if (i != 0) return false;
        }

        return true;
    }
}
```
T:O(N) S:O(1)

#### 349. Intersection of Two Arrays - Easy - 需要复习

感想：

1. 首先两个数组长度可能不相等，先找出长数组，然后把短数组的字母与出现频率放入一个HashMap里，再去长数组中比较
2. 呃。。看了眼题解，原来用HashSet就可以了，我做得太复杂了

---
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>();
        for (int n : nums1) {
            set1.add(n);
        }

        Set<Integer> set2 = new HashSet<>();
        for (int n : nums2) {
            set2.add(n);
        }

        if (set1.size() >= set2.size()) return findTheIntersection(set1, set2);
        else return findTheIntersection(set2, set1);
    }

    private int[] findTheIntersection(Set<Integer> longArr, Set<Integer> shortArr) {
        List<Integer> list = new ArrayList<>();

        for (Integer n : shortArr) {
            if (longArr.contains(n)) {
                list.add(n);
            }
        }

        // We can use this streams() method of list and mapToInt() to convert ArrayList<Integer> to array of primitive 
        // data type int
        return list.stream().mapToInt(i -> i).toArray();
    }
}
```
T:O(N + M), where N is the length of nums1 and M is the length of nums2 
S:O(N * M), when nums1 and nums2 don't have any intersections

感想

3. [随想录的文档](https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC:~:text=%23-,Java,-%EF%BC%9A)里，直接在放入set的时候就可以判断，省去一个helper method。更新一下：

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) return new int[0];

        Set<Integer> set1 = new HashSet<>();
        Set<Integer> res = new HashSet<>();

        for (Integer n : nums1) {
            set1.add(n);
        }

        for (Integer n : nums2) {
            if (set1.contains(n)) {
                res.add(n);
            }
        }

        return res.stream().mapToInt(i -> i).toArray();
    }
}
```
The time and space complexities are the same as the above approach.

#### 202. Happy Number - Easy

感想：

1. 难点在于如果不是快乐数，那什么时候应该停下来？-> 当出现重复和的时候
2. 所以用个set来存放每次的和就行了

---
```java
// 我的做法
class Solution {
    public boolean isHappy(int n) {
        if (n == 1) return true;
        if (n == 0) return false;

        Set<Long> set = new HashSet<>();
        
        long sum = findSum((long) n);
        if (sum == 1) return true;
        else {
            while (!set.contains(sum)) {
                set.add(sum);
                sum = findSum(sum);
                if (sum == 1) return true;
            }
        }

        return false;
    }

    private long findSum(long n) {
        long sum = 0L;

        while (n != 0) {
            long remain = n % 10;
            sum += remain * remain;
            n /= 10;
        }

        return sum;
    }
}
```
太啰嗦了，可以把开头那两个特判的逻辑直接放到while里，而且用int就行了，不需要换成long
```java
// 优化后
class Solution {
    public boolean isHappy(int n) {

        Set<Integer> set = new HashSet<>();

        while (n != 1 && !set.contains(n)) {
            set.add(n);
            n = findSum(n);
        }

        return n == 1;
    }

    private int findSum(int n) {
        int sum = 0;

        while (n != 0) {
            int remain = n % 10;
            sum += remain * remain;
            n /= 10;
        }

        return sum;
    }
}
```
T:O(logN) S:O(logN)

#### 1. Two Sum - Easy

感想：

没有什么好说的，基本操作

---
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] res = new int[2];
        if (nums == null || nums.length == 0) return res;
        
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i ++) {// i = 0, nums[i] = 2, target - nums[i] = 7
            if (map.containsKey(nums[i])) {
                res[0] = i;
                res[1] = map.get(nums[i]);
            }

            map.put(target - nums[i], i);//map:{{7, 0}, }
        }

        return res;
    }
}
```
T:O(N) S:O(N)