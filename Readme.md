1、明确需求
基于中国的病毒数据训练模型(训练集)
预测其他国家和地区的感染人数走势(测试及验证)
感染人数的指标选择累积感染人数(因为题目要求是感染人数,且新增感染人数泛化误差更大)
待解决的问题：
    中国的数据模型一定适用于外国？排除病毒流行本身特征，各地医疗、政策及重视程度也有比较大影响
    各国流行疫情的时间区间不一致，中国较早流行新冠病毒，如果建立时间序列模型，模型结果对时间敏感，验证预测时会产生不小影响
    单纯以时间作为自变量训练模型，是否忽略了其他比如医疗水平，政策以及文化带来的影响
    题目所说其他国家或地区，是否是指除中国以外其他所有国家感染病毒数据的总和？本次分析采用选择其中几个重要的国家数据进行分析
建模思路
    参考 https://baijiahao.baidu.com/s?id=1663464336214532632&wfr=spider&for=pc
    模型一般考虑为时间序列模型(ARIMA或LSTM)或基于病毒流行学理论的动力模型(SIR模型等)
    钟南山院士在论文中用了LSTM做训练 参考链接如下：http://jtd.amegroups.com/article/view/36385
    一般还用一种模型 logistics曲线模型
过程
    根据中国病毒数据进行训练，部分已知的外国数据作为测试，其他已知的外国数据作为验证，未知的外国数据作为预测
结果
    走势需要可视化，分析统计特征，以及各个国家和地区的对比
2、 获取数据集
数据来源
https://www.kaggle.com/sudalairajkumar/novel-corona-virus-2019-dataset
https://pages.semanticscholar.org/coronavirus-research
3、进行数据清洗
    删除不用的数据列比如 Province/State	Country/Region	Lat	Long
    根据国家进行合并成每国感染的总数据（把每个国家对应的每个地区的数据全部加起来的总和）
    转置，使得行标签为时间
    分为三个 确诊人数，死亡人数，治愈人数
    无需缺失值，异常值处理
4、依据进行数据建模（不考虑动力学模型，只是基于数据的机器学习深度学习模型）
    做的过程中，因为Arima模型的超参数估计在当前数据量下不满足显著性水平，故舍弃该方法
    然后使用logistics方程进行了预测，短期预测效果较好，但是长期效果不显著
    使用LSTM 进行模型预测
5、可视化结果数据，数据在result目录下，
    其中predict.txt为主要国家和地区的未来七天确诊死亡治愈的数据
    重要国家及地区的图片可视化数据

6、代码为python代码，需要先执行
    pip install -r requirements.txt
    导入相关依赖
    因为框架为Keras整合TensorFlow，所以推荐使用anaconda集成包进行安装

    dataModel为主程序入口
    用户需输入两个参数，即待选国家和待选指标（确诊，死亡，治愈）
    然后点击运行



