#### 49. Group Anagrams

1. map的key应该是string,不能是char[]

8.29 - 第一次用了char[]为key，出错。不是bug free，需要复习

---
#### 347. Top K Frequent Elements

1. 忘了priorityqueue的用法
2. 忘了把整个entry放到pq里，只是简单地对元素排序，这样做不对
3. 忘了hashmap的操作

8.29 - 需要复习

---
#### 238. Product of Array Except Self

1. 分情况讨论那里没做好，反复修改了几次，不能一遍过

8.29 - 需要复习

---
#### 271. Encode and Decode Strings

1. 最好的办法是标记每个字符串的个数，然后转换为一个字符，放在该字符串前面。等到解码的时候，将这个字符转换为数字，依次往后取相应个数的substring就行了。
2. 难点在于不能用char size = (char)(str.length() + '0');的方法，这样会数字转换为相应的ascii码。而直接int size = str.length(); sb.append((char) size).append(str);这样就把数字转换为一个Unicode character，解码的时候直接用int读就可以了。

8.31 - 需要复习

---
#### 128. Longest Consecutive Sequence

1. 没有思路
2. 学习了可以先用hashset过一遍整个数组，然后再挨个去找每一组连续sequence的起始数字，然后记录最大长度。

8.31 - 需要复习

---
#### Expansion and Review