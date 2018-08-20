---
layout: blog
book: true
title:  "Binary Search Test 2"
tags:
- ACM
- binary search
background-image: http://ot1cc1u9t.bkt.clouddn.com/17-7-15/82431810.jpg
date:   2017-06-27 23:43:54
category: book
---

## Binary Search Tests 

#### Easy2: Find the demon 

There was demons in Binary Forest. They committed terrible crimes. But no one saw their face exactly, the only known thing is their height (they all share a common height, which is diﬀerent from any other people). One day, the sheik of the forest asked all men lived in the forest to line up and stand in ascending order of their height. But he didn’t know how to ﬁnd the demons. Could you please help the poor sheik?

##### Input 

The ﬁrst line will be an integer $T$ $(100<=T<=1000)$, which is the number of test cases. For each case have two lines. The ﬁrst line of the input contains two number $n$ and $m$, $n$ is the number of the men in the forest $(1 <= n <= 10^6)$, and $m$ $(1 <= m <= 10^8)$ is the demon’s height. The second line contains $n$ numbers which are the heights of the men in the forest. You should determine whether demon(s) is(are) among the men or not.

##### Output 

For each case, output only one line. If there is(are) demon(s) among the men, output YES, otherwise output NO.

##### Sample Input

```
2
3 4
1 2 3
3 1
1 2 3
```

##### Sample Output

```
NO
YES
```

