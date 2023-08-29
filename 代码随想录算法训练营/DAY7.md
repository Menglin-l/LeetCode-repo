#### 454. 4Sum II - Medium - 做错了，需要复习

感想：

1. 首先要理解，本题的元素可以重复，只统计最后的个数即可

---
```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums1.length; i ++) {
            for (int j = 0; j < nums2.length; j ++) {
                int sum = 0 - nums1[i] - nums2[j];
                map.put(sum, map.getOrDefault(sum, 0) + 1);
            }
        }

        int res = 0;
        for (int i = 0; i < nums3.length; i ++) {
            for (int j = 0; j < nums4.length; j ++) {
                int sum = nums3[i] + nums4[j];
                
                res += map.getOrDefault(sum, 0);
            }
        }

        return res;
    }
}
```
T:O(N^2) S:O(N^2)
#### 383. Ransom Note - Easy - 需要复习

感想：

1. 这题和Valid Anagram挺像的，但是不用考虑magazine的情况，我用了一个哈希表来做
2. 但是可以用一个长度为26的数组来优化

---
```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote == null || magazine == null || ransomNote.length() > magazine.length()) return false;

        int[] arr = new int[26];
        char[] ransom = ransomNote.toCharArray();
        char[] mag = magazine.toCharArray();

        for (int i = 0, j = 0; j < mag.length; j ++) {
            if (i < ransom.length) {
                arr[ransom[i] - 'a'] += 1;
                i ++;
            }
            arr[mag[j] - 'a'] -= 1;
        }

        for (int i = 0; i < 26; i ++) {
            if (arr[i] > 0) return false;
        }

        return true;
    }
}
```
T:O(N) S:O(1)
#### 15. 3Sum - Medium - 需要复习

感想：

1. 用for loop挨个确定第一个数，将本题降维为两数和。
2. 这题返回的是不重复的组合，而不是索引，所以可以用双指针来优化哈希表
3. 参照[随想录的文档](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF:~:text=%E5%8E%BB%E9%87%8D%E9%80%BB%E8%BE%91%E7%9A%84,%23)说明
4. 我做错了两点，第一是忘了先排序，第二是只对第一个数去重了，但实际上后面两个数也需要去重

---
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();

        if (nums == null || nums.length < 3) return res;
        Arrays.sort(nums);
        if (nums[0] > 0) return res;

        for (int i = 0; i < nums.length; i ++) {
            int sum = 0 - nums[i];
            if (i > 0 && nums[i - 1] == nums[i]) continue;

            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                while (left < right && nums[left] + nums[right] < sum) left ++;
                while (left < right && nums[left] + nums[right] > sum) right --;

                if (left < right && nums[left] + nums[right] == sum) {
                    res.add(new ArrayList<>(Arrays.asList(nums[i], nums[left], nums[right])));

                    while (left < right && nums[left] == nums[left + 1]) left ++;
                    while (left < right && nums[right - 1] == nums[right]) right --;

                    left ++;
                    right --;
                }
            }
        }
        return res;
    }
}
```
T:O(N^2) S:O(1)
#### 18. 4Sum - Medium - 需要复习

感想：

1. 和3sum比较像，先排序，再去重
2. 我去，居然有个case没过 -> nums: [1000000000,1000000000,1000000000,1000000000] target: -294967296

---
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();

        if (nums == null || nums.length < 4) return res;
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i ++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < nums.length; j ++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;

                long sum = (long) target - nums[i] - nums[j];
                int left = j + 1, right = nums.length - 1;

                while (left < right) {
                    while (left < right && nums[left] + nums[right] < sum) left ++;
                    while (left < right && nums[left] + nums[right] > sum) right --;

                    if (left < right && nums[left] + nums[right] == sum) {
                        res.add(new ArrayList<>(Arrays.asList(nums[i], nums[j], nums[left], nums[right])));

                        while (left < right && nums[left] == nums[left + 1]) left ++;
                        while (left < right && nums[right] == nums[right - 1]) right --;

                        left ++;
                        right --;
                    }
                }
            }
        }

        return res;
    }
}
```
T:O(N^3) S:O(1)