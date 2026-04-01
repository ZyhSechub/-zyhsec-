# 📌 基于深度学习的恶意网络流量检测系统（NSL-KDD）

> 一个完整的离线恶意网络流量检测原型系统，基于 **NSL-KDD** 数据集，支持 **DNN / CNN / LSTM** 多模型训练、评估、离线推理与实验分析。

---

## 📖 一、项目简介

本项目围绕经典入侵检测数据集 **NSL-KDD**，构建了一个完整的离线恶意网络流量检测原型系统。项目以结构化网络流量特征为输入，通过深度学习方法对正常流量与恶意流量进行识别，并支持：

- **二分类任务**：正常流量 vs 攻击流量
- **多分类任务**：normal / dos / probe / r2l / u2r

除了基础模型训练外，项目还实现了统一的数据预处理流程、标准化评估、离线推理、结果汇总与实验报告生成，形成了较完整的工程闭环。

### ✨ 项目亮点

- ✅ **统一数据预处理流程**：面向 NSL-KDD 数据特征设计标准化处理链路
- ✅ **多模型对比实验**：支持 DNN / CNN / LSTM 三类深度学习模型
- ✅ **双任务支持**：同时支持二分类与五分类检测任务
- ✅ **标准化评估体系**：统一输出 Accuracy / Precision / Recall / F1
- ✅ **离线推理能力**：支持模型加载、测试集推理与外部文件预测
- ✅ **实验分析自动化**：支持实验结果汇总、可视化与报告生成
- ✅ **模块化项目结构**：便于扩展新模型、新数据集和新功能

---

## 🎯 二、项目目标

本课题的主要目标如下：

| 序号 | 目标描述 |
|:----:|----------|
| 1 | 构建统一的数据预处理流程，适配 NSL-KDD 数据集 |
| 2 | 实现多种深度学习模型（DNN / CNN / LSTM） |
| 3 | 支持二分类与多分类两类检测任务 |
| 4 | 对比不同模型在不同任务下的性能表现 |
| 5 | 输出标准评估指标，形成可比较的实验结果 |
| 6 | 构建离线恶意网络流量检测原型系统 |

### 🧭 检测任务说明

#### 1）二分类（Binary Classification）

将原始标签映射为两类：

- `normal` → 正常流量
- `attack` → 所有攻击流量

#### 2）多分类（Multiclass Classification）

将原始攻击标签归并为 5 类：

- `normal`：正常流量
- `dos`：拒绝服务攻击
- `probe`：探测攻击
- `r2l`：远程到本地攻击
- `u2r`：用户到 root 攻击

---

## 📊 三、数据集说明

### 数据集基本信息

| 属性 | 说明 |
|------|------|
| **数据集名称** | NSL-KDD |
| **训练集规模** | 125,973 条 |
| **测试集规模** | 22,544 条 |
| **原始特征数** | 41 个 |
| **特征类型** | 数值型 + 类别型 |
| **应用场景** | 入侵检测 / 恶意流量识别 |

### 📁 数据目录结构

```bash
nsl-kdd/
├── KDDTrain+.txt    # 训练集
└── KDDTest+.txt     # 测试集
```

### 🏷️ 标签说明

#### 二分类标签映射

| 原始标签 | 编码 |
|----------|------|
| normal | 0 |
| attack | 1 |

#### 多分类标签映射

| 类别 | 说明 |
|------|------|
| normal | 正常流量 |
| dos | 拒绝服务攻击 |
| probe | 探测攻击 |
| r2l | 远程到本地攻击 |
| u2r | 用户到 root 攻击 |

---

## 🏗️ 四、项目结构

