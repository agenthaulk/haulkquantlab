# Qlib 安装指南

## 方式 1: 虚拟环境安装（推荐）

```bash
# 创建虚拟环境
cd ~/Desktop/openclaw\ space/haulkquantlab
python3 -m venv venv
source venv/bin/activate

# 安装 Qlib
pip install pyqlib

# 初始化数据（A 股）
python -m qlib.run.get_data qlib_data --target_dir ./data/qlib_data/cn_data --region cn
```

## 方式 2: Conda 安装

```bash
# 创建 conda 环境
conda create -n qlib python=3.10
conda activate qlib

# 安装 Qlib
pip install pyqlib

# 初始化数据
python -m qlib.run.get_data qlib_data --target_dir ./data/qlib_data/cn_data --region cn
```

## 验证安装

```bash
# 测试 Qlib 是否正常
python -c "import qlib; print(qlib.__version__)"
```

---

*注意*: macOS 系统需要先创建虚拟环境才能安装 pip 包
