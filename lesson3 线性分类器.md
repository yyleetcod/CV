To develop a more powerful approach to image classification that will have two major components: a **score function** that maps the raw data to class scores, and a **loss function** that quantifies the agreement between the predicted scores and the ground truth labels. We will then cast this as an optimization problem in which we will minimize the loss function with respect to the parameters of the score function.

可以认为线性分类器在做一个模板匹配。只关心影响最后输出的像素点；或者认为是高维空间上的一个分类边界

线性分类器没有能力同时获取多个模型，所以如果模型中红色较多，模型就会偏红色
分类边界非线性时，线性分类器无法工作

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ2Nzg5NDE0MV19
-->