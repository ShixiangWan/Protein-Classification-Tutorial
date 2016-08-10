

# 蛋白质分类教程（以cytokine为例）
version 0.1

##一、流程概述如下：

![](http://lab.malab.cn/~shixiang/img/001.jpg)

##二、下载FASTA数据（正例）并识别家族属性（PFAM）

识别蛋白质家族属性的方法有很多，最传统的方法是在某个蛋白质数据库（例如：Uniport）中找到该种蛋白质，其家族属性（PFAM）便一目了然。 （参见：http://datamining.xmu.edu.cn/~wangzhen/cytokine/）。 显然，这种方法只适合小批量查询，我们写个小Java程序实现信息抓取。举个例子，我想分类一个蛋白质是不是cytokine (细胞因子)，步骤如下（请自备梯子）： 

####1.  找出目前已知的cytokine，先google一下有没有已知的database，如果没有可以去Uniprot数据库（[链接](http://www.uniprot.org/ "Title")），搜索cytokine，然后以FASTA格式download下载结果： 

![](http://lab.malab.cn/~shixiang/img/002.jpg)

####2.  下载完毕，解压缩文件后，将FASTA数据格式的文件用Notepad++打开，可以看到如下格式，其中，用红色方框圈起来的部分是蛋白质的UniportID：
 



####3.  利用UniportID查询蛋白质的PFAM家族。首先，访问网站http://www.programmableweb.com/，查找PFAM对应的API：









 












