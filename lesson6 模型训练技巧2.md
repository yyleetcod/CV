# 参数更新
1. SGD
![enter image description here](https://lh3.googleusercontent.com/T3s80zaaw2El9QaKH_NO2Uw0OWnRQ0GyVzp3NV-k02qUllNdF7jwrj7YwEHxXlyUBhAvvcSggL9b)
2. Momentum Update
$v=m_uv-lr*grad$
$w+=v$
这个更新过程可以认为梯度是加速度，$m_u$是摩擦力带来的衰减，取0~1.
这个更新会使得陡峭方向会阻尼衰减，平缓方向不断前进，这样就不会振荡收敛。
![enter image description here](https://lh3.googleusercontent.com/rTxH6dzSQavHOqCoptHfzN9qJpE3NkClp5oiWZDcPuJI8G5T3-5BXW_z08byXu7fmrhzRC-lwSCc)
3. Nesterov Momentum Update
![enter image description here](https://lh3.googleusercontent.com/aTYeUwi5asHFkrI2qns-yyNGnJCOcD6cXDBI3UqwbnPK4mfnSn7CPTiLNR08coa6PqYpNf6I8X7V)
算梯度的时候之间
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ4NjU0MTQwLC0xNDMxOTEzNDg3LDE2OT
Y5NzE1MzhdfQ==
-->