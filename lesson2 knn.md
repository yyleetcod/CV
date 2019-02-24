- knn: 训练过程廉价，测试过程昂贵。而且需要存储所有的训练集
- cnn: 训练过程昂贵，测试过程廉价

**不使用knn的原因：**

- 测试时间代价大
- 不直观。不同的图片千差万别，但是可能和测试图片都有着相同的欧几里得距离。在原始像素值上使用L1或L2距离是不够的，因为距离与图像的背景和颜色分布的相关性比与其语义内容的相关性更强

可以认为线性分类器在做一个模板匹配。只关心影响最后输出的像素点；或者认为是高维空间上的一个分类边界

线性分类器没有能力同时获取多个模型，所以如果模型中红色较多，模型就会偏红色
分类边界非线性时，线性分类器无法工作

knn的步骤：

1.  Preprocess your data: Normalize the features in your data (e.g. one pixel in images) to have zero mean and unit variance. We will cover this in more detail in later sections, and chose not to cover data normalization in this section because pixels in images are usually homogeneous and do not exhibit widely different distributions, alleviating the need for data normalization.
2.  If your data is very high-dimensional, consider using a dimensionality reduction technique such as PCA ([wiki ref](http://en.wikipedia.org/wiki/Principal_component_analysis),  [CS229ref](http://cs229.stanford.edu/notes/cs229-notes10.pdf),  [blog ref](http://www.bigdataexaminer.com/understanding-dimensionality-reduction-principal-component-analysis-and-singular-value-decomposition/)) or even  [Random Projections](http://scikit-learn.org/stable/modules/random_projection.html).
3.  Split your training data randomly into train/val splits. As a rule of thumb, between 70-90% of your data usually goes to the train split. This setting depends on how many hyperparameters you have and how much of an influence you expect them to have. If there are many hyperparameters to estimate, you should err on the side of having larger validation set to estimate them effectively. If you are concerned about the size of your validation data, it is best to split the training data into folds and perform cross-validation. If you can afford the computational budget it is always safer to go with cross-validation (the more folds the better, but more expensive).
4.  Train and evaluate the kNN classifier on the validation data (for all folds, if doing cross-validation) for many choices of  **k**  (e.g. the more the better) and across different distance types (L1 and L2 are good candidates)
5.  If your kNN classifier is running too long, consider using an Approximate Nearest Neighbor library (e.g.  [FLANN](http://www.cs.ubc.ca/research/flann/)) to accelerate the retrieval (at cost of some accuracy).
6.  Take note of the hyperparameters that gave the best results. There is a question of whether you should use the full training set with the best hyperparameters, since the optimal hyperparameters might change if you were to fold the validation data into your training set (since the size of the data would be larger). In practice it is cleaner to not use the validation data in the final classifier and consider it to be  _burned_  on estimating the hyperparameters. Evaluate the best model on the test set. Report the test set accuracy and declare the result to be the performance of the kNN classifier on your data.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MTAwOTE1MTMsLTU3OTcyNzc5NSwtMj
A5NTEzOTYzNiw5MDU2MjIzNCw1MDk5NzY3OSwxMDI4MTI4MTI2
LDI5NjE0NTcxNywtMTA5MjMzNjg1NSw4MzkzOTY2ODZdfQ==
-->