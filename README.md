# GCONV-dataset

GCONV 半监督故障诊断实验使用的数据集仓库。

本仓库只提供数据集下载，不包含训练代码。数据文件以 GitHub Release 附件形式发布。

## Download

Release 页面：
- [GCONV dataset v1.0](https://github.com/mailier1973/GCONV-dataset/releases/tag/v1.0)

直接下载：
- [small_processed_data.mat](https://github.com/mailier1973/GCONV-dataset/releases/download/v1.0/small_processed_data.mat)
- [big_processed_data.mat](https://github.com/mailier1973/GCONV-dataset/releases/download/v1.0/big_processed_data.mat)
- [black_processed_data.mat](https://github.com/mailier1973/GCONV-dataset/releases/download/v1.0/black_processed_data.mat)

## Dataset Overview

该数据集包含 3 个预处理后的 `.mat` 文件：

- `small_processed_data.mat`
- `big_processed_data.mat`
- `black_processed_data.mat`

每个数据文件均包含以下字段：

- `trainData`
- `trainLabels`
- `valData`
- `valLabels`
- `testData`
- `testLabels`

## Dataset Split

| Dataset | Classes | Train | Val | Test | Total |
| --- | ---: | ---: | ---: | ---: | ---: |
| small | 3 | 3132 | 392 | 391 | 3915 |
| big | 3 | 3150 | 394 | 394 | 3938 |
| black | 5 | 5760 | 720 | 720 | 7200 |

每个样本长度为 `64000` 点。

## Class Distribution

### small
- Train: `[1059, 1054, 1019]`
- Val: `[104, 146, 142]`
- Test: `[121, 132, 138]`

### big
- Train: `[1045, 1106, 999]`
- Val: `[125, 133, 136]`
- Test: `[127, 133, 134]`

### black
- Train: `[1173, 1135, 1126, 1169, 1157]`
- Val: `[134, 164, 156, 133, 133]`
- Test: `[133, 141, 158, 138, 150]`

## Loading Notes

- `small` 和 `big` 可使用 `scipy.io.loadmat` 直接加载
- `black` 建议使用 `h5py` 加载
- 标签在原始文件中为从 `1` 开始的编号，使用时可转换为从 `0` 开始：
  - `labels = labels.ravel() - 1`
- `black` 数据集加载后通常需要转置：
  - `data = np.array(f['trainData']).T`

## Example

```python
import scipy.io as scio
import h5py
import numpy as np

# small / big
mat = scio.loadmat('small_processed_data.mat')
train_data = mat['trainData'].astype(np.float32)
train_labels = mat['trainLabels'].ravel() - 1

# black
with h5py.File('black_processed_data.mat', 'r') as f:
    train_data = np.array(f['trainData']).T.astype(np.float32)
    train_labels = np.array(f['trainLabels']).ravel() - 1
