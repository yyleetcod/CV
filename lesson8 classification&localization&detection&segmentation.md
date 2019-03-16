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

R-CNN：使用预训练好的模型，对region proposal提取出的box进行svm分类任务的训练，以及回归任务（对box进行微调）
具体步骤：1. 使用region proposal提取出一张图片中可能的区域（一张~2000个） 2.使用预训练模型进行finetune 3.把提取出的box缩放到cnn输入大小，过cnn。把每张图片经过conv没过fc层提取出的特征保存下来。 4.对每张保存的特征，用来对每个类别训练svm二分类任务（是否是猫，是否是狗） 5.region proposal提出的box进行回归，进行微调（dx,dy,dw,dh）

评估标准：mAP（mean average precision）
![enter image description here](https://lh3.googleusercontent.com/YYiQ7yLYPH-uXht8H6TE_abPYNKL7jeAzU2eMA4qtc1YCq9nDUm0Lqr_jVnW8vbP9F17jaLJ16gw)

![enter image description here](https://lh3.googleusercontent.com/vwN8mMTE-4ScfpSaT-lLjM6EHAl2iSqGAqiZ585Sc2xfiIvv-HG9twDK2aLIxdestZGCBu6fk61V)
![enter image description here](https://lh3.googleusercontent.com/U1AqHtidQSl-qK7pgsOXxqBE4I0kUKSbO_57TLrNtinUuelPZ2A_xFai3uhq0OwrdERpZo8azaD9)
![enter image description here](https://lh3.googleusercontent.com/DacJRt_9bcyRDeVC3gUaH_77gPCrJVyvBcwXymEJFMShni_Sqz9WYHS2mH3fHnPSAV16FWvd2FRL)
![enter image description here](https://lh3.googleusercontent.com/CimXEhEV1P_rbw4XKEWUe9RWPK3cmeoOuv2b1VghLmpxgulZtH4CaN7U0fZs7A4uDgwVk6TZ-Bd9)
![enter image description here](https://lh3.googleusercontent.com/U_NVaO2zv5BTP3bRkuWOUpI5v7A4VSa2F3Fa8e0O0BScuaXI2_zikLKNsKnnbgC0DuAFMndeY_HD)
![enter image description here](https://lh3.googleusercontent.com/ruUedfl55pF8JRiGXfTry8Zgf942cNv1i1MGYoYFyeHQ1sIgVnVLf25fHOeaOIX2eZgNeHTiRkdP)
![enter image description here](https://lh3.googleusercontent.com/prIJKNzla6KFnKxX-1YBXjQ8CXqP_uUSy3EsL_TMFENYJ7p0ep1rZae0EIP1mTO4cLSWMrATW_wU)
![enter image description here](https://lh3.googleusercontent.com/imon3oXdXMrjDSolZpjI_e60Yl_t-bRVJ7NA7TaWW9j1apVHsflWLOkzGcueVI05GJJnNehyPCiA)
![enter image description here](https://lh3.googleusercontent.com/3Qk1BovqAg335UZ8CKCP1izwvC1stJk5Pt1JGD505nob7wHSTP7br4I8FXUp5uFcTVxIBZ8_3Nm5)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyMzQ5Mzk0Ml19
-->