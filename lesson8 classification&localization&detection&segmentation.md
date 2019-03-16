# localization
一张图只有一个类别，一个位置 
使用转移学习，去掉最后一层fc，训练回归任务，回归四个点，左上角（x，y）和宽w，高h
 不定类回归：不管图片是什么类别，都返回4个回归数值
 定类回归：一共c个类别，返回c个类别的分类得分，每个类别还有对应的4个数值来表示localization
 对于输入大小不一致的情况：1.使用滑动窗口 2.把fc层换成conv层，可以高效解决，替代滑动窗口的方法。然后再merge各个窗口回归出的box
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg1NzkwODE2NF19
-->