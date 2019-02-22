- knn: 训练过程廉价，测试过程昂贵
- cnn: 训练过程昂贵，测试过程廉价

**不使用knn的原因：**

- 测试时间代价大
- 不直观。不同的图片千差万别，但是可能和测试图片都有着相同的欧几里得距离

可以认为线性分类器在做一个模板匹配。只关心影响最后输出的像素点
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjk2MTQ1NzE3LC0xMDkyMzM2ODU1LDgzOT
M5NjY4Nl19
-->