```text
src/
├── data/                         # 数据处理模块
│   ├── dataset_config.py         # 数据集配置
│   ├── dataset_loader.py         # 数据加载器
│   └── preprocess.py             # 预处理流程
│
├── models/                       # 模型定义模块
│   ├── dnn.py                    # DNN 模型
│   ├── cnn.py                    # CNN 模型
│   └── lstm.py                   # LSTM 模型
│
├── training/                     # 训练模块
│   └── train.py                  # 统一训练入口
│
├── evaluation/                   # 评估模块
│   └── evaluate.py               # 评估指标计算与结果输出
│
├── inference/                    # 推理模块
│   └── predict.py                # 离线推理入口
│
├── analysis/                     # 实验分析模块
│   ├── summarize_results.py      # 结果汇总
│   ├── plot_results.py           # 模型对比图绘制
│   ├── plot_history.py           # 训练曲线绘制
│   └── generate_report.py        # 实验报告生成
│
results/                          # 实验结果输出目录
├── history/                      # 训练历史记录
├── summary/                      # 汇总结果
└── plots/                        # 图表输出
│
artifacts/
└── models/                       # 训练后保存的模型文件
```

---

## ⚙️ 五、核心功能

## 1. 数据预处理

针对 NSL-KDD 中包含数值特征与类别特征的特点，项目设计了统一预处理流程，保证训练、验证、测试与推理阶段数据处理的一致性。

### 处理流程

| 处理类型 | 方法 | 说明 |
|----------|------|------|
| 类别特征 | One-Hot 编码 | 将协议类型、服务类型、连接状态等类别型特征转为数值表示 |
| 数值特征 | StandardScaler 标准化 | 对数值型特征进行标准化，使其均值为 0、方差为 1 |
| 标签映射 | 任务模式转换 | 根据 `binary` 或 `multiclass` 模式映射标签 |
| 数据划分 | 训练 / 验证 / 测试 | 避免数据泄漏，仅在训练数据上拟合预处理器 |

### 预处理特点

- 支持二分类与多分类两种标签模式
- 统一特征处理逻辑，训练与推理共用
- 能够兼容外部输入文件的批量预测场景
- 减少不同模块之间的数据处理差异

---

## 2. 模型支持

当前项目支持三类深度学习模型，分别用于比较不同网络结构在结构化网络流量检测任务上的表现。

| 模型 | 类型 | 特点 |
|------|------|------|
| DNN | 全连接神经网络 | 结构简单，适合结构化特征输入，训练效率高 |
| CNN | 1D 卷积神经网络 | 具备局部模式提取能力，适合挖掘特征组合关系 |
| LSTM | 长短期记忆网络 | 适合序列建模，可捕捉特征间潜在依赖关系 |

### 模型使用说明

- `DNN`：作为结构化数据基线模型
- `CNN`：通过 1D 卷积增强局部特征提取能力
- `LSTM`：将特征序列化建模，探索序列依赖信息

---

## 3. 训练功能

项目提供统一训练入口：

```bash
python -m src.training.train
```

### 支持参数

| 参数 | 类型 | 说明 | 默认值 |
|------|------|------|--------|
| `--model` | str | 模型类型：`dnn` / `cnn` / `lstm` | `dnn` |
| `--mode` | str | 任务类型：`binary` / `multiclass` | `binary` |
| `--epochs` | int | 训练轮数 | `10` |
| `--batch-size` | int | 批次大小 | `32` |
| `--validation-size` | float | 验证集比例 | `0.2` |
| `--random-seed` | int | 随机种子 | `42` |
| `--use-class-weight` | flag | 是否启用类别权重 | `False` |
| `--save-model` | flag | 是否保存训练后的模型 | `False` |
| `--data-dir` | str | 数据集目录路径 | `nsl-kdd` |

### 训练功能特点

- 支持不同模型与不同任务模式的统一调用
- 支持自动在测试集上评估模型性能
- 支持将实验结果保存为 JSON 文件
- 支持保存训练后的 `.keras` 模型
- 支持使用 `class_weight` 缓解类别不平衡问题

---

## 4. 评估功能

项目在训练完成后可自动对模型进行评估，并输出标准化指标。

### 支持指标

- Accuracy
- Precision
- Recall
- F1-Score
- Classification Report
- Confusion Matrix（如后续扩展）

### 评估特点

