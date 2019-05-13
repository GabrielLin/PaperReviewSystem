# Paper-Review-System

## Environment requirements

- Python >=3.4

## Run

Install dependencies

```
pip install -r requirement.txt
```

### For Dev

```
python3 index.py
```

### 文件夹说明

- components 文件夹中维护了所有的分析模块
- 参考文献及参考文献标注中为写代码时阅读的文献资料
- paper_data 中为最原始的从知网上下载的文献 pdf
- txt_data 中为将 pdf 解析为 txt 后的结果文献
- train_data 中是经过处理的文献资料用于词向量的训练集
- model 文件夹中是训练完成的模型数据
- format_data 中存放的是最后分析得出的文献数据
- predict 文件夹为预测数据集
- test_dir/jos/jos_data 文件夹为软件学报期刊数据集
- test_dir/ecice/ecice_data 文件夹为计算机工程期刊数据集
- test_dir/ceaj/ceaj_data 文件夹为计算机工程与应用期刊数据集

### 模块说明

具体的函数作用在代码中都有详尽的注释

- pre_work.py 为预处理模块
- format_tool.py 为格式化工具模块
- doc2vec.py 为词向量训练模块
- clean_data.py 为清理异常点模块
- one_class_svm.py 为单分类SVM模块

### 流程说明

1. 初始化文件夹
2. 将 pdf 文献读取为 txt
3. 获取论文基本特征,包括字数,段落数,参考文献数,参考文献发表年限,论文发表年限,论文与参考文献发表年限的差,基金项目,参考文献的来源,作者单位,作者信息,第一作者作者信息
4. 预处理文献预料
5. 训练语料的词向量模型
6. 获取与文献最相似的文献极其相似程度
7. 将文字信息或数组信息总结为权威比重，并记录平均值
8. 清理数据，分类训练集竟可能被信任，并记录下训练集
9. 进行单分类训练
--------------（DONE）
10. 提交评审论文，解析文献特征并预测
11. 若成功则成功，否则比对平均数据找出异常值并返回
12. 人工给出最后评审结果并录入训练集

### 文献特征

- 字数
- 段落数
- 参考文献数
- 作者信息(权威及占比程度)
- 第一作者作者信息(权威及占比程度)
- 作者单位(权威及占比程度)
- 与其他文献平均相似程度
- 与其他文献最大相似程度
- 发表期刊的名称(权威及占比程度)
- 参考文献的来源(权威及占比程度)
- 论文与参考文献发表年限的差
- 基金项目(权威及占比程度)

```diff
- 参考文献发表年限
- 论文发表年限
- 网上的来源的影响因子
```

### 单分类 SVM 的

- 共 12 个特征进行单分类 SVM 训练
- 采用高斯核函数即 rbf
- 训练集误差参数上界及支持向量数下界为 0.05，在对训练集有较高信心的情况下设置为 un=0.05
- gamma 核系数选为 1/（n_features \* X.std（））

### TODO (按优先级排列)

1. 预处理文献特征数据,去除异常点(DONE)
2. 进行单分类 SVM 的训练(DONE)
3. 完整的文献评审流程
4. 参考年限的获取还无法完全准确获取,有待完善
```diff
+ 5. 摘要的相关性
+ 6. 人为维护期刊占比 如：计算机学报 软件学报
计算机工程 选取‘自然语言处理’和‘支持向量机’的关键词的期刊 作为训练集
软件学报 和 计算机工程与应用 选取‘自然语言处理’和‘支持向量机’的关键词的期刊 作为测试集
软件学报的创新度 大于 计算机工程与应用 则创新度证明是可以的
```

