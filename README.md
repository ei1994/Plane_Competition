# Plane_Competition
* [中科院航天星图杯比赛官网](http://sw.chreos.org/Home)
* ["航天星图杯”高分软件大赛获奖名单](http://eece.ucas.ac.cn/index.php/zh-cn/2014-06-13-06-44-38/1401-2017-09-14-01-14-10)
* 参赛算法是在[pjreddie/darknet](https://github.com/pjreddie/darknet)代码的基础上进行改进调试。 改进主要是针对比赛数据的特性，集中在对数据进行预处理及预测结果的后处理上。  
## 1、比赛总结
**时间：** 2017.06 — 2017.09
* （1）**训练集的制作：** 组委会提供的图片数据集（10张高分辨率可见光图片）尺寸大（如：5000* 5000），其中的飞机目标小且分布稀疏，因此将数据集图片切割为1000* 1000的图像块，只取包含飞机目标的图像块，利用标图软件labelImg，人工标注得到图像中飞机目标对应的类别、坐标位置等信息数据，并将其制作成voc格式的训练数据集。
* （2）**数据预处理：** 由于训练数据少且目标分别稀疏，所以在训练时加入了数据扩充处理：左右上下翻转、旋转角度、色度亮度变化等。
* （3）**网络训练：** 基础网络选择darknet，平方误差损失，batchsize是32，优化器选择sgd，学习率0.001，anchor box是通过聚类得到5个（长宽比接近1）。
* （4）**预测及后处理：** 测试时是将输入图像（如大小为8000 * 8000）切块，切块方法是：通过将原图宽、高分别除以1000，向上取整得到分块的个数；用原图宽、高除以分块个数得到每块的宽、高值，再依此进行切分。将切分后的图像块依次输入网络中进行计算预测，得到的目标坐标再通过坐标变换映射回原图中，并在原图中标出对应的目标框位置，同时将数据写入xml文件中。   
另外，对于在切分图像块时，位于切分边界上的飞机目标，会被分割为两部分，这在后期网络预测时也能够被检测出来，但如果直接将其位置信息映射回原图中会出现一个飞机目标上画出两个框的现象。针对此问题，我们通过观察分析得知，图像中的飞机目标框长宽比接近1。根据这个特性，对检测出来的飞机目标框进行非极大值抑制，即将分割为两部分的目标框合成一个，这在一定程度上提升了目标检测的准确率。   
* （5）**基本流程图：** 
![算法展示](/images/软件算法说明.PNG "飞机识别")
## 2、比赛排名：
![航天星图杯优秀奖](/images/高分软件大赛优秀奖.jpg "航天星图杯")

