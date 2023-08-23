#### 704. Binary Search - Easy - 需要复习

读题："sorted in ascending order"是个明显的题眼，暗示了可以用二分。

感想：这个题我以前做过，但是对于二分边界条件的处理我做的不好，比如左右边界的取值和到底要不要加等号。

test cases: [5] 5, [2, 5] 5, [2, 5] 2, [2, 5] 0, [2, 5] 8

第一次写用左闭右闭的做法，判断条件要加等号，因为边界值也有效：

    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) return -1;

        int left = 0, right = nums.length - 1; 
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {
                return mid;
            }
        }

        return -1;
    }
time: O(logN)
space: O(1)

试着写一下左闭右开的写法：
如果是左闭右开，那么每一次的mid取值都偏右，而且当left == right的时候没有意义

第一次写没对，学习了[文档上](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#%E6%80%9D%E8%B7%AF) 的方法。

难点：理解每一次缩小搜索半径时的取值

#### 27. Remove Element - Easy - 一次做对

读题：用双指针来做：

1. 左指针从index为0的位置开始从左向右找到第一个target number
2. 右指针从index为nums.length - 1的位置开始从右向左找到第一个非target number
3. swap 左右指针
4. 继续1-3的过程直至左右指针重合
5. 返回左指针的下标 + 1

第一次写bug free：

    public int removeElement(int[] nums, int val) {
        // 0,1,4,0,3,2,2,2
        //         l
        //           r
        if (nums.length == 0 || nums == null) return 0;
        int left = 0, right = nums.length - 1;
        while (left < right) {
            while (left < right && nums[left] != val) {
                left ++;
            }
            while (right > left && nums[right] == val) {
                right --;
            }

            int t = nums[left];
            nums[left] = nums[right];
            nums[right] = t;
        }
        return nums[left] == val ? left : left + 1;
    }
time: O(N)
space: O(1)

#### 34. Find First and Last Position of Element in Sorted Array - Medium - 需要复习

读题：看到“sorted in non-decreasing order”和“find the position of a given target value”就直觉用二分。

1. 先用二分找到target number,标记位置p;
2. 以p为界，往左右两边分别二分，找首次出现的target number和最后一次出现的target number
3. put two indices of the numbers into the result array
4. return the result array

写到2发现写不了了，看了一眼题解，发现是思路错了。其实写两遍二分就完事了，第一遍找第一个数字，第二遍找第二个数字。

    public int[] searchRange(int[] nums, int target) {
        int[] res = new int[2];
        if (nums == null || nums.length == 0) return new int[]{-1, -1};

        res[0] = fromLeftSide(nums, target);
        res[1] = fromRightSide(nums, target);
        return res;
    }

    private int fromLeftSide(int[] nums, int target) {

        int left = 0, right = nums.length - 1;
        // find the first position
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] == target) {
                right = mid - 1;
            } 
        }

        if (left > nums.length - 1 || nums[left] != target) {
            return -1;
        } 
        return left;
    }

    private int fromRightSide(int[] nums, int target) {

        int left = 0, right = nums.length - 1;
        // find the last position
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] == target) {
                left = mid + 1;
            } 
        }
        if (right < 0 || nums[right] != target) return -1;

        return right;
    }
