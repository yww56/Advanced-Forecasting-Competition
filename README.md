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
* 测试集上的MAPE(Mean Absolute Percentage Error,平均绝对误差百分比，精确到小数点后8位)，公式如下：<br>
![Mape计算公式](https://latex.codecogs.com/svg.latex?Mape=\frac{1}{N}\sum_{i=1}^N\left|\frac{y_i-p_i}{y_i}\right|)<br>
其中，yi为真实值，pi为预测值，N为样本数量，训练集和测试集的数据已经满足任何yi均不为0；
* MAPE值越低，排名越高；
* 每位参赛者有3次提交机会，提交相关说明如下：
  * 2020-11-10开始每两周内提交一次，每次所有同学提交之后我会两天之内公布leaderboard,最终成绩取3次分数中最好的一次；
  * 每两周之内的提交具体日期不限，但不可逾期，且两周内只有一次机会；
  * 提交截止日期为2020-12-23；
  * 2020-11-17前可以多进行一次提交进行测试，若大家提交的文件有问题我会进行反馈，但不公布leaderboard.
# Prize
第一名：IIF Student Award,奖励为1年IIF Member和100美金，获奖者学校和姓名将被公示于：IIF Student Award Program;获得第一位的同学可以公开自己的源码或者分享自己的思路给大家。
# Benchmark
基准分数（MAPE）：普通线性回归模型（0.2273）<br>
<br>
MATLAB代码：<br>
```
%%% advanced forecasting competition

%% preprocess the dataset
load('advanced_forecast_competition_train.mat');
[m_train, n] = size(X);
m_test = size(X1);
fprintf('Dimension of training X: %d, %d, dimension of test X: %d, %d\n', m_train, n, m_test, n);
% perform a min-max scaling for both training ang test X
X_all = [X;X1];
m = size(X_all);
mins = min(X_all);
maxs = max(X_all);
for i = 1:m
    X_all(i,:) = (X_all(i,:) - mins) ./ (maxs - mins);
end
scaled_X_train = X_all(1:m_train, :);
scaled_X_test = X_all(m_train + 1:end,:);
clear X_all; % delete X_all to save memory
%% build simple linear regression model
% train linear regression model using MATLAB's Statistics and Machine Learning Toolbox
fprintf('Fitting Linear Regression ...\n');
lm = fitlm(scaled_X_train, y);
pred_train = lm.predict(scaled_X_train);
mape_train = mean(abs((y - pred_train) ./ y));
fprintf('R-Square: %g, Mape on training set: %g\n', lm.Rsquared.Adjusted, mape_train);
pred = lm.predict(scaled_X_test); % make prediction on the test set using fitted model

% save the prediction
save('./2018100402-LRbenchmark.mat', 'pred');
% submit this saved file to me: yww0507@hotmail.com
```
