---
layout: blog
comments: true
code: true
title:  "Note for Neural Networks(3)"
tags:
- ML
- DNN, ANN
background-image: https://adventuresinmachinelearning.com/wp-content/uploads/2017/03/medical-abstract-swirls-1-1151086-e1490598260335.jpg
date:   2018-09-12 23:43:54
category: code
---

## Note for Neural Networks (3)

Implementing the gradient descent step 
$$
J(w, b) = \frac{1}{m}\sum^{m}_{z=0}J(W, b, x^{(z)}, y^{(z)})
$$
Vectorize the process:

![1570275593890](https://github.com/yizhao1998/yizhao1998.github.io/raw/master/_posts/code/img/1570275593890.png)

As we need to go through the above operations, we are actually process the data by batches.

we need to sum up all the partial differential terms.

![1570275828806](https://github.com/yizhao1998/yizhao1998.github.io/raw/master/_posts/code/img/1570275828806.png)

Finally, we update.

![1570275859073](https://github.com/yizhao1998/yizhao1998.github.io/raw/master/_posts/code/img/1570275859073.png)

Conclude the whole algorithm

![1570276079943](https://github.com/yizhao1998/yizhao1998.github.io/raw/master/_posts/code/img/1570276079943.png)

Next section will give a detailed explanation for the implementation.


Reference:

[1]. <https://adventuresinmachinelearning.com/neural-networks-tutorial/>