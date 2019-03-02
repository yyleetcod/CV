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

这样做可以使梯度大的方向由于被放缩而减小。
但是该方法时间一长就会停止更新，为了解决可以引入衰减率

# RMSProp Update
![enter image description here](https://lh3.googleusercontent.com/tN4vFsgluaYAnF_oJ7IagJ--sSKO5ksR2XOS20MdOUw1RM4f7UGYs2jKGqZsoAF6w-hUACW6xNfQ)
具有AdaGrad Update的优点，又不会停止训练
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjIxMjY5MzcsLTIwOTkxMDkwODAsLTczOT
k5NjgwMyw2NTUxMTk0MTMsLTE0MzE5MTM0ODcsMTY5Njk3MTUz
OF19
-->