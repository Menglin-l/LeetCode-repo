#### 153. Find Minimum in Rotated Sorted Array

1. 从例子可以很清楚地看到，如果mid元素比两端都小，说明最小元素在左半侧；如果都大，说明在右半侧，从而知道如何缩小范围。
2. 我做错了两个地方：一是在缩小范围时，需要考虑mid和left以及mid和right索引相等；二是我提前排除了从0到2个元素时的情形，确定开始while循环操作的一定有大于等于3和元素，所以可以去掉对mid取值范围的判断。
3. 自己debug给整对了，但操作还是不熟。

9.4 - 需要复习

---