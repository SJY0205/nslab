# nslab

用于在首次拿到测控设备后，快速搭建可运行的 Python 测控环境：统一声明依赖版本（含 `nsqdriver`），并配套 Jupyter 教程，便于按步骤连接设备、配置同步与波形实验。

## 适用对象与前提

- **目标用户**：需要在本地用 Python + Jupyter 操作 RIGOL 测控硬件（如 PowerQ / MCI 等）的研发与实验人员。
- **运行环境**：Windows / Linux / macOS 均可，需安装python。
- **网络**：电脑与设备处于可互通的局域网，并已知 **设备 IP**（及 QSYNC 所用 IP，可与主设备相同或按现场划分）。
- **工具**：使用 **[uv](https://docs.astral.sh/uv/)** 管理依赖与运行命令；安装方式见 [uv 安装文档](https://docs.astral.sh/uv/getting-started/installation/)。

## 使用 uv 安装与运行

### 1. 准备

终端中确认已安装 Python和 uv：

```bash
python --version
uv -V
```

### 2. 安装

在终端打开：

```bash
git clone https://github.com/SJY0205/nslab.git
cd nslab
```

### 3. 同步依赖（创建/更新虚拟环境）

在仓库根目录执行：

```bash
uv sync
```

### 4. 启动 Jupyter Lab

仍在仓库根目录：

```bash
uv run jupyter lab
```

浏览器打开界面后，打开 **`Program/PowerQ_UG_0225_备份不动.ipynb`**等，其中包括各种操作历程。

## 使用教程

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

## 常见问题

- **连不上设备**：检查网段、防火墙、IP；可在本机 `ping <设备IP>` 做粗测。  
- **导入或内核报错**：确认 Jupyter 使用的是本仓库通过 `uv sync` 安装依赖后的解释器；优先用 `uv run jupyter lab` 启动。  
- **Python 版本过低**：本项目要求 **≥3.13**，请先升级 Python 再执行 `uv sync`。

更完整的说明（背景、目录、注意事项与排查表）见 **[docs/使用说明.md](docs/使用说明.md)**。
