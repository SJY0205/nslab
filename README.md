# nslab

用于在首次拿到测控设备后，快速搭建可运行的 Python 测控环境：统一声明依赖版本（含 `nsqdriver`），并配套 Jupyter 教程，便于按步骤连接设备、配置同步与波形实验。

## 适用对象与前提

- **目标用户**：需要在本地用 Python + Jupyter 操作 RIGOL 测控硬件（如 PowerQ / MCI 等）的研发与实验人员。
- **运行环境**：Windows / Linux / macOS 均可，需 **Python 3.13 或更高版本**（与 `pyproject.toml` 中 `requires-python` 一致）。
- **网络**：电脑与设备处于可互通的局域网，并已知 **设备 IP**（及 QSYNC 所用 IP，可与主设备相同或按现场划分）。
- **工具**：本说明假定使用 **[uv](https://docs.astral.sh/uv/)** 管理依赖与运行命令；安装方式见 [uv 安装文档](https://docs.astral.sh/uv/getting-started/installation/)。

## 仓库里有什么

| 内容 | 说明 |
|------|------|
| `pyproject.toml` | 项目元数据与依赖列表（含 `nsqdriver==0.12.18` 等）。 |
| `uv.lock` | 锁定依赖版本，与 `uv sync` 配合可复现相同环境。 |
| `PowerQ_UG_0225_备份不动.ipynb` | **主教程**：从系统初始化到 PQ-XY / PQ-ZC / PQ-RD 及多种标定与测量示例。 |
| `main.py` | 占位入口；日常测控以笔记本为主。 |

## 使用 uv 安装与运行

### 1. 准备 Python

终端中确认已安装 Python 3.13+：

```bash
python --version
```

### 2. 获取代码

将仓库地址换成你的 GitHub 地址：

```bash
git clone <你的仓库 URL>
cd nslab
```

### 3. 同步依赖（创建/更新虚拟环境）

在仓库根目录执行：

```bash
uv sync
```

会根据 `pyproject.toml` 与 `uv.lock` 安装依赖，并管理项目虚拟环境（默认 `.venv`）。

### 4. 启动 Jupyter Lab

仍在仓库根目录：

```bash
uv run jupyter lab
```

浏览器打开界面后，打开 **`PowerQ_UG_0225_备份不动.ipynb`**。若笔记本内核列表里没有本项目环境，可在 Jupyter 内核选择器里选带 `.venv` 的 Python，或在项目根目录执行一次：

```bash
uv run python -m ipykernel install --user --name nslab
```

之后在笔记本中选择名为 **nslab** 的内核即可。

## 使用教程笔记本：第一步要改什么

1. 打开 `PowerQ_UG_0225_备份不动.ipynb`。
2. 在 **「3.1 系统初始化」** 的代码单元中，把示例 IP 改成现场地址：
   - `deviceIP`：主控 / MCI 设备 IP  
   - `qsync_ip`：QSYNC 设备 IP（可与主设备相同，视硬件而定）
3. 按顺序执行：先完成 **QSYNC 与 MCI 的 `open` 与 `sync_system`**，再按需执行后续 **PQ-XY / PQ-ZC / PQ-RD** 及各测量章节。

笔记本内章节较多，主要包括（以笔记本内标题为准）：

- **3.1** 系统初始化  
- **3.2** PQ-XY（波形播放、连续波、延迟等）  
- **3.3** PQ-ZC  
- **3.4** PQ-RD  
- **4.x / 11.x** 等：S21、谱、Rabi、T1、Ramsey 等示例  

请在充分理解硬件与参数含义的前提下修改 `sysparam`、`qsync_param`、通道名与采样率等；错误配置可能导致设备行为与预期不符。

## 依赖说明（摘要）

- **`nsqdriver`**：设备通信驱动（当前锁定 `0.12.18`）。  
- **`jupyterlab`**、**`ipykernel`**、**`ipywidgets`**：交互式教程。  
- **`numpy`**、**`matplotlib`**、**`tqdm`**：计算、绘图与进度。

更新依赖时，请同步维护 `pyproject.toml` 与 `uv.lock`（例如使用 `uv lock` / `uv sync`），并在目标硬件上回归测试。

## 常见问题

- **连不上设备**：检查网段、防火墙、IP；可在本机 `ping <设备IP>` 做粗测。  
- **导入或内核报错**：确认 Jupyter 使用的是本仓库通过 `uv sync` 安装依赖后的解释器；优先用 `uv run jupyter lab` 启动。  
- **Python 版本过低**：本项目要求 **≥3.13**，请先升级 Python 再执行 `uv sync`。

更完整的说明（背景、目录、注意事项与排查表）见 **[docs/使用说明.md](docs/使用说明.md)**。
