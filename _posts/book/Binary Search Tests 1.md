---
layout: blog
istop: true
title: "Binary Search Tests 1"
background-image: https://o243f9mnq.qnssl.com/2017/06/116099051.jpg
date:  2017-03-07
category: book
tags:
- ACM
- Binary_Search
---

## Binary Search Tests

### Easy1: High School Problem

Joy is a twelfth grade student who is aiming to go to SUSTech. One day, when he is doing math practices. He found a problem very hard. A function goes like 
$$
F(x) = 5x^{7}+6x^{6}+3x^{3}+4x^{2}-2xy (0 <= x <=100)
$$
y is a given real number. Can you find the minimum value when x is in its domain.

##### Input

The first line of the input contains an integer T $(1<=T<=100)$  which means the number of test cases. Then T lines follow, each line has only one real numbers Y. $( 0 < Y < 10^{10} )$

##### Output

For each case, print the case number as well as one line contains the minimum value (accurate up to 4 decimal places), when x is in its domain.

##### Sample Input

```
3
100
200
300
```

##### Sample Output

```
Case 1: -193.3774
Case 2: -448.1475
Case 3: -729.3383
```

