# 🚀 AI Composite Engineering & Self-Study Roadmap

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.12%2B-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C.svg?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.40%2B-FF4B4B.svg?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115%2B-009688.svg?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![uv](https://img.shields.io/badge/Package_Manager-uv-blueviolet.svg)](https://github.com/astral-sh/uv)
[![Datawhale](https://img.shields.io/badge/Ecosystem-Datawhale-orange.svg)](https://github.com/datawhalechina)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**面向未来深造（保研推免 / 考研复试 / 名校进组）与前沿就业（大模型 / Agent / CV / NLP / ML 算法与工程）的 T 型复合自学仓库**

[📋 完整30周自学计划 (Learning Plan)](Learning_plan.md) • [📖 开发者操作指南 (Guides)](#-开发者操作指南索引-learning_file) • [⚡ 快速上手 (Quick Start)](#-快速环境搭建-quick-start)

</div>

---

## 📌 项目定位与核心理念

本项目是一个面向**计算机与人工智能专业本科生**的现代自学与实践工程仓库。旨在打破传统狭隘的单一赛道死锁（如过早耗费数月纯手工切图），建立具有高适应性、高爆发力的 **T 型复合竞争力模型**：

```
                      ┌────────────────────────────────────────┐
                      │    本科生终极竞争力评估模型 (T型架构)  │
                      └──────────────────┬─────────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
       【竖杠：理论与算法上限 (Ceiling)】                   【横杠：敏捷工程落地底线 (Floor)】
       · 西瓜书/南瓜书统计学习与优化理论                    · 现代 Python (uv) + Linux + Git
       · PyTorch 深入浅出 (thorough-pytorch)                · Streamlit 极速原型可视化 (1-2天出Demo)
       · Transformer 统一数学与网络架构 (base/happy-llm)    · FastAPI 高性能接口封装
       · 懂模型边界、损失函数、权重变化与调优               · Docker 容器化打包与一键交付
       ─────────────────────────────────                ─────────────────────────────────
       【决定你能走多高，能否进顶尖组/大厂算法】        【决定你能否把论文/想法快速做成可用产品】
```

1. **课内课外深度共振：** 深度结合高校《机器学习》必修课（周志华《西瓜书》），利用 Datawhale《南瓜书》推导公式，用 PyTorch 亲手跑通算法，实现“期末 90+ 高绩点 + 算法真内功”双赢。
2. **极速原型交付：** 采用 100% 纯 Python 的 **Streamlit** 作为第一梯队交互界面，只需几行代码即可将算法转化为可交互 Web 系统并免费发布至 Hugging Face Spaces，告别繁重前端负担。
3. **直通大模型与智能体前沿：** 从 Transformer 底层机制（Multi-Head Attention、RoPE、KV Cache）贯穿至小模型预训练、LoRA 微调，进阶到 ReAct、LangGraph 状态机及 MCP 智能体协议。

---

## 📂 仓库目录结构

```
.
├── Learning_plan.md            # 核心总纲：AI 复合型本科生 30 周全景成长计划 (3.0 终极版)
├── README.md                   # 仓库主页与导航索引（本文档）
├── .gitignore                  # 工业级 Python / PyTorch / Web 忽略规则配置
│
├── learning_file/              # 核心工程化操作指南与语法速查手册
│   ├── git_workflow_guide.md   # Git 与 GitHub 完整工程化操作指南 (工作区/分支/PR/CR/Actions)
│   ├── shell_basics_guide.md   # Shell 基础与现代开发者工具指南 (Bash + PowerShell + GPU监控 + uv)
│   ├── markdown_syntax_guide.md# Markdown 完整语法与 LaTeX/Mermaid/GitHub Alerts 规范
│   ├── html_syntax_guide.md    # HTML5 完整语法与语义化规范指南 (标签/表单/多媒体/无障碍)
│   └── css_syntax_guide.md     # CSS3 完整语法与现代响应式布局指南 (Flexbox/Grid/变量/媒体查询)
│
└── html_css/                   # 历史静态前端实践组件（已封存归档，供拓展参考）
    ├── personal_UI/            # 个人主页静态布局实践 (带头像、卡片与基础样式)
    └── Register_UI/            # 用户注册表单交互界面实践 (含表单控件与组件化重构模型)
```

---

## 📚 开发者操作指南索引 (`learning_file/`)

仓库精选并沉淀了 5 份极为详尽的操作与语法指南，帮助你在学习过程中随时随地秒查指令与规范：

| 指南文档 | 涵盖核心内容 | 重点价值与推荐查阅场景 |
| :--- | :--- | :--- |
| 📘 **[`git_workflow_guide.md`](learning_file/git_workflow_guide.md)** | 四大工作区、Conventional Commits 提交规范、分支策略、Pull Request 流程、Code Review 准则、GitHub Actions CI/CD、Windows 中文路径防乱码配置 | 团队协作、提交代码、规避代码冲突与工程化规范必备 |
| 💻 **[`shell_basics_guide.md`](learning_file/shell_basics_guide.md)** | Linux Bash 与 Windows PowerShell 双栈对比、管道与重定向、进程管理、自动化脚本模板、**GPU `nvidia-smi` 显存监控、`uv` 极速包管理、WSL2 常用命令** | 跑实验、服务器远程操作、显卡监控与现代工具链实操 |
| 📝 **[`markdown_syntax_guide.md`](learning_file/markdown_syntax_guide.md)** | 基础排版、表格与任务清单、Mermaid 流程图、**GitHub 原生警示块 (Alerts)、机器学习与深度学习 LaTeX 核心公式排版** (Softmax, Attention, Loss) | 撰写技术博客、整理算法推导笔记、美化 GitHub 项目主页 |
| 🌐 **[`html_syntax_guide.md`](learning_file/html_syntax_guide.md)** | HTML5 语义化标签、`<form>` 表单控件与输入校验、多媒体嵌入、SEO 基础与无障碍访问 (a11y) | 了解 Web 骨架原理、爬虫数据解析与网页交互基础 |
| 🎨 **[`css_syntax_guide.md`](learning_file/css_syntax_guide.md)** | 盒模型机制、Flexbox 弹性盒子布局、CSS Grid 网格系统、CSS 变量、媒体查询响应式设计、过渡与关键帧动画 | 调试页面排版、美化交互组件与移动端适配 |

---

## 🗺️ 30周学习路线概览 (Roadmap Summary)

详细的周度任务拆解、推荐资料链接与交付检查表，请直接精读 **[Learning_plan.md](Learning_plan.md)**：

```
┌────────────────────────────────────────────────────────────────────────┐
│ Phase 0：现代工程师工具箱 (第 1-2 周)                                  │
│  · Python 3.12 (uv) + Linux Shell 核心 + Git / GitHub 规范工作流       │
├────────────────────────────────────────────────────────────────────────┤
│ Phase 1：机器学习理论底座【击穿西瓜书】 (第 3-7 周)                    │
│  · 周志华《西瓜书》+ Datawhale《南瓜书》矩阵公式手推 + NumPy 算法复现  │
├────────────────────────────────────────────────────────────────────────┤
│ Phase 2：深度学习通用武器库与 PyTorch 核心 (第 8-12 周)                │
│  · Datawhale thorough-pytorch：张量/Autograd/ResNet/工业级训练循环模板 │
├────────────────────────────────────────────────────────────────────────┤
│ Phase 3：Streamlit 极速原型开发与服务化 (第 13-15 周)                  │
│  · 纯 Python 构建 AI 交互系统 + 西瓜书调参实验台 + FastAPI 异步接口封装│
├────────────────────────────────────────────────────────────────────────┤
│ Phase 4：Transformer 统一架构与大模型底层 (第 16-21 周)                │
│  · Datawhale base-llm + happy-llm + self-llm：注意力机制/LoRA/Ollama   │
├────────────────────────────────────────────────────────────────────────┤
│ Phase 5：智能体 Agent 与复合系统工程 (第 22-26 周)                     │
│  · Datawhale tiny-universe (手搓白盒) + hello-agents (LangGraph + MCP) │
├────────────────────────────────────────────────────────────────────────┤
│ Phase 6：毕业级标杆作品集与科研/求职破局 (第 27-30 周)                 │
│  · DeepResearch 自主研究智能体 / 垂直多模态系统 + 完整开源规范与 Demo  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## ⚡ 快速环境搭建 (Quick Start)

本项目推荐使用新一代极致快速的 Python 包与虚拟环境管理器 **`uv`**（由 Astral 打造，速度比 pip/conda 快 10~100 倍）：

### 1. 克隆本仓库到本地
```bash
git clone https://github.com/Equinox-HHan/AI_Full_Stack.git
cd FullStack
```

### 2. 创建并激活独立的虚拟环境
```bash
# 秒级创建 Python 3.12 虚拟环境
uv venv

# 激活环境
# Linux / macOS:
source .venv/bin/activate
# Windows PowerShell:
.venv\Scripts\Activate.ps1
```

### 3. 安装常用开发与 AI 核心依赖
```bash
# 极速并行安装依赖
uv pip install torch torchvision streamlit fastapi uvicorn scikit-learn pandas matplotlib
```

### 4. 验证 PyTorch 与 CUDA 支持
```bash
python -c "import torch; print('PyTorch Version:', torch.__version__); print('CUDA Available:', torch.cuda.is_available())"
```

---

## 🤝 致谢与权威生态参考

- **[Datawhale 开源社区](https://github.com/datawhalechina)**：感谢南瓜书 (`pumpkin-book`)、深入浅出 PyTorch (`thorough-pytorch`)、从零构建智能体 (`hello-agents`)、动手学大模型 (`happy-llm`)、大模型微宇宙 (`tiny-universe`) 等杰出开源项目为学习者铺平道路。
- **[University of Helsinki: Full Stack Open](https://fullstackopen.com/en/)**：国际一流的现代化全栈工程教学体系。
- **[MIT 6.1040: The Missing Semester](https://missing.csail.mit.edu/)**：计算机科学家与工程师的终身工具箱素养。

---

## 📄 许可证

本项目遵循 [MIT License](LICENSE) 开源协议。欢迎学习交流、Fork 与 Star！ ⭐
