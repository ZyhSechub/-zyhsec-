# 基于深度学习的恶意网络流量检测系统（NSL-KDD）

> 一个完整的离线恶意网络流量检测原型系统，基于 NSL-KDD 数据集，支持多模型对比与自动化实验分析。

---

## 项目简介

本项目基于经典入侵检测数据集 **NSL-KDD**，实现了一个完整的离线恶意网络流量检测系统。

### 项目亮点

- 统一数据预处理流程
- 多模型对比（DNN / CNN / LSTM）
- 标准评估体系（Accuracy / Precision / Recall / F1）
- 支持离线推理
- 自动化实验分析

---

## 数据集说明

### 基本信息

| 属性 | 说明 |
|------|------|
| 数据集 | NSL-KDD |
| 训练集 | 125,973 |
| 测试集 | 22,544 |
| 特征数 | 41 |

### 数据结构

```bash
nsl-kdd/
├── KDDTrain+.txt
└── KDDTest+.txt
```

### 标签说明

#### 二分类

| 标签 | 编码 |
|------|------|
| normal | 0 |
| attack | 1 |

#### 多分类

| 类别 | 说明 |
|------|------|
| normal | 正常 |
| dos | 拒绝服务攻击 |
| probe | 探测攻击 |
| r2l | 远程攻击 |
| u2r | 提权攻击 |

---

## 项目结构

```text
src/
├── data/
├── models/
├── training/
├── evaluation/
├── inference/
├── analysis/
```

---

## 快速开始

### 训练

```bash
python -m src.training.train \
  --model dnn \
  --mode binary \
  --epochs 5 \
  --batch-size 32 \
  --data-dir nsl-kdd \
  --save-model
```

### 推理

```bash
python -m src.inference.predict \
  --model-path artifacts/models/dnn_binary_xxx.keras \
  --model dnn \
  --mode binary \
  --data-dir nsl-kdd
```

---

## 实验结果

### 二分类（DNN）

| 指标 | 数值 |
|------|------|
| Accuracy | 0.8011 |
| Precision | 0.9709 |
| Recall | 0.6706 |
| F1 | 0.7933 |

### 多分类（CNN）

| 指标 | 数值 |
|------|------|
| Accuracy | 0.7662 |
| Precision | 0.7696 |
| Recall | 0.7662 |
| F1 | 0.7635 |

---

## 环境

```bash
.\.venv\Scripts\activate
pip install -r requirements.txt
```
