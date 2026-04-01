# 📌 基于深度学习的恶意网络流量检测系统（NSL-KDD）

> 一个完整的离线恶意网络流量检测原型系统，基于 NSL-KDD 数据集，支持多模型对比与自动化实验分析。

---

## 📖 一、项目简介

本项目基于经典入侵检测数据集 **NSL-KDD**，实现了一个完整的离线恶意网络流量检测原型系统。通过构建深度学习模型，对网络流量进行建模，实现对正常流量与攻击流量的自动识别，并支持多类别攻击分类。

### ✨ 项目亮点

项目不仅完成模型训练，还实现了：

- ✅ **统一数据预处理流程**（适配 NSL-KDD）
- ✅ **多模型对比实验**（DNN / CNN / LSTM）
- ✅ **标准化评估体系**（Accuracy / Precision / Recall / F1）
- ✅ **离线推理能力**（支持单条/批量预测）
- ✅ **实验结果自动汇总与报告生成**

---

## 🎯 二、项目目标

本课题的主要目标包括：

| 序号 | 目标描述 |
|:----:|----------|
| 1 | 构建统一的数据预处理流程（适配 NSL-KDD） |
| 2 | 实现多种深度学习模型（DNN / CNN / LSTM） |
| 3 | 支持两类检测任务：<br>• 二分类（normal vs attack）<br>• 多分类（normal / dos / probe / r2l / u2r） |
| 4 | 对比不同模型性能 |
| 5 | 输出标准评估指标（Accuracy / Precision / Recall / F1） |
| 6 | 构建离线恶意流量检测原型系统 |

---

## 📊 三、数据集说明

### 数据集信息

| 属性 | 说明 |
|------|------|
| **数据集名称** | NSL-KDD |
| **训练集规模** | 125,973 条 |
| **测试集规模** | 22,544 条 |
| **原始特征数** | 41 个（数值型 + 类别型） |

### 📁 数据目录结构

```bash
nsl-kdd/
├── KDDTrain+.txt    # 训练集
└── KDDTest+.txt     # 测试集
🏷️ 标签类型
二分类（Binary）
原始标签	编码
normal	0
attack	1
多分类（Multiclass）
类别	说明
normal	正常流量
dos	拒绝服务攻击
probe	探测攻击
r2l	远程到本地攻击
u2r	用户到根攻击
🏗️ 四、项目结构
text
src/
├── 📁 data/                     # 数据处理模块
│   ├── dataset_config.py        # 数据集配置
│   ├── dataset_loader.py        # 数据加载器
│   └── preprocess.py            # 预处理流程
│
├── 📁 models/                   # 模型定义模块
│   ├── dnn.py                   # DNN 模型
│   ├── cnn.py                   # CNN 模型
│   └── lstm.py                  # LSTM 模型
│
├── 📁 training/                 # 训练模块
│   └── train.py                 # 训练入口
│
├── 📁 evaluation/               # 评估模块
│   └── evaluate.py              # 评估指标计算
│
├── 📁 inference/                # 推理模块
│   └── predict.py               # 离线推理
│
├── 📁 analysis/                 # 实验分析模块
│   ├── summarize_results.py     # 结果汇总
│   ├── plot_results.py          # 对比图绘制
│   ├── plot_history.py          # 训练曲线绘制
│   └── generate_report.py       # 报告生成
│
📁 results/                      # 实验结果输出目录
📁 artifacts/models/             # 模型文件存储目录
⚙️ 五、核心功能
🔧 数据预处理
处理类型	方法	说明
类别特征	One-Hot 编码	将类别型特征转换为数值型
数值特征	标准化（StandardScaler）	均值为0，方差为1
数据划分	训练/验证/测试	避免数据泄漏，仅在训练集上拟合
🤖 模型支持
模型	类型	特点
DNN	全连接神经网络	适合结构化特征，简单高效
CNN	1D 卷积网络	局部特征提取能力强
LSTM	长短期记忆网络	适合序列建模，捕捉时序依赖
🚀 训练功能
统一训练入口：

bash
python -m src.training.train
支持参数：

参数	类型	说明	默认值
--model	str	模型类型：dnn / cnn / lstm	dnn
--mode	str	任务类型：binary / multiclass	binary
--epochs	int	训练轮数	10
--batch-size	int	批次大小	32
--use-class-weight	flag	是否使用类别权重	False
--save-model	flag	是否保存模型	False
--data-dir	str	数据目录路径	nsl-kdd
🔍 推理功能
bash
python -m src.inference.predict
支持特性：

✅ 加载训练好的模型（.keras 格式）

✅ 对测试集批量推理

✅ 对外部文件（CSV）进行推理

✅ 输出预测结果 CSV 文件

✅ 生成分类统计报告

✅ 计算评估指标

输出文件：

文件	说明
predictions.csv	预测结果（包含真实标签与预测标签）
classification_report.txt	分类报告（Precision/Recall/F1）
metrics.json	评估指标 JSON 文件
📊 实验分析
命令	功能
python -m src.analysis.summarize_results	汇总所有实验结果
python -m src.analysis.plot_results	生成模型对比图（柱状图）
python -m src.analysis.plot_history --history-file xxx.json	绘制训练曲线（损失/准确率）
python -m src.analysis.generate_report	自动生成实验报告（Markdown/HTML）
🔄 六、完整实验流程
步骤 1：训练模型
bash
# 训练 DNN 二分类模型
python -m src.training.train \
    --model dnn \
    --mode binary \
    --epochs 5 \
    --batch-size 32 \
    --data-dir nsl-kdd \
    --save-model
步骤 2：汇总实验结果
bash
python -m src.analysis.summarize_results
步骤 3：生成对比图
bash
python -m src.analysis.plot_results
步骤 4：生成实验报告
bash
python -m src.analysis.generate_report
📈 七、实验结果总结
实验概况
实验总数：9 次

模型类型：DNN / CNN / LSTM

任务类型：Binary Classification + Multiclass Classification

🏆 最优模型结果
二分类任务（Binary Classification）
指标	数值	最优模型
Accuracy	0.8011	DNN
Precision	0.9709	DNN
Recall	0.6706	DNN
F1-Score	0.7933	DNN
🎯 最优模型：DNN

多分类任务（Multiclass Classification）
指标	数值	最优模型
Accuracy	0.7662	CNN
Precision	0.7696	CNN
Recall	0.7662	CNN
F1-Score	0.7635	CNN
🎯 最优模型：CNN

💡 八、结果分析
🔍 关键发现
1. 二分类任务
DNN 表现最优：全连接网络更适合处理结构化特征

高 Precision（0.9709） 表明误报率低

相对较低的 Recall（0.6706） 说明仍有部分攻击漏检

2. 多分类任务
CNN 表现最优：1D 卷积能够提取局部特征模式

各类别综合性能均衡

少数类识别困难：r2l 和 u2r 样本数量少，检测效果较差

3. 类别权重的作用
使用 class_weight 参数后

少数类 Recall 提升约 5-8%

整体 Accuracy 略有下降（约 1-2%）

📊 模型性能对比
模型	Binary Acc	Binary F1	Multi Acc	Multi F1
DNN	0.8011	0.7933	0.7612	0.7584
CNN	0.7956	0.7878	0.7662	0.7635
LSTM	0.7892	0.7815	0.7548	0.7519
✨ 九、项目特点
特点	说明
🔄 完整工程闭环	训练 → 推理 → 分析，全流程打通
🧪 多模型对比实验	支持 DNN/CNN/LSTM 三种模型横向对比
💻 离线检测原型	支持单条/批量流量检测，可直接部署
📊 自动化实验报告	自动生成 Markdown/HTML 格式报告
🏗️ 结构清晰易扩展	模块化设计，便于添加新模型或新功能
🚀 十、后续优化方向
方向	具体措施	预期收益
🤖 引入先进模型	Transformer / Attention 机制	提升特征提取能力
🎯 提升少数类检测	过采样、Focal Loss、集成学习	改善 r2l/u2r 检测率
📊 扩展数据集	CIC-IDS、UNSW-NB15	验证模型泛化能力
🌐 在线检测系统	部署为 RESTful API 服务	实现实时流量检测
🛠️ 十一、运行环境
环境要求
依赖	版本要求
Python	3.9+
TensorFlow	2.x
scikit-learn	1.0+
pandas	1.3+
numpy	1.21+
matplotlib	3.4+
虚拟环境配置
bash
# 激活虚拟环境
..venv\Scripts\activate

# 安装依赖（如需要）
pip install -r requirements.txt
🚀 十二、快速开始
1️⃣ 训练模型
bash
python -m src.training.train \
    --model dnn \
    --mode binary \
    --epochs 5 \
    --batch-size 32 \
    --data-dir nsl-kdd \
    --save-model
2️⃣ 执行推理
bash
python -m src.inference.predict \
    --model-path artifacts/models/dnn_binary_xxx.keras \
    --model dnn \
    --mode binary \
    --data-dir nsl-kdd
3️⃣ 生成报告
bash
python -m src.analysis.generate_report
📝 附录
数据集引用
Tavallaee, M., Bagheri, E., Lu, W., & Ghorbani, A. A. (2009). A detailed analysis of the KDD CUP 99 data set. 2009 IEEE Symposium on Computational Intelligence for Security and Defense Applications, 1-6.

联系方式
如有问题或建议，欢迎提交 Issue 或 Pull Request。
