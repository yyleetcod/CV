# 激活函数

如果没有激活函数，网络的分类能力基本上类似于线性分类器
**sigmoid问题：**
1. 饱和区域梯度为0
2. sigmoid输出恒为正。恒为正的数据作为输入数据，将会导致梯度要么都为正，要么都为负。因此w只能通向变化。如果要达到w的最优值，那么所走的路径是很曲折的（比如w1，w2变化方向不一致，要达到的话不能走斜线，只能分两次，竖直和水平各走一次），因此收敛速度很慢。所以我们更希望得到关于原点中心的输入数据和关于原点中心对此的输出
3. 指数运算很耗时

**tanh**
- 输出在[-1，1]之间，解决了sigmoid输出不关于原点对称的问题，但是仍然存在数据饱和的问题

**ReLu**
- 优点：在>0的区域不会饱和，计算很快，而且收敛也比sigmoid和tanh函数快（大约快6倍）
- 缺点：输出不是关于原点对称的，而且如果每个输入数据都处于非激活区域，计算得到梯度为0，权重不会被更新（Dead ReLu），神经网络不会训练。可能原因是初始化不合理或者学习率更大。Dead ReLu不可逆转。一般来说把偏置值初始化为比较小的正值，比如0.01，这会使网络更有可能输出正值，避免Dead ReLu

**Leaky ReLu**
- 优点：不会饱和，计算效率高，收敛快，不会有Dead ReLu的问题
- 缺点：不是关于原点对称

**Exponential Linear Units**
- 优点：不会饱和，收敛快，没有Dead ReLu，输出更接近零均值；负饱和区域，和Leaky ReLu相比，增加了噪声鲁棒性
- 缺点：需要指数运算，计算代价高

**Maxout**
- 优点：不会饱和，没有Dead ReLu，是ReLu和Leaky ReLu的一种推广
- 缺点：参数数量翻倍

**激活函数的选择和优化方式是有关的**
建议：
1. 使用ReLu，小心地设置学习率
2. 尝试Leaky ReLu、ELU、Maxout
3. 尝试tanh但不要期望太高
4. 不要使用sigmoid（除了在rnn中），因为tanh可以替代它

# 数据预处理

1. 零均值。有两种方式，减去每个像素均值（32x32x3），或者减去每个通道均值（3）
2. 归一化（在cv中不常见，因为像素本身取值是0-255）
3. PCA去除数据相关性，使得协方差矩阵为对角矩阵。再进行白化，使得协方差矩阵为单位矩阵（机器学习中常见，cv不常见，因为图像太多维，协方差矩阵太大）

# 权重初始化

1. 如果随机乘一样的值，那么所有神经元是对称的，网络无法工作。
一般用很小的随机值进行初始化（比如从标准差为0.01的零均值高斯随机变量中产生）。这种方式在层数较少的网络里工作不错，但是层数多的时候，可能导致每层的输出数据分布不同的问题。层数多了之后，标准差越来越小，最终为0，这是因为W是小量，多层之后导致输出趋近0。
如果输出是小量，反向传播时，该处的梯度也是小量（~WX，梯度消失），这样导致权重几乎不变。一层层反向传播就更小了，梯度消失
**输出为0，权重不更新**
如果使用标准差为1的零均值高斯随机变量产生权重，可能导致饱和，反向传播梯度为0。
激活函数为tanh时，一种有效的初始化方式：权重标准差~1/sqrt(num_in)，这样有助于每层的输出标准差为1。
但是上述方式在ReLu中不奏效，因为ReLu让方差减半了（把<0的一办置为0）。因此权重标准差~1/sqrt(num_in/2)。没有这个2，输出分布就会以指数级别坍缩

2. Batch Normalization：对每个维度的特征单独做归一化，来获得单位高斯分布的变量。该层加在全连接层（或者卷积层）和非线性激活层（如tanh）之间。但是问题时tanh的输入一定要是单位高斯分布的吗？可以这么解决（加入偏移作为参数，如果不需要batch normalization，参数最后会学习让这个层作用消失）：
![enter image description here](https://lh3.googleusercontent.com/rmswx2YOnu-Gfcgl_GY0VdN_v0tWX16Yaj7h3VIhZn9R-eOGeAM9gOhN2RNd6ggnc1XaQik94_dR)
batch normalization好处：
![enter image description here](https://lh3.googleusercontent.com/nJ0f4digtFYPbUnKOPP3LVt5wBVYidRofzIty8rDNs4jpthgmxfpO56OBLA4Oxac8MUrVDnxkk4m)
BN层把不同样本联系在了一起，得到了当前batch的样本表示空间，有不错的正则化效果
![enter image description here](https://lh3.googleusercontent.com/R8Kb-BIB66RE9mWGFzX10b3aIBJhmG2BPt1HDpJR1AhQlfg21weq946LkMkWEQM7TyIh_vtODHpW)
BN层会延长模型的训练时间，最大30%

# 超参数
**loss not going down**:learning rate too low
**loss exploding**:learning rate too high

多做几次超参数选择，以免落入局部最小值

学习速率和正则化参数要在对数空间中取样，因为这两个参数在反向传播的时候是相乘的。如果像1、2、3、...、n这样取样，里面很多都是处于效果不好的区域
1. 在一定区域内随机取超参数
2. 在一定区域内网格化取超参数

随机取效果更好。因为大部分情况，一个超参数会比另一个超参数的重要性大很多。而且可能只是在一个很小的区域内取值，模型会有比较好的效果。这样随机取样，取到的值就更多：
![enter image description here](https://lh3.googleusercontent.com/7uWQEn4R44GHoYClTG1emTsfDDig7-nvebcwROzCrrPoAs7JMEY-jRHgnK3ZIJwEMVVGC7unyOOU)
![enter image description here](https://lh3.googleusercontent.com/ffpCMzc_N0WFBw-m9bGgWXwIOIedHUKH0saZngfqMAcbbeXiNWsk1YJcWv52rGL336ROGuwnFjxg)
一个比较好的更新/权重~1e-3

# Summary

![enter image description here](https://lh3.googleusercontent.com/ruqXTdCdpZb76MmSh6yssI3AXOGkWrrlFVPLaf7HzIWRUXCaVRg5R-O1EIiPnFMChK3TTaQiOTm-)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTMxNjAyODgyLC0xMTc5MTM1MjM5LDE5Nj
QzMDE5MzQsLTE5NTgwMDc2MDUsMTI5Njc3NTYyNiwxMjc1MTk4
OTk4LC0xNjk1Mzg1Mjc0LC0xNDc1NjAwODQ2LDIwNjc2Nzc3OD
QsMjE1MzUyNzI2LC0xNzIxNjQxOTcxLC0xODEyNzU0MDYsMTg3
MjM0MTEyNCwxMDYwNTIwOTcxLDIwMDM4OTMyNzRdfQ==
-->