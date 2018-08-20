---
layout: blog
book: true
title:  "Binary Search Test 3"
tags:
- ACM
- binary search
background-image: http://www.sustc.edu.cn/resources/cn/image/p27.png
date:   2018-08-20 11:15:00
category: book
---

## Binary Search Tests

### Easy3: Fight against demon

Under your help, the sheik of the forest found that the demons weren’t among the men. They must have escaped. After reading ancient books, the sheik found that exactly $m​$ cd (candela, which is the unit of strength of light) units of light exposed by the join of two light swords can kill the demons. The men in the forest brought n light swords and put them <b>in ascending order.</b> Could you help sheik to find how many pairs are in the given $n​$ light swords with a combination of $m​$ cd.

Here we simply regard the strength of the combination of light swords is the sum of light strength of the original ones.

##### Input

The first line will be an integer T$(1<=T<=10)$, which is the number of test cases.

For each case contain two lines. The first line contains two numbers n $(1 <= n <= 5000)$ and m $(1 <= m <= 10^8)$, $n$ is the number of light swords. $m$ is the specified target.

The second line contains $n$ integers: $a_{1}, a_2, ... a_n$ $(1 <= a_{i} <= 10^6, 1 <= i <= n)​$, which are the light strength of the light swords brought by men in the forest.

##### Output

For each case print a number, the number of pairs.

##### Sample Input

```
3
4 5
1 2 3 4
4 9
2 7 11 15
3 6
1 2 3
```

##### Sample Output

```
2
1
0
```

