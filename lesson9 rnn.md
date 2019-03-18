反向传播过程需要不断乘以一个Whh矩阵，如果这个矩阵的特征值的最大值大于1，梯度爆炸；小于1，梯度消失
通过clipping（裁剪）控制梯度爆炸，LSTM控制梯度消失(同resnet相似的相加结构。如果不考虑忘记门，我们都是在c上相加地改变它，这会使得梯度反向传播时没有阻碍。考虑忘记门时，如果忘记门打开（f=0），会使得梯度被截断无法继续向后传。我们可以初始化偏置，使得忘记门失效，然后让网络自己去学习什么时候打开忘记门)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMxMzUyODI5MiwtMTIxODA4MzQ3NCwyMD
g3MjU4OSwyNTk3MjkyNjBdfQ==
-->