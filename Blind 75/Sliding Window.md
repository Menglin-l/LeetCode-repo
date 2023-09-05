#### 121. Best Time to Buy and Sell Stock

1. 贪心。每一次比较都更新最大值。
2. 我用了一次遍历来做，如果前一个值比当前值大或等于，跳过；如果小，更新最小价格和最大收益。最后如果最大收益还是Integer.MIN_VALUE，说明收益就是0.
3. 不过这次和我以前的做法不一样。。回顾了一下，以前的做法好像更简单一点。。

9.3 - 需要复习

---

#### 3. Longest Substring Without Repeating Characters

1. 这题求的是没有重复字母的字符串，用个hash set就行了。但是要注意只要进行了指针的++操作，取值前就一定要先判断，以防最后一个元素的越界。

9.3 - 隔三个月再复习

---

#### 424. Longest Repeating Character Replacement

1. 这个题关键是判断什么时候缩小窗口，然后统计最高频率的那个字母，不需要用到额外空间。不过我第一遍做的时候没有做对，没有处理好缩小窗口的条件。

9.3 - 需要复习

---
#### 76. Minimum Window Substring

1. 难受，还是没做出来。。。
2. 需要用两个map来分别记录，因为target string里的字母个数也要记录，窗口也要维护，最后取substring时的start位置也要记录。
3. 其实可以用两个长度为128的数组来替代map，复习的时候再写。

9.4 - 需要复习

---
#### Expansion and Review