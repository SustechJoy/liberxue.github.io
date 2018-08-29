---
layout: blog
comments: true
book: false
title:  "Binary Search Test 5"
tags:
- ACM
- binary search
background-image: http://www.sustc.edu.cn/resources/cn/image/p27.png
date:   2018-08-20 11:15:00
category: book
---

## Binary Search Tests

#### Medium2: Puberty Rite

A remote planet is called Joy. There is an ancient rule in Joy that every kid need to go for a hiking in dark forest to become an adult. This year, the kids of Joy that need to go for a hiking want to go together to ensure their safety. But they want to calculate a position to gather which minimizes sum of their “distance”.

To simplify, we assume Joy is a straight line, every kid has a location $x_i$ on the line and every $x_i$ has a corresponding weight $w_i$. As the gravitational force on Joy differs from earth, the “distance” between their home and the gathering position $p$ need to be calculated as $S^{3}*W_i$ where S is $|x_i - p|$.

In a word, we need to calculate the position p that minimize the total distance $\sum\ S^{3}*W_i$ where S is $|x_i - p|$.

Hint: Triple  search.

##### Input

The
first line of the input is the number $T$ $(1<=T<=20)$, which is the number of test cases. The first line of each case has one integer $N$ $(1<=N<=50000)$, indicating the number of kids. Then comes $N$ lines in the order that $x[i]<=x[i+1]$ for all $i$ $(1<=i<N)$. The $i_{th}$ line contains two real number : $X_i$, $W_i$, representing the location and the weight of the $i_{th}$ kid. $( |x_i|<=10^6, 0< w_i <15 )$

##### Output

For each test case, please output a line which is "Case #X: Y", $X$ is the number of the test case and $Y$ is the minimum sum of “distance” which is rounded to the nearest integer.

##### Sample Input

```
1
4
0.6 5
3.9 10
5.1 7
8.4 10
```

##### Sample Output

```
Case #1: 832
```

