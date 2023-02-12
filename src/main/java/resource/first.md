boston数据集-特征是连续数字，用于做回归

用来衡量在未知数据中的准确率的指标，叫做泛化误差。

1、模型太复杂或太简单，都会让泛化误差升高，我们追求的是位于中间的平衡点；
2、模型太复杂就会过拟合，模型太简单就会欠拟合；
3、对树模型和树的集成模型来说，树的深度越深，枝叶越多，模型越复杂
4、树模型和树的集成模型的目标，都是减少复杂度，把模型往图像的左边移动


参数对于模型的影响程度
1、n_estimators
2、max_depth
3、min_samples_leaf
4、min_samples_split
5、max_features
6、criterion，一般用gini



学习曲线

sklearn中的特征曲线必须是二维


数据挖掘5大流程
1、获取数据
2、数据预处理
数据预处理是从数据中检测，纠正或删除损坏，不准确或不适用于模型的记录的过程
可能面对的问题有：
数据类型不同，比如有的是文字，有的是数字，有的含时间序列
也可能数据质量不行，有噪声、有异常等
3、特征工程
特征工程是将原始数据转换为更能代表预测模型的潜在问题的特征的过程，可以通过挑选最相关的特征，提取特征以及创造特征来实现。其中创造特征又经常以降维算法的方式实现。
可能面对的问题：特征之间有相关性，特征和标签无关
特征工程目标：1、降低计算成本 2、提升模型上限

4、建模，测试模型并预测结果
5、上线，验证模型效果


数据预处理PreProcessing & Impute
数据无量纲化
在机器学习算法实践中，我们往往有着将不同规格的数据转换到统一规格，或不同分布的数据转换到某个特定分布的需求，这种需求统称为将数据”无量纲化“。
数据的无量纲化可以是线性的，也可以是非线性的。
线性的无量纲化包括中心化处理（Zero-centered）和缩放处理（Scale）。
中心化的本质是让所有记录减去一个固定值，既让样本数据平移到某个位置。
缩放的本质是通过除以一个固定值，将数据固定在某个范围内。
当数据(x)按照最小值中心化后，再按极差（最大值减最小值）缩放，数据移动了最小值个单位，并且会被收敛到[0,1]之间。
这个过程，就叫做 数据归一化（Normalization，Min-Max Scaling）。归一化后的数据服从正态分布。
当数据(x)按均值(u)中心化后，再按标准差(o)缩放，数据就会服从均值为0，方差为1的正态分布（标准正态分布）。
这个过程，就叫做数据标准化（Standardization，Z-score normalization）。

StandardScaler和MinMaxScaler选哪个？
大多数机器学习算法中，会选择StandardScaler进行特征缩放，因为MinMaxScaler对异常值特别敏感。
建议先试试StandardScaler，效果不好的话再换MinMaxScaler

编码与哑变量
专门处理分类型特征的预处理方式
在现实中，许多特征和标签都是不是以数字表示的，此时需要将数据进行编码，即将文字型数据转换为数值型。


处理连续型特征：二值化与分段
二值化：根据阈值将特征进行二值化，将特征值设置为0或1
分段（分箱）：将连续型变量划分为分类变量，将连续型变量排序后按顺序分箱并编码。


数据预处理步骤
1、数据无量纲化
2、处理缺失值
3、使用编码与哑变量处理分类型特征值
4、使用二值化与分箱处理连续性特征值


当数据预处理完成后，我们就要开始进行特征工程。
特征工程包含：
1、特征提取(feature extraction)
从文字、图像、声音等非结构化数据中提取新信息作为特征。比如说，从淘宝报备的名称中提取出产品类别，产品颜色，是否是网红产品等等。
2、特征创造(feature creation)
把现有特征进行组合，或互相计算，得到新的特征。比如说，有一列特征是速度，有一列特征是距离，我们就可以通过让两列相除的方式，创造新的特征：通过距离所花的时间。
3、特征选择(feature selection)
从所有的特征中，选择出有意义，对模型有帮助的特征，以避免将所有特征都导入模型去训练的情况。

如果不了解业务，或者无法依赖对业务的理解来选择特征，可以使用以下四种方法选择特征：
1、过滤法
过滤法通常用作预处理步骤，特征选择完全独立于任何机器学习算法。它是根据各种统计检验中的分数以及各种相关性的指标来选择特征。
过滤法的主要应用对象是需要遍历特征或升维的算法们（最近邻算法KNN、单颗决策树、神经网络、回归算法等），而过滤法的主要目的是：在维持算法表现的前提下，帮助算法降低计算成本，但是无法保证算法表现性能。
1、方差过滤(Variance Threshold)：通过特征本身的方差来筛选特征。比如一个特征本身的方差很小，就表示样本在这个特征上基本没有差异，可能特征中的大多数值一样，甚至整个特征的取值都相同。
方差过滤对于算法模型的影响：随机森林的准确率略逊于KNN，但运行时间连KNN的1%不到，只需要十几秒时间。
方差过滤后，随机森林的准确率也微弱上升，但运行时间几乎没什么变化。
方差过滤调的特征到底是噪声还是有效特征，过滤后的模型到底是会变好还是会变坏？答案是每个数据集不一样，只能自己去尝试。
我们只会使用阈值为0挥着阈值很小的方差过滤，来消除一些明显用不到的特征，然后选择更优的特征选择方法来继续削减特征数量。
2、相关性过滤：我们希望选出与标签相关且具有意义的特征，因为这样的特征能够为我们提供大量信息。有三种常用的方法来评判特征与标签之间的相关性：
1、卡方
2、F检验
3、互信息

	2、嵌入法
	3、包装法
	4、降维算法	