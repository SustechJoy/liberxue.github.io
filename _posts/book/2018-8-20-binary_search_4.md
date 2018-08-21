---
layout: blog
book: true
comments: true
title:  "Binary Search Test 4"
tags:
- ACM
- binary search
background-image: http://www.sustc.edu.cn/resources/cn/image/p27.png
date:   2018-08-20 11:15:00
category: code
---

## Binary Search Tests

### Medium1: University Hotel

As you know, every university has a hotel but it is always hard to book a room. Every year, thousands of orders fly to the hotels. The boss of a hotel in S University is busy, and hopes to make a program to deal with the orders. He know you are good at programming, and he turns to you for help. The problem is described as follows:

We need to deal with the orders in the following $n$ days. During the n days, there will be $r_i$ rooms available. And there are $m$ orders, every order is described by 3 integers, which are $d_j$, $s_j$, $t_j$ respectively. This indicates the customer need to book d~j~ rooms from day $s_j$ to $t_j$ (inclusive). To simplify,  we need not to care whether the customer get the same room everyday.

The principle for booking rooms is First come, First get, $i.e.$, we need to assign rooms for the customers by the chronological sequence of the orders. If any one order can’t be satisfied, we need to stop assignment and inform the customer to modify the order. Here, the dissatisfaction refers to that at least one day during day $s_j$ to $t_j$ the number of remaining rooms is less than $d_j$.

Now we need to know, whether there are orders can’t be satisfied. And if so, we should inform which customer to modify the order.

##### Input

The first line will be 2 integers $n$, $m$, which are the number of days and orders.

The second line contains $n$ integers the $i_{th}$ number is $r_i$, indicates the number of rooms can be booked that day. 

Then there are $m$ lines, every line has 3 integers $d_j$, $s_j$, $t_j$, indicates the number of rooms  to be booked, the begin day and end day of the $j_{th}$ order. $(1≤n,m≤10^6,0≤r_i,d_j≤10^9,1≤s_j≤t_j≤n)$

##### Output

If all orders can be satisfied, the only output is one line, contains an integer 0, otherwise output two line, the first line output -1, and the second line output the index of the order that should be modified.

##### Sample Input

```
4 3 
2 5 4 3 
2 1 3 
3 2 4 
4 2 4 
```

##### Sample Output

```
-1
2
```

