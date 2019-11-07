---
title: 深度学习基础知识
date: 2019-11-05 16:36:29
tags:
- DeepLearning
categories: DeepLearning
mathjax: true
---

## 二分分类

>问题引入：你需要识别图片是否含有一只猫，如果含有，则返回1，否则返回0

### 基础概念

1. 设这张图片为64×64像素，则这张图片实际上由三个64×64的矩阵构成，分别表示红绿蓝的RGB值，我们用一个64×64×3的向量x来表示这张图片，称为**特征向量**

2. 一些符号：
   * **n~x~**：特征向量的长度（有时用 n 表示）
   * **(x, y)**：x ∈ R^nx^ ，y ∈ {0， 1}
   * **m**：训练集(x^(1)^, y^(1)^), (x^(2)^, y^(2)^), (x^(3)^, y^(3)^)…… (x^(m)^, y^(m)^)的训练样本数量
   * **m~train~**：训练集样本数         **m~test~**：测试集样本数
   * **X**：由训练集中特征向量x~1~、x~2~构成的矩阵n~x~×m的矩阵

3. **logistic回归**：
   * 给定$x$，令$\hat y  = P(y=1 | x)  $
   
   * 参数： $w \in R^{n_x}，b \in R$
   
   * 输出：$\hat y = w（ w^Tx + b）$
   
   * sigmoid函数: $\sigma(z) = \frac{1}{1+e^{-z}}$，图像如下：
   
     ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20191107160317.png)





* **损失函数**（Loss Function）：
  $$
  L(\hat y, y) = \frac{1}{2}(\hat y - y)^2
  $$
  我们定义这个函数来衡量 $\hat y$ 和 y 有多接近

  但我们在逻辑回归中使用以下损失函数：
  $$
  L(\hat y, y) = -(ylog\hat y + (1 - y)log(1 - \hat y))
  $$
  我们需要使得损失函数尽可能小。对以上函数，y = 1时，第二项为0，故只看第一项，第一项中，如果使 $-ylog\hat y$ 尽可能小，就可以让要想使得 $log\hat y$也就是$\hat y$ 尽可能大，即尽可能接近1，与y也就越加接近。y = 0时，第一项为零，同理可分析可知，$L(\hat y, y)$尽可能小时，会使得$\hat y$更接近1。

  损失函数衡量了对单个样本预测的准确程度



* **成本函数**（Cost Function）：
  $$
  \begin{equation}
  \begin{split}
  J(w, b) &= -\frac{1}{m}\sum_{i=1}^{m}L(\hat y^{(i)}, y^{(i)})\\
  &=-\frac{1}{m}\sum_{i=1}^{m}-(y^{(i)}log\hat y^{(i)} + (1 - y^{(i)})log(1 - \hat y^{(i)}))
  \end{split}
  \end{equation}
  $$
  成本函数衡量了对整个样本集预测的准确程度，且$J(w,b)$越小越好



### 梯度下降法（Gradient Descent）

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/image-20191107163229574.png)

* 使用以上成本函数可以保证$J(w,b)$是凸函数，否则可能出现多个局部最优解，影响梯度下降法的结果（类似高次多项式函数的情况）

* 梯度下降法所做的，就是从初始点开始，一步步的沿着最陡的下降路线朝着函数的最低点走。而这个最陡的下降路线，可以用高等数学中学习到的**偏导数**进行求解。

* 为了方便解释，首先忽略b，仅仅对w进行梯度下降法，则$J(w)$d的图像如图所示：

  ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20191107163803.png)

  我们对w进行如下重复操作：
  $$
  w:=w - \alpha\frac{\mathrm{d}J(w)}{\mathrm{d}w}
  $$
  我们知道，导数代表该点的斜率，若w大于最小值所在的点最高点，则其导数为正值，减去导数，w减小，故更加接近最小值所在点；若w小于最小值所在的点，同理可推得，其也更加接近最小值，如图所示：

  ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20191107164833.png)