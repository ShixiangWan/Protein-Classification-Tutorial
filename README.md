

# 蛋白质分类教程（以cytokine为例）
version 0.1

## 一、流程概述如下：

![](/img/001.jpg)

## 二、下载FASTA数据（正例）并识别家族属性（PFAM）

识别蛋白质家族属性的方法有很多，最传统的方法是在某个蛋白质数据库（例如：Uniport）中找到该种蛋白质，其家族属性（PFAM）便一目了然。 （参见：http://datamining.xmu.edu.cn/~wangzhen/cytokine/）。 显然，这种方法只适合小批量查询，我们写个小Java程序实现信息抓取。举个例子，我想分类一个蛋白质是不是cytokine (细胞因子)，步骤如下（请自备梯子）： 

#### 1. 找出目前已知的cytokine，先google一下有没有已知的database，如果没有可以去Uniprot数据库（[链接](http://www.uniprot.org/ "Title")），搜索cytokine，然后以FASTA格式download下载结果： 

![](/img/002.jpg)

#### 2. 下载完毕，解压缩文件后，将FASTA数据格式的文件用Notepad++打开，可以看到如下格式，其中，用红色方框圈起来的部分是蛋白质的UniportID：
 
![](/img/003.jpg)

#### 3. 利用UniportID查询蛋白质的PFAM家族。

首先，访问网站http://www.programmableweb.com/

然后，查找PFAM对应的API：

![](/img/004.jpg)

![](/img/005.jpg)

#### 4. 进入该API网站，我们可以看到有这样的返回XML格式的API，从返回的简单XML结果中提取PFAM，速度杠杠的！
```
api：http://pfam.xfam.org/protein/{PFAM}?output=xml；
```

 ![](/img/006.jpg)


#### 5. 将以上步骤编程实现

程序源代码部署在Github：https://github.com/ShixiangWan/EasyNegProtein


## 三、去重复并抽取反例

首先需要知道：此时属于cytokine的蛋白序列就是正例，否则就是反例。每个序列都会属于一个PFAM家族，相似序列会属于同一个家族。

在找出cytokine（正例）的所有PFAM家族后，需要删除重复的PFAM得到不重复的正例家族（txt格式）。在全部家族中删除正例序号对应的家族那么找出所有反例的家族，并且每个反例家族留一条最长的蛋白序列作为抽取到的反例。

程序源代码部署在Github：https://github.com/ShixiangWan/EasyNegProtein

## 四、CH-HIT聚类去冗余

设置合适的聚类阈值，分别对前面得到的正反例fasta序列文件进行聚类，去除冗余，精简序列个数，方便后面进行的特征提取处理。

CD-HIT网址：http://weizhong-lab.ucsd.edu/public/

使用方法非常简单，设置好输入文件名，聚类阈值（例如70%），输出文件名即可。

## 五、提取特征向量

前述的方法已经得到较为精简的fasta蛋白质序列文件，weka无法对其进行分类操作，需要将其分别提取特征向量，并按照csv或arff等weka能够处理的文件格式转化。
提取特征向量的方法非常多，如对于蛋白质序列，有188维，Pse-in-one，Pseb，K-skip，n-gram，protrweb等。

一个全面的总结帖：http://malab.cn/forum.php?mod=viewthread&tid=926&extra=page%3D1

* 188维网址：见总结帖
* Pse-in-one网址：http://bioinformatics.hitsz.edu.cn/Pse-in-One/
* Pseb网址：见总结帖
* n-gram网址：见总结帖
* protrweb网址：http://protrweb.scbdd.com/

## 六、对高维特征向量降维

N-Gram方法中，如果n取值为3，依据蛋白质氨基酸种类为20，那么将得到20*20*20=8000维的特征向量，在分类或预测成千上万个蛋白序列时，必然产生了高维灾难，不利于研究进行。降维方法也有很多，例如mRMR，mRMD等等。

* mRMR网址：http://penglab.janelia.org/proj/mRMR/
* mRMD网址：http://datamining.xmu.edu.cn/~zjcdm/data/MRMD_CH.html

## 七、weka交叉验证及分类预测

1. 首先，要对前述得到正反例特征向量文件进行格式整理，整理成为arff或libsvm或csv文件均可（arff文件是weka支持的标准格式；libsvm是很多提取特征向量算法得到的输出文件格式；csv存储所需的空间最小，最简单，墙裂推荐）；

2. 对整理后的正例文件加正例标签（例如1），对整理后的反例文件加反例标签（例如0），并合并成一个总文件。对这个总文件调用weka分类器（例如LibSVM，LibD3C，LibLinear，Bagging，Randomforest，IBK，J48,等等）进行5折或10折交叉验证分类评估，得到分类准确度以及混淆矩阵，特异性指标和灵敏度指标。

3. 注意：LibD3C是数据挖掘实验室开发，网址如下：http://datamining.xmu.edu.cn/~gjs/project/LibD3C.html

 ![](/img/007.jpg)

### 参考网站：
[1] http://datamining.xmu.edu.cn/~wangzhen/cytokine/

[2] http://malab.cn/forum.php?mod=viewthread&tid=926&extra=page%3D1

