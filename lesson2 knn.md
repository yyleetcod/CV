- knn: 训练过程廉价，测试过程昂贵
- cnn: 训练过程昂贵，测试过程廉价

**不使用knn的原因：**

- 测试时间代价大
- 不直观。不同的图片千差万别，但是可能和测试图片都有着相同的欧几里得距离

可以认为线性分类器在做一个模板匹配。只关心影响最后输出的像素点；或者认为是高维空间上的一个分类边界
线性分类器没有能力同时获取多个模型，所以如果模型中红色较多，模型就会偏红色
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTA5OTc2NzksMTAyODEyODEyNiwyOTYxND
U3MTcsLTEwOTIzMzY4NTUsODM5Mzk2Njg2XX0=
-->