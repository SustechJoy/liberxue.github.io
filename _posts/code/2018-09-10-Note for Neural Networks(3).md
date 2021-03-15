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

![](https://i.loli.net/2021/03/15/xlUcSHCXedzs9r1.png)

As we need to go through the above operations, we are actually process the data by batches.

we need to sum up all the partial differential terms.

![](https://i.loli.net/2021/03/15/LDxR3GcewkOlNHE.png)

Finally, we update.

![](https://i.loli.net/2021/03/15/POjS6CMVFf2kW89.png)

Conclude the whole algorithm

![](https://i.loli.net/2021/03/15/98PnDfLyqboIGMt.png)

Next section will give a detailed explanation for the implementation.


Reference:

[1]. <https://adventuresinmachinelearning.com/neural-networks-tutorial/>