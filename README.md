# Advanced Forecasting(DUFE) Competition 2020
本竞赛目前仅限组内成员参加<br>
请各位参赛者将结果文件发送至：yww0507@hotmail.com
# Data
分发给各位参赛者的：advanced_forecast_competition_train.mat,保存了三个MATLAB变量：X1, y, X1<br>
* 数据来源于GEF2012Comp(2012年全球能源预测竞赛)，为美国某地区3年间每小时的电力负荷值和温度数据。分发的数据为源数据将时间（年，月，小时）处理为哑变量，
并与温度的幂做交叉得到的，因变量为每个对应时段的电力负荷值，更多详细信息见文章：[PhD Dissertation of Dr. Hong](https://xueshu.baidu.com/usercenter/paper/show?paperid=25c9c3a0d40abf6b37ae346fa4e39414&site=xueshu_se)
* X为训练集的特征（维度：[17520, 289]），y为训练集的因变量（维度：[17520, 1]）
* X1为测试集的特征（维度：[8760, 289]）
* 注意数据的稀疏性所造成内存空间的浪费或者程序效率的影响
# Rules
* 编程语言不限
* 提交的结果文件为测试集各个样本的预测值，请保持与给定测试样本顺序一致，可以是Matlab数据文件(.mat),或者是任意文本文件(.csv, .txt, .dat等等)，
具体要求如下：
  * 文件必须命名为“学号-名字”，不可缺失扩展名，如：“2018100402-于文文.csv”;
  * 若提交.mat文件，必须保证该文件只包含一个MATLAB变量，即测试样本预测值的列（行）向量；
  * 若提交文本文件，必须保证文件只包含一列数字（即测试样本的预测值），无文件头（列名）和索引（行序号）；
* 禁止剽窃其他参赛者代码
# Evaluation
* 测试集上的MAPE(Mean Absolute Percentage Error,平均绝对误差百分比，精确到小数点后8位)，公式如下：
![mape](https://latex.codecogs.com/svg.latex?Mape=\frac{1}{N}\sum_{i=1}^N\left|%20\frac{y_i-p_i}{y_i}\right|\\)
