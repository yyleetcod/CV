# localization

一张图只有一个类别，一个位置 
使用转移学习，去掉最后一层fc，训练回归任务，回归四个点，左上角（x，y）和宽w，高h
 不定类回归：不管图片是什么类别，都返回4个回归数值
 定类回归：一共c个类别，返回c个类别的分类得分，每个类别还有对应的4个数值来表示localization
 对于输入大小不一致的情况：1.使用滑动窗口 2.把fc层换成conv层，可以高效解决，替代滑动窗口的方法。然后再merge各个窗口回归出的box

# detection

1. 使用分类的方法：不同位置，不同大小的滑动窗口，去判断是否是某个类别

2. region proposal：例如选择性搜索：从像素出发，把具有相似颜色和纹理的相邻像素合并，得到区域。不断合并，得到更大的相似区域。然后把不同尺度的块状区域转化成框。
![enter image description here](https://lh3.googleusercontent.com/X2HKcAjyF2TrcWrgXxqhpGbkr_JckqD7XaDZIB4XAnx7zDkRypE6iMemiutQTUxKmC8Ot2vJOeFZ)

使用预训练好的模型，对region proposal提取出的box进行svm分类任务的训练，以及回归任务（对box进行微调）
具体步骤：1. 使用region proposal提取出一张图片中可能的区域（一张~2000个） 2.使用预训练模型进行finetune 3.把提取出的box缩放到cnn输入大小，过cnn。把每张图片经过conv没过fc层提取出的特征保存下来。 4.对每张保存的特征，用来训练er'fen
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4OTY2MjA4MTIsMTU0NzkyNDI2MSwxNT
U2OTI4OTAsOTgzOTIxMjM2XX0=
-->