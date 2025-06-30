---
title: 在内网环境安装 PaddleSpeech 实现语音合成服务
tags:
  - 汇总
  - paddle-speech
categories:
  - 汇总
---

## 简单方式安装（失败）

sudo apt install build-essential

sudo apt install python3-pip

pip install pytest-runner -i https://pypi.tuna.tsinghua.edu.cn/simple

pip install paddlespeech -i https://pypi.tuna.tsinghua.edu.cn/simple 

## docker安装（不支持M1）

docker run --name dev -v $PWD:/mnt -p 8888:8888 -it paddlecloud/paddlespeech:develop-cpu-fb4d25  /bin/bash

## root权限的Ubuntu安装（失败）

查看你的系统架构

```bash
uname -m
```

> aarch64

安装 Conda

**aarch64**

```bash
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-aarch64.sh -P tools/

bash ./tools/Miniconda3-latest-Linux-aarch64.sh -b

$HOME/miniconda3/bin/conda init

bash

conda create -y -p tools/venv python=3.7

conda activate tools/venv

conda install -y -c conda-forge sox libsndfile swig bzip2 libflac bc

pip install pytest-runner -i https://pypi.tuna.tsinghua.edu.cn/simple 

pip install -e .[develop] -i https://pypi.tuna.tsinghua.edu.cn/simple
```

**x86_64**

```bash
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -P tools/
```

## Mac M1安装

[下载 conda 的 sh 文件](https://docs.conda.io/en/latest/miniconda.html)

```bash
bash 你下载的sh文件

conda list

conda install -y -c conda-forge sox libsndfile bzip2

brew install gcc

pip install pytest-runner -i https://pypi.tuna.tsinghua.edu.cn/simple 

pip install paddlepaddle -i https://mirror.baidu.com/pypi/simple

pip install paddlespeech -i https://pypi.tuna.tsinghua.edu.cn/simple 
```

## 解决方案

通过 conda 创建虚拟环境

```bash
# 創建一個可以安裝intel包的名為ppocr_rosetta的虛擬環境
CONDA_SUBDIR=osx-64 conda create -n ppocr_rosetta python=3.7

# 激活該環境
conda activate ppocr_rosetta

# 驗證該環境支持平台
python -c "import platform;print(platform.machine())"

# 確保該環境為創建的包為intel架構所用
conda env config vars set CONDA_SUBDIR=osx-64

# 退出該環境
conda deactivate

# 重新激活該環境
conda activate ppocr_rosetta

# 查看環境變量，確定是osx-64,支持intel包
echo "CONDA_SUBDIR: $CONDA_SUBDIR"

# 安裝paddlepaddle包
python -m pip install paddlepaddle -i https://mirror.baidu.com/pypi/simple
```

进入 python 验证环境

```bash
python
```

```bash
>>> import paddle
>>> paddle.utils.run_check()
Running verify PaddlePaddle program ... 
PaddlePaddle works well on 1 CPU.
W0904 23:21:10.721201 9092608 fuse_all_reduce_op_pass.cc:76] Find all_reduce operators: 2. To make the speed faster, some all_reduce ops are fused during training, after fusion, the number of all_reduce ops is 2.
PaddlePaddle works well on 2 CPUs.
PaddlePaddle is installed successfully! Let's start deep learning with PaddlePaddle now.
```

> 如果 import 报错，运行下面命令降级   
> pip install protobuf==3.20.1