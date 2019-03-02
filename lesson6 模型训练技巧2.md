# 参数更新
1. SGD
![enter image description here](https://lh3.googleusercontent.com/T3s80zaaw2El9QaKH_NO2Uw0OWnRQ0GyVzp3NV-k02qUllNdF7jwrj7YwEHxXlyUBhAvvcSggL9b)
2. Nesterov Momentum Update
$v=m_uv-lr*grad$
$w+=v$
这个更新过程可以认为梯度是加速度，$m_u$是摩擦力带来的衰减，取0~1.
这个更新会使得陡峭方向会阻尼衰减，平缓方向不断前进，这样就不会振荡收敛。
![enter image description here](https://lh3.googleusercontent.com/rTxH6dzSQavHOqCoptHfzN9qJpE3NkClp5oiWZDcPuJI8G5T3-5BXW_z08byXu7fmrhzRC-lwSCc)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzE5MTM0ODcsMTY5Njk3MTUzOF19
-->