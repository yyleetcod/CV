# 参数更新
Loss function has high condition number: ratio of largest to smallest singular value of the Hessian matrix is large
这时候sgd就有问题
1. SGD
![enter image description here](https://lh3.googleusercontent.com/T3s80zaaw2El9QaKH_NO2Uw0OWnRQ0GyVzp3NV-k02qUllNdF7jwrj7YwEHxXlyUBhAvvcSggL9b)
2. Momentum Update
$v=m_uv-lr*grad$
$w+=v$
这个更新过程可以认为梯度是加速度，$m_u$是摩擦力带来的衰减，取0~1.
这个更新会使得陡峭方向会阻尼衰减，平缓方向不断前进，这样就不会振荡收敛。
![enter image description here](https://lh3.googleusercontent.com/rTxH6dzSQavHOqCoptHfzN9qJpE3NkClp5oiWZDcPuJI8G5T3-5BXW_z08byXu7fmrhzRC-lwSCc)
而且sgd无法逃脱鞍点和局部最小值
3. Nesterov Momentum Update（Nesterov Accelerated Update）
![enter image description here](https://lh3.googleusercontent.com/aTYeUwi5asHFkrI2qns-yyNGnJCOcD6cXDBI3UqwbnPK4mfnSn7CPTiLNR08coa6PqYpNf6I8X7V)
算梯度的时候直接到下一个位置算
![enter image description here](https://lh3.googleusercontent.com/6fvYdN_jwtRZ4YD0A0-oBBR2hifcqJMMwcbaIcZ-4NJ3Xv3UuU6IJaPhNk6jS0xQXmWgBH5F9LEm)
越过最优点的幅度不如Momentum Update那么大，每次都往前预测多考虑一点，多次累加起来的效果会好很多
在很大的神经网络中，最大和最小的局部最小值之间其实相差不大。局部最小值很差这种问题一般只出现在小的神经网络中

# AdaGrad Update
![enter image description here](https://lh3.googleusercontent.com/IdpoPmelNjr3ZwXu_iUhN1h_YGM-hxeLn3lZrw8WwtlvUuzJhpurRGjvW0TTQv0nNTeQNMd8hAxg)
1e-7是一个超参数，平滑因子，防止分母为0
这样做可以使梯度大的方向由于被放缩而减小。确保梯度大和梯度小的方向补偿相当
但是该方法时间一长就会停止更新，为了解决可以引入衰减率

# RMSProp Update
![enter image description here](https://lh3.googleusercontent.com/tN4vFsgluaYAnF_oJ7IagJ--sSKO5ksR2XOS20MdOUw1RM4f7UGYs2jKGqZsoAF6w-hUACW6xNfQ)
具有AdaGrad Update的优点，又不会停止训练
RMSProp Update只考虑最近几个的梯度平方和

# Adam
rmsprop和momentum的结合：
![enter image description here](https://lh3.googleusercontent.com/OgbL7hjb4tqLjRsa7Ip68WY7aNO27ng9VqW67j1eKyDaFFOg0QdAlKCQ8phwUmwxxT9S34qFRD-K)
$\beta_1=0.9$,$\beta_2=0.995$是一组鲁棒性不错的参数
![enter image description here](https://lh3.googleusercontent.com/SXnOh0LlS3IhSikyCpN494IobOgKhryGka9H__-QiQ5XnVcbZ18Wlao1pvyVEUzOJjvYA71W8rfG)
引入偏置是因为m、v一开始被设置成0，这样可以让他们更快地warm up
Adam with beta1 = 0.9,  beta2 = 0.999, and learning_rate = 1e-3 or 5e-4 is a great starting point for many models!

# 学习率
一开始采用大学习率，后来采用小学习率
![enter image description here](https://lh3.googleusercontent.com/Z5koJlN6VPTZcOHDwDApT-MGigdDBwJdb1Mdl0qz8qTZQG2hRJoqXUXS-xAUAT462BNddBk2CnW_)

# 二阶优化方法
![enter image description here](https://lh3.googleusercontent.com/ZHpm8A-PeIRKSWM089S1xwiG2p2eHnciiVmXpN98SwC65U-cSdSioDfpioVQHuJa1xWQE5psKzHd)
![enter image description here](https://lh3.googleusercontent.com/fJUnC7eXJ4JOVQurGFD-NYx1aXIAVKd2NSrOKUDFxTW6PXHH9Vk9S1I2WutxYuC1Wmevi4uAV-TP)
比如牛顿法。
![enter image description here](https://lh3.googleusercontent.com/TBM6EwMfNtPmG3eYtuDRgdCN-Lhn-vI1rEv-ukYtDP1e3HqCT2CdUIUcj1hGCdcXfhm31R5kIkJ5)
好处是收敛更快，超参数更少，不用学习率;In particular, multiplying by the inverse Hessian leads the optimization to take more aggressive steps in directions of shallow curvature and shorter steps in directions of steep curvature. Note, crucially, the absence of any learning rate hyperparameters in the update formula, which the proponents of these methods cite this as a large advantage over first-order methods.
坏处是二阶导矩阵太大了，计算不出来
Hessian has O(N^2) elements 
Inverting takes O(N^3)  
N = (Tens or Hundreds of) Millions
![enter image description here](https://lh3.googleusercontent.com/lI33lSsY7vnwUKsThyXnOv6_82sKLeOzHMMkQrzNdRQbTi6mYfQaFhbXCOnQRVF_XYjwKfsFTJZa)
![enter image description here](https://lh3.googleusercontent.com/lrpy-f84tDhruseIoIQE5iyXWdw9uQmc6e4zDrXR6d9xS4ZlkHngCCzohhwEzCWUyjBdPau0CQBw)
![enter image description here](https://lh3.googleusercontent.com/jaWnZjaOfGf4ZKB1AiL656uwFOnAC7OPdfK7MQ4NsV0caIEnhzW7jKJNddVxVInTriqX0GQI0YZz)
![enter image description here](https://lh3.googleusercontent.com/dGsxwaBkHSub_Gl8T9rNeTy4nxz70QA1_1CreG7-2XbYkFBEzm-pYEF7y3U9ymMzI0O64oQpXnUd)

# 集成模型
![enter image description here](https://lh3.googleusercontent.com/kZAuhvyNiNmlHz2gQp8Lf5U3RLMDfSdRRYPfb_X-gDgEmO1EnMz6F7ku8IvLGmzqWN55QuwN9qpO)
![enter image description here](https://lh3.googleusercontent.com/_1e_G6EWnIzOy8TYSMfvzuYTTTeVJcodtM_x8Fpl3-GXvqyX03z3S_n3eALB93i7sW1BuvwBLjVB)
![enter image description here](https://lh3.googleusercontent.com/s-SgtKtZePAsw6y1KkzhE2XOTJ_mvkaE_7teDiezCbrgb6bw89vpXwBo5vNS8xND4UYnQwccth1J)
In practice, one reliable approach to improving the performance of Neural Networks by a few percent is to train multiple independent models, and at test time average their predictions. As the number of models in the ensemble increases, the performance typically monotonically improves (though with diminishing returns). Moreover, the improvements are more dramatic with higher model variety in the ensemble. There are a few approaches to forming an ensemble:
-   **Same model, different initializations**. Use cross-validation to determine the best hyperparameters, then train multiple models with the best set of hyperparameters but with different random initialization. The danger with this approach is that the variety is only due to initialization.
-   **Top models discovered during cross-validation**. Use cross-validation to determine the best hyperparameters, then pick the top few (e.g. 10) models to form the ensemble. This improves the variety of the ensemble but has the danger of including suboptimal models. In practice, this can be easier to perform since it doesn’t require additional retraining of models after cross-validation
-   **Different checkpoints of a single model**. If training is very expensive, some people have had limited success in taking different checkpoints of a single network over time (for example after every epoch) and using those to form an ensemble. Clearly, this suffers from some lack of variety, but can still work reasonably well in practice. The advantage of this approach is that is very cheap.
-   **Running average of parameters during training**. Related to the last point, a cheap way of almost always getting an extra percent or two of performance is to maintain a second copy of the network’s weights in memory that maintains an exponentially decaying sum of previous weights during training. This way you’re averaging the state of the network over last several iterations. You will find that this “smoothed” version of the weights over last few steps almost always achieves better validation error. The rough intuition to have in mind is that the objective is bowl-shaped and your network is jumping around the mode, so the average has a higher chance of being somewhere nearer the mode.
One disadvantage of model ensembles is that they take longer to evaluate on test example. An interested reader may find the recent work from Geoff Hinton on “Dark Knowledge” inspiring, where the idea is to “distill” a good ensemble back to a single model by incorporating the ensemble log likelihoods into a modified objective.

# Dropout
![enter image description here](https://lh3.googleusercontent.com/epgG28uUD40Hm1VAASJbxW1B2fqp5cr8LeiJh5JppIvFP7kacME-R32HF2wsoGqHU_WXsjwWsTno)
![enter image description here](https://lh3.googleusercontent.com/ryQbkcyHJ42fBRt0l5KyPzvchslWmIG3L51DaqGshUcd4sQ6SwXPfbmnhNMAuz5p0-QLuL80fNBe)
前向传播和反向传播要同时进行dropout（或都不进行）
一般都保留50%，想要更强的正则化，就多失活一些神经元
有效防止过拟合：每次只是用一半的节点，网络中设计的变量数减少，模型表达能力下降，减少过拟合的几率。
可以这么理解dropout：
1. Forces the network to have a redundant representation; Prevents co-adaptation of features。由于过程中每个特征都可能被随机的失活，所以dropout会让模型更倾向于去依赖更多的特征，来提高自己的准确率
2. 是很多小模型集成的大模型。失活的神经元在反向传播的过程中梯度为0，它之前与它相连的权重都不会被更新。这就相当于在一次中随机选取了一个子模型的权重拉进行更新。每个随意失活后的模型都是一个子模型，都只会被一个数据训练（失活是随机的，刚好这个数据遇到了这个失活的模式）。多次循环之后我们会用相同的数据来训练有着共同参数的不同模型
![enter image description here](https://lh3.googleusercontent.com/vdAQz4tet3npSRW0IW4f4ebEiKZPd5-0xVUm4Sjauxji-i1JsuYqBtb6U-ZPTVP9kOi0-BYp-9iq)
测试时：
![enter image description here](https://lh3.googleusercontent.com/ZEnlqFWeWGlm-GuvnCEsKrn1MwSh59fIWbrZHTfcQM5rftpmy_mgRgR30iDeRYXbi8pJs19ifMEc)
![enter image description here](https://lh3.googleusercontent.com/is8Qe3ysCiRQtX1hlH6qlj4iCIFws-v7LS_NRk_hnRQtoxMJZ1ezk-naKSvbpTSqwiVEopZyFXOF)
上述这种方法太过耗时
![enter image description here](https://lh3.googleusercontent.com/dwZZ7xLqsCa_-Rz0ghVrVTq3RA7d0aucsr_PDBr4YR1FNXfHcfIV8jRBgsOiuGvRWSFOqiVfBipj)
![enter image description here](https://lh3.googleusercontent.com/VIKt5kopcHSZAzurvMs--j8mB0nq_KZBUNjOFTleTL_MKDo-ZfYy9TFc9z6bj3Mau5EX6F4XEVKf)
![enter image description here](https://lh3.googleusercontent.com/pOva4KD4Euw3X9bd-B0BsUb8pkBQQuVakYzUqc5NvASGJsb3UuP3y2LIw02vdsD52JwBrRIND_wo)
需要对激活函数的输出进行一个补偿（乘上p）
![enter image description here](https://lh3.googleusercontent.com/3yiWOu0EbHxiIr_4yGf4AhFGEy-SjSefwp71W1qGxfo3xj8BceaxzB55zfxmmsV57bAOGeacMYbe)
或者也可以在训练的时候修改，测试的时候不变：
![enter image description here](https://lh3.googleusercontent.com/jEchkeoejEuB8MCYqh-qplVHyTObOedIHlAiAk19FCLc7F2DzFiIKCfQUUz_JCrD55W3QNVyl3Te)
这个p的放缩只是一种近似，因为激活函数的非线性。而且是一种期望的概念

# Regularization
![enter image description here](https://lh3.googleusercontent.com/q1XI_oQEwtlsGFs4BIFuKqS3WVPTw0q1DR0k5Ro5m1_TVQyfes1qnj4rY2qIZQ9Ohp1BmevJr-bV)

# Data Augmentation
![enter image description here](https://lh3.googleusercontent.com/8c6mi53KzdXX8_FI2scK8NVrAlX__5GT7gvo4do3L7CkhEAHSYLWz8GngbY4TzYrUVl4c75M2hke)
![enter image description here](https://lh3.googleusercontent.com/lb1-t1Ja3oJW3wbbIffLHh_yRbOHeCaMQQNbyd8NMfS3063VgL7HRG9KNRhkR6TBn-M5BormcyjV)

# Gradient Checking

# Transfer Learning
![enter image description here](https://lh3.googleusercontent.com/Av4e4aBeCT_mfBygZ-BYQkDoMyNnykw08vJcrKE4rUlwgVj83CM1WdKKrlVX-bKO7VBEA45zcIQA)
![enter image description here](https://lh3.googleusercontent.com/Vb7EIkwh0Lm5HTB8wTQhGh6ESWvGtoEEfkzoDGaXUWpfmGtiPYTPu1wEW70-MMVwPZPjFbwemsvN)

# 选择大网络
如果数据不够复杂以防止过度拟合，似乎可以优先选择较小的神经网络。 然而，这是不正确的 - 还有许多其他首选方法可以防止神经网络中的过度拟合，我们将在后面讨论（例如L2正则化，丢失，输入噪声）。 在实践中，使用这些方法来控制过度拟合而不是神经元的数量总是更好。
这背后的微妙原因是较小的网络难以使用诸如梯度下降之类的局部方法进行训练：很明显，它们的损失函数具有相对较少的局部最小值，但事实证明，这些最小值中的许多更易于收敛，并且他们很糟糕（即损失很大）。相反，较大的神经网络包含明显更多的局部最小值，但这些最小值在实际损失方面变得更好。由于神经网络是非凸的，因此很难在数学上研究这些特性，但是已经进行了一些尝试来理解这些目标函数，例如，在最近的一篇论文“多层网络的损失表面”中。在实践中，你发现如果你训练一个小网络，最后的损失可以显示出很大的差异 - 在某些情况下你会很幸运并收敛到一个好地方但在某些情况下你会被困在一个坏的极小。另一方面，如果你训练一个大型网络，你将开始找到许多不同的解决方案，但最终实现的损失的差异会小得多。换句话说，所有解决方案都同样好，并且更少依赖于随机初始化的运气。
我们讨论了这样一个事实，即较大的网络总是比较小的网络更好地工作，但是它们的更高的模型容量必须通过更强的正规化（例如更高的权重衰减）来适当地解决，否则它们可能过拟合。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MzAyMjQxNjIsMTQ5MjI4MjE3MCwxMj
UwMzQ0MDgyLDM4ODA3MDMxMCw3NzI3MDU2NzgsLTExOTY5MjA1
MzEsOTgzMzkwNjY4LC0xODczMzIwNjksOTI3MDY2OTgsLTExMj
kwNjU0MDgsLTI1NTg4ODgyOSw3NzM1OTUyNDEsOTk0MDc1OTI2
LC03MzkzNzkxNzgsMTk0NjE3MDQ5MCwtNTYyNDAwMzEyLC0yMD
c2MzM4MTAwLDY5NTUzMDU4MiwtNDY5MDQwMDgwLDQ2MDE0Njgz
XX0=
-->