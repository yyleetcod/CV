反向传播过程需要不断乘以一个Whh矩阵，如果这个矩阵的特征值的最大值大于1，梯度爆炸；小于1，梯度消失
通过clipping（裁剪）控制梯度爆炸，LSTM控制梯度消失(同resnet相似的相加结构)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA4NzI1ODksMjU5NzI5MjYwXX0=
-->