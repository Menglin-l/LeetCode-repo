#### 239. Sliding Window Maximum - Hard - 需要复习

感想：

1. output数组里的元素是每一次滑动时，窗口数组中的最大值。感觉可以用一个priority queue来维护,先改写comparable方法让其变成一个大顶堆，然后每次把要删除的元素和栈顶元素比较一下。如果要删除的就是栈顶元素，那就pop然后入队新元素，否则就一直保持。
2. priority queue解法超时了。。。不过我改进了一下，把一个Pair(nums[i],i)放入pq中，只有当窗口最左边的元素是最大元素时才将其移出队列，而不是每一次都移出。

---
```java
// 我的做法
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k) return new int[0];

        int[] window = new int[nums.length - k + 1];
        PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<>((i1, i2) -> (i2.getKey() - i1.getKey()));
        int index = 0;

        int left = 0; // left window boundary
        int right = 0; // right window boundary

        while (right < nums.length) {
            pq.offer(new Pair(nums[right], right));

            if (right - left + 1 < k) {
                right ++;
            } else {

                // Remove maximum num which lies outside window i.e having index less than left window boundary 
                while (pq.peek().getValue() < left) {
                    pq.poll();
                }

                window[index ++] = pq.peek().getKey();

                left ++;
                right ++;
            }
        }

        return window;
    }
}
```
T:O(NlogN) S:O(N)

3. 学习了[随想录](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html#%E6%80%9D%E8%B7%AF:~:text=%23-,Java,-%EF%BC%9A)的方法，用双端队列来做
4. 不容易想到如何既维护了数组length，又确保每一次找max都是O(1)

---
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length < k) return new int[0];

        ArrayDeque<Integer> deque = new ArrayDeque<>();// traverse every index of the element in nums array
        int[] res = new int[nums.length - k + 1];
        int idx = 0;

        for (int i = 0; i < nums.length; i ++) {// keep current index of the element on the top and the max on the last
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) { // keep the tail element is the max
                deque.pollLast();
            }

            while (!deque.isEmpty() && deque.peek() < i - k + 1) {// keep the size of deque is from i - k + 1 to i
                deque.poll();
            }

            deque.offer(i);

            if (i + 1 >= k) {
                res[idx ++] = nums[deque.peek()];
            }
        }

        return res;
    }
}
```
T:O(N) S:O(K)

#### 347. Top K Frequent Elements - Medium

感想：

1. 用priority queue和hash map来做。先用map统计每个元素和相应的频率，再把map的entry放入pq中，改造comparable方法使其成为大顶堆，然后pop出k个元素。

---
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        if (nums == null || nums.length < k) return new int[0];

        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        int[] res = new int[k];
        int i = 0;
        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>((i1, i2) -> (i2.getValue() - i1.getValue()));

        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            pq.offer(entry);
        }

        while (!pq.isEmpty() && i < k) {
            res[i ++] = pq.poll().getKey();
        }

        return res;
    }
}
```
T:O(NlogK) S:O(N)