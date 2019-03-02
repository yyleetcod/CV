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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTczOTk5NjgwMyw2NTUxMTk0MTMsLTE0Mz
E5MTM0ODcsMTY5Njk3MTUzOF19
-->