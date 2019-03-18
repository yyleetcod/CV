反向传播过程需要不断乘以一个Whh矩阵，如果这个矩阵的特征值的最大值大于1，梯度爆炸；小于1，梯度消失
通过clipping（裁剪）控制梯度爆炸，LSTM控制梯度消失(同resnet相似的相加结构。如果不考虑忘记门，我们都是在c上相加地改变它，这会使得梯度反向传播时没有阻碍。考虑忘记门时，如果忘记门打开（f=0），会使得梯度被截断无法继续向后传。我们可以初始化偏置，使得忘记门失效，然后让网络自己去学习什么时候打开忘记门)
![enter image description here](https://lh3.googleusercontent.com/AvUhDzou9RYMgAtVHhXrO9iGuU47XmuFoRXPADIsCeoeVwsAPR4YvVrx5YWieEKJWa0k8Vtew3G4)
![enter image description here](https://lh3.googleusercontent.com/UCK5bqmjKoInxc99Fsj_n9BPRiV-F6QNznZCl23E-l23AROV7EGgrjE1cizMdCtzCZwdsi4zP9cU)
![enter image description here](https://lh3.googleusercontent.com/O4LTwC97py3np5eyGzunCpWK6elphwmRG_-JLdXat6UwG-1fz2zLp7BAjm4XII-Uu5fxL3zUBQRV)
![enter image description here](https://lh3.googleusercontent.com/YfQuH_7THbrAGTDND80v5hrLKHjDLfuOX7AdyKC_SMdPUu7z2IqDzScoKCbiedySz6chYLlMuMno)
![enter image description here](https://lh3.googleusercontent.com/GKCTohJgBYEA8lK4X5w68D2OAcuSK-QD7AynDv_bA-V7gviqos5qEnrNieouM7uv5zAi8urWFtDm)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NTczODU2NzcsLTEyMTgwODM0NzQsMj
A4NzI1ODksMjU5NzI5MjYwXX0=
-->