To develop a more powerful approach to image classification that will have two major components: a **score function** that maps the raw data to class scores, and a **loss function** that quantifies the agreement between the predicted scores and the ground truth labels. We will then cast this as an optimization problem in which we will minimize the loss function with respect to the parameters of the score function.

可以认为线性分类器在做一个模板匹配。只关心影响最后输出的像素点；或者认为是高维空间上的一个分类边界

线性分类器没有能力同时获取多个模型，所以如果模型中红色较多，模型就会偏红色
分类边界非线性时，线性分类器无法工作

**Setting Delta.** Note that we brushed over the hyperparameter Δ and its setting. What value should it be set to, and do we have to cross-validate it? It turns out that this hyperparameter can safely be set to Δ=1.0 in all cases. The hyperparameters Δ and λ seem like two different hyperparameters, but in fact they both control the same tradeoff: The tradeoff between the data loss and the regularization loss in the objective. The key to understanding this is that the magnitude of the weights W has direct effect on the scores (and hence also their differences): As we shrink all values inside W the score differences will become lower, and as we scale up the weights the score differences will all become higher. Therefore, the exact value of the margin between the scores (e.g. Δ=1, or Δ=100) is in some sense meaningless because the weights can shrink or stretch the differences arbitrarily. Hence, the only real tradeoff is how large we allow the weights to grow (through the regularization strength λ).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM5MDk2MDI1MywtOTM2MDg1MDA3LC00Nj
c4OTQxNDFdfQ==
-->