- 统一评估接口，便于跨模型比较
- 同时适用于二分类与多分类任务
- 输出结果既可终端查看，也可持久化保存

---

## 5. 推理功能

统一推理入口：

```bash
python -m src.inference.predict
```

### 推理能力

- ✅ 加载训练好的模型（`.keras`）
- ✅ 对测试集进行批量预测
- ✅ 对外部输入文件（CSV）进行推理
- ✅ 输出预测结果文件
- ✅ 生成预测统计与评估信息
- ✅ 若输入带标签，可自动计算指标

### 常见输出文件

| 文件 | 说明 |
|------|------|
| `predictions.csv` | 预测结果，包含预测标签及相关信息 |
| `classification_report.txt` | 分类报告 |
| `metrics.json` | 指标结果 |
| `results/predictions/*.csv` | 批量推理输出目录 |

---

## 6. 实验分析功能

项目提供独立的实验分析模块，用于自动整理多次实验结果。

### 支持命令

| 命令 | 功能 |
|------|------|
| `python -m src.analysis.summarize_results` | 汇总所有实验结果 |
| `python -m src.analysis.plot_results` | 生成模型对比图 |
| `python -m src.analysis.plot_history --history-file xxx.json` | 绘制训练曲线 |
| `python -m src.analysis.generate_report` | 自动生成实验报告 |

### 分析模块特点

- 自动聚合多组实验结果
- 生成结果汇总表格
- 生成模型性能柱状图
- 支持输出 Markdown / HTML 报告
- 为课程设计、毕设展示和论文写作提供支撑

---

## 🔄 六、完整实验流程

### 步骤 1：训练模型

以 DNN 二分类任务为例：

```bash
python -m src.training.train \
    --model dnn \
    --mode binary \
    --epochs 5 \
    --batch-size 32 \
    --data-dir nsl-kdd \
    --validation-size 0.2 \
    --random-seed 42 \
    --save-model
```

### 步骤 2：汇总实验结果

```bash
python -m src.analysis.summarize_results
```

### 步骤 3：生成模型对比图

```bash
python -m src.analysis.plot_results
```

### 步骤 4：生成训练曲线图

```bash
python -m src.analysis.plot_history --history-file results/history/your_history_file.json
```

### 步骤 5：生成实验报告

```bash
python -m src.analysis.generate_report
```

### 步骤 6：执行离线推理

```bash
python -m src.inference.predict \
    --model-path artifacts/models/dnn_binary_xxx.keras \
    --model dnn \
    --mode binary \
    --data-dir nsl-kdd
```

---

## 📈 七、实验结果总结

### 实验概况

- 实验模型：**DNN / CNN / LSTM**
- 任务类型：**Binary Classification + Multiclass Classification**
- 结果用途：用于比较不同模型在恶意流量检测任务中的性能表现

> 注：若你的实际实验文件数量与下表不完全一致，请以 `results/summary` 中汇总结果为准。

---

### 🏆 最优模型结果

## 1. 二分类任务（Binary Classification）

| 指标 | 数值 | 最优模型 |
|------|------|----------|
| Accuracy | 0.8011 | DNN |
| Precision | 0.9709 | DNN |
| Recall | 0.6706 | DNN |
| F1-Score | 0.7933 | DNN |

**最优模型：DNN**

---

## 2. 多分类任务（Multiclass Classification）

| 指标 | 数值 | 最优模型 |
|------|------|----------|
| Accuracy | 0.7662 | CNN |
| Precision | 0.7696 | CNN |
| Recall | 0.7662 | CNN |
| F1-Score | 0.7635 | CNN |

**最优模型：CNN**

---

## 📊 八、模型性能对比

| 模型 | Binary Acc | Binary F1 | Multi Acc | Multi F1 |
|------|------------|-----------|-----------|----------|
| DNN | 0.8011 | 0.7933 | 0.7612 | 0.7584 |
| CNN | 0.7956 | 0.7878 | 0.7662 | 0.7635 |
| LSTM | 0.7892 | 0.7815 | 0.7548 | 0.7519 |

---

## 💡 九、结果分析

