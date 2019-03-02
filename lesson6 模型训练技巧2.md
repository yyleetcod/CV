# 参数更新
1. SGD
![enter image description here](https://lh3.googleusercontent.com/T3s80zaaw2El9QaKH_NO2Uw0OWnRQ0GyVzp3NV-k02qUllNdF7jwrj7YwEHxXlyUBhAvvcSggL9b)
2. Momentum Update
$v=m_uv-lr*grad$
$w+=v$
这个更新过程可以认为梯度是加速度，$m_u$是摩擦力带来的衰减，取0~1.
这个更新会使得陡峭方向会阻尼衰减，平缓方向不断前进，这样就不会振荡收敛。
![enter image description here](https://lh3.googleusercontent.com/rTxH6dzSQavHOqCoptHfzN9qJpE3NkClp5oiWZDcPuJI8G5T3-5BXW_z08byXu7fmrhzRC-lwSCc)
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjI3NDUwNDM5LC0xMTI1MzkzMjIxLDY1OD
AyMTM2NCw2NjExMDUxNTQsLTIwOTkxMDkwODAsLTczOTk5Njgw
Myw2NTUxMTk0MTMsLTE0MzE5MTM0ODcsMTY5Njk3MTUzOF19
-->