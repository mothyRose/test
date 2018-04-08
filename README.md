
该程序是为了将jpeg格式的图片转换成NPZ格式的数据供模型训练时使用(类似于mnist.npz, npz中含有四个array变量: x_train,y_train,x_test,y_test)。

在samples文件夹下提供了样例数据(来源自CSDN博客：https://blog.csdn.net/zh_jnu/article/details/54342856)。

deepforest的部分来自于：https://github.com/kingfengji/gcForest
						http://lamda.nju.edu.cn/code_gcForest.ashx
(如有侵权，请联系我删除)


使用流程：

1.将数据图片分类，每类图片都放至同一个文件夹下，再将这些文件夹放至同一个目录下。（例子见samples目录下的放置方法。）

2.将每个类文件夹内的文件重新命名，命名为 0.JPEG, 1.JPEG, 2.JPEG, ... , N-1.JPEG。（此步骤可借助于samples目录下的rename.py)

3.对genNPZ.py中前面类的名字（classes）、根目录(rootpath)、想要把目标图片统一转换到的图片宽度(width)、高度(height)进行设置。(在样例中，rootpath='\\samples\\', classes=('bus', 'dinasour', 'elephant', 'flower', 'horse'), width = 64, height = 64)。

4. 调用genNPZ.py中的 gen_dataset(n_train, n_test) （或者 gen_3chdataset(n_train, n_test) 用于生成具有三个通道,即保留图像色彩信息的数据）。 其中，n_train为训练数据的数量， n_test为测试数据的数量， n_class为待分类的类的数量。 例如，gen_dataset(80, 20), 会将每个类文件夹下的 0.JPEG - 79.JPEG 用作训练（对应于data.npz中的x_train, y_train）， 80.JPEG - 99.JPEG 用作测试对应于data.npz中的x_test, y_test)。

5.该程序会在相同目录下生成一个data.npz。

6.利用相应程序使用该data.npz文件。为了方便，直接将DeepForest文件也放在这里了。在原demo_mnist.py的基础上将第62行（原文件的第60行）的mnist.load_data()替换为自己编写的get_data(), 该函数在readNPZ.py中，使用时将其导入即可。（(X_train, y_train), (X_test, y_test) = get_data()#mnist.load_data()）。 再将config中的"n_classes"修改为对应数值即可运行（在sample中n_classes应该为5）。


各部分说明：

genNPZ.py：将对应目录下的图片文件转换为npz数据。

	在该文件开头的部分可以对rootpath、classes、width、height进行设置。
	rootpath 待处理的图片所在路径，在该路径下应该有各个分类好的图片的文件夹，可参考samples目录。
	classes 待处理图片的类别名称，这里设置了5个类 bus、dinasour、elephant、flower、horse
	width 想要将图片resize之后保留的宽度
	height想要将图片resize之后保留的高度

	gen_dataset(n_train, n_test)	生成对应的数据文件。
	
	gen_3chdataset(n_train, n_test) 生成具有3个通道的数据文件。
	
	save_image() 将resize处理后的数据保存到 根目录\saved_img\类别名字\ 目录下
	
readNPZ.py: 将data.npz中的数据读出
	get_data() 返回形如(x_train,y_train),(x_test,y_test)格式的数据，其中x_train、y_train、x_test、y_test都是array类型的数据。
	
rename.py:
	classes 待处理类别名称元组
	将对应类别下的文件从0.JPEG到n-1.JPEG进行命名。 如果已经存在部分名字，名字冲突会报错，可以先全部改成其他名字，之后再改回来。
	


