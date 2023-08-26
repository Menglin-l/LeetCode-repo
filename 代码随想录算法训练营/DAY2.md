#### 977. Squares of a Sorted Array - Easy - 需要复习

读题：

1. 先对每个元素平方处理
2. 找到最小的元素
3. 用双指针比较并交换
4. T: O(N) S: O(1)

没有做好：以最小的元素划分，左边是一个有序数组，右边也是一个有序数组。这种情况下，如何处理当最小元素的index是nums.length - 1?

看了题解，想复杂了，就是合并两个有序数组，新建一个数组就行，但是要注意因为最小元素是在数组中间的某个不确定的位置，简单的方法是从新数组的最后一个元素降序排列。

    public int[] sortedSquares(int[] nums) {
        int[] res = new int[nums.length];

        if (nums == null || nums.length == 0) {
            return res;
        }

        int left = 0, right = nums.length - 1, p = nums.length - 1;

        while (left <= right) {
            if (Math.abs(nums[left]) > Math.abs(nums[right])) {
                int n = nums[left] * nums[left];
                res[p] = n;
                left ++;
            } else {
                int n = nums[right] * nums[right];
                res[p] = n;
                right --;
            }
            p --;
        }
        return res;
    }
T:O(N) S:O(N)

#### 209. Minimum Size Subarray Sum - Medium - 需要复习

读题：滑窗

没有做好：知道怎么做，也知道何时扩大窗口何时缩小窗口，但是不熟。而且这道题只需要控制当窗内的和大于target的情况。

    public int minSubArrayLen(int target, int[] nums) {
        // 2， 3， 1， 2， 4,  3
        //     l
        //                r
        //s:7 length:3
        int left = 0, right = 0;
        int sum = 0, length = Integer.MAX_VALUE;
        while (right < nums.length) {
            sum += nums[right];
            right ++;

            while (left < right && sum >= target) {
                length = Math.min(right - left, length);
                sum -= nums[left];
                left ++;
            }
        }
        return length == Integer.MAX_VALUE ? 0 : length;
    }
T:O(N) S:O(1)

#### 59. Spiral Matrix II - Medium - 需要复习

读题：要设置控制方向的变量

类似的旋转或者螺旋矩阵题：48 54

    public int[][] generateMatrix(int n) {

        int top = 0, bottom = n - 1;
        int left = 0, right = n - 1;
        int num = 1;
        int[][] res = new int[n][n];

        while (num <= n * n) {
            // from left to right on the top
            if (top <= bottom) {
                for (int i = left; i <= right; i ++) {
                    res[top][i] = num;
                    num ++;
                }
                top ++;
            }

            // from top to buttom on the right side
            if (right >= left) {
                for (int j = top; j <= bottom; j ++) {
                    res[j][right] = num;
                    num ++;
                }
                right --;
            } 

            // from right to left on the buttom
            if (bottom >= top) {
                for (int i = right; i >= left; i --) {
                    res[bottom][i] = num;
                    num ++;
                }
                bottom --;
            }

            // from buttom to top on the left side
            if (left <= right) {
                for (int j = bottom; j >= top; j --) {
                    res[j][left] = num;
                    num ++;
                }
                left ++;
            }
        }
        return res;
    }
T:O(n^2) S:O(1)