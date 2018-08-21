---
layout: blog
comments: true
book: true
title:  "Binary Search Test 6"
tags:
- ACM
- binary search
background-image: http://www.sustc.edu.cn/resources/cn/image/p27.png
date:   2018-08-20 11:15:00
category: book
---

## Binary Search Tests

#### Hard1:  Save Joy

Joy is now applying for postgraduate programs. But due to his low GPA, no school really want to receive him. One day poor Joy meet Aladdin, Aladdin promises to help him delete some of his courses he attended to increase his GPA. But now he is confused again, can you write a program to help him point out the maximum GPA he can get by deleting courses.

It should be mentioned that the calculation formula of GPA is $\frac{\sum s[i]c[i]}{\sum s[i]}$, where the academic credit of the $i_{th}$ course is $s[i]$ and the score of the $i_{th}$ course is $c[i]$. And because of the limitation of power of the magic lamp, he could at most delete $k$ courses.

##### Input

The first line has two positive integers $n$ $(1<=n<=10^5)$, $k$ $(0<=k<n)$, which is the total course number of Joy and the maximum number of deleting course. The second line has $n$ positive integers $s[i]$. The third line has $n$ positive integers $c[i]$ $(1<=s[i], c[i]<=10^3)$.

##### Output

Output the highest final score, your answer is correct if and only if the absolute
error with the standard answer is no more than $10^{-5}$.

##### Sample Input

```
3 1
1 2 3
3 2 1
```

##### Sample Output

```
2.33333333333
```