### 🔍 关键发现

#### 二分类任务

- DNN 在二分类任务中表现最佳，说明全连接网络对结构化流量特征具有较强适应性
- Precision 较高，说明误报率较低
- Recall 相对偏低，说明仍存在部分攻击流量漏检问题

#### 多分类任务

- CNN 在多分类任务中取得最佳综合表现
- 1D 卷积结构对局部模式特征提取更有效
- 多分类场景下不同攻击类型之间存在较强区分难度

#### 少数类问题

- `r2l` 与 `u2r` 类别样本较少
- 少数类识别效果通常弱于主流类别
- 启用 `class_weight` 后，少数类 Recall 通常会有所改善，但整体 Accuracy 可能略有下降

### 📌 结果结论

- **DNN 更适合作为二分类基线模型**
- **CNN 更适合作为多分类场景下的优选模型**
- **LSTM 具备探索价值，但当前结果未超过 DNN / CNN**

---

## ✨ 十、项目特点

| 特点 | 说明 |
|------|------|
| 🔄 完整工程闭环 | 训练 → 评估 → 推理 → 分析，全流程打通 |
| 🧪 多模型对比实验 | 支持 DNN / CNN / LSTM 横向比较 |
| 💻 离线检测原型 | 支持本地模型加载与批量预测 |
| 📊 自动化实验分析 | 支持实验汇总、绘图与报告生成 |
| 🏗️ 结构清晰易扩展 | 代码模块划分明确，便于后续迭代 |

---

## 🚀 十一、后续优化方向

| 方向 | 具体措施 | 预期收益 |
|------|----------|----------|
| 🤖 引入更强模型 | Transformer / Attention 机制 | 提升特征建模能力 |
| 🎯 提升少数类检测 | 过采样、Focal Loss、集成学习 | 改善 `r2l/u2r` 检测率 |
| 📊 扩展数据集验证 | CIC-IDS、UNSW-NB15 | 提升模型泛化能力 |
| 🌐 在线检测系统 | 基于 FastAPI 部署 RESTful API | 支持实时检测与系统集成 |
| 📉 更丰富评估 | 混淆矩阵、ROC、PR 曲线 | 提高分析完整性 |

---

## 🛠️ 十二、运行环境

### 环境要求

| 依赖 | 版本要求 |
|------|----------|
| Python | 3.9+ |
| TensorFlow | 2.x |
| scikit-learn | 1.0+ |
| pandas | 1.3+ |
| numpy | 1.21+ |
| matplotlib | 3.4+ |

### 虚拟环境配置

```bash
# 激活虚拟环境
.\.venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt
```

---

## 🚀 十三、快速开始

### 1️⃣ 训练模型

```bash
python -m src.training.train \
    --model dnn \
    --mode binary \
    --epochs 5 \
    --batch-size 32 \
    --data-dir nsl-kdd \
    --save-model
```

### 2️⃣ 执行推理

```bash
python -m src.inference.predict \
    --model-path artifacts/models/dnn_binary_xxx.keras \
    --model dnn \
    --mode binary \
    --data-dir nsl-kdd
```

### 3️⃣ 汇总结果

```bash
python -m src.analysis.summarize_results
```

### 4️⃣ 生成对比图

```bash
python -m src.analysis.plot_results
```

### 5️⃣ 生成实验报告

```bash
python -m src.analysis.generate_report
```

---

## 📝 附录

### 数据集引用

Tavallaee, M., Bagheri, E., Lu, W., & Ghorbani, A. A. (2009).  
**A detailed analysis of the KDD CUP 99 data set.**  
*2009 IEEE Symposium on Computational Intelligence for Security and Defense Applications*, 1-6.

### 适用场景

本项目适用于：

- 本科毕业设计展示
- 入侵检测方向课程设计
- 深度学习结构化数据建模练习
- 简历 / 面试中的项目展示
- 后续扩展为在线检测系统的基础版本

### 联系方式

如有问题或建议，欢迎提交 **Issue** 或 **Pull Request**。
