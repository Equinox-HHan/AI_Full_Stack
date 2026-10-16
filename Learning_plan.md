# 🎯 AI 复合型卓越本科生全景成长计划 (Learning Plan)

> **版本：** 3.0 (终极重构版)  
> **面向对象：** 计算机 / 人工智能专业本科生  
> **核心定位：** 拒绝过早死锁单一狭窄赛道，在本科阶段建立**“硬核数学算法底座 + 现代 AI 统一通识 + 极简敏捷工程交付”**的 T 型核心竞争力，最大化未来深造（保研推免、考研复试、名校进组）与前沿就业（大模型、Agent、CV、NLP、ML 算法及工程）的选择权与爆发力。  
> **学习模式：** **从零全新启程**（以前的 HTML/CSS 作为附录拓展库，不占用主线精力）  
> **时间节奏：** 日常 1.5 - 2 小时 / 天，周末 3 - 4 小时 / 天（周均 12 - 16 小时），总周期约 30 周（以可运行的代码与成果物驱动）。

---

## 目录

1. [全新航标：本科生如何获得“最大化选择权”？](#一全新航标本科生如何获得最大化选择权)
2. [全景技术矩阵与顶级开源生态](#二全景技术矩阵与顶级开源生态)
3. [总体全景路线图 (Roadmap)](#三总体全景路线图-roadmap)
4. [分阶段详细周计划与动手落地](#四分阶段详细周计划与动手落地)
   - [Phase 0：现代工程师工具箱与环境（第 1-2 周）](#phase-0现代工程师工具箱与环境第-1-2-周)
   - [Phase 1：机器学习数学底座与经典算法【击穿西瓜书】（第 3-7 周）](#phase-1机器学习数学底座与经典算法击穿西瓜书第-3-7-周)
   - [Phase 2：深度学习通用武器库与 PyTorch 核心（第 8-12 周）](#phase-2深度学习通用武器库与-pytorch-核心第-8-12-周)
   - [Phase 3：Streamlit 极速原型开发与服务化（第 13-15 周）](#phase-3streamlit-极速原型开发与服务化第-13-15-周)
   - [Phase 4：Transformer 统一架构与大模型底层（第 16-21 周）](#phase-4transformer-统一架构与大模型底层第-16-21-周)
   - [Phase 5：智能体 Agent 与复合系统工程（第 22-26 周）](#phase-5智能体-agent-与复合系统工程第-22-26-周)
   - [Phase 6：毕业级标杆作品集与求职/科研破局（第 27-30 周）](#phase-6毕业级标杆作品集与求职科研破局第-27-30-周)
5. [高校核心课程协同专栏（保绩点 + 涨实力的双赢打法）](#五高校核心课程协同专栏保绩点--涨实力的双赢打法)
6. [附录：拓展知识库（按需选修）](#六附录拓展知识库按需选修)
   - [附录 A：商业级前端 Web 选修（React + TypeScript）](#附录-a商业级前端-web-选修react--typescript)
   - [附录 B：高校 Java / Spring Boot 课程协同备忘](#附录-b高校-java--spring-boot-课程协同备忘)
7. [避坑指南、自律系统与里程碑检查表](#七避坑指南自律系统与里程碑检查表)

---

## 一、全新航标：本科生如何获得“最大化选择权”？

在本科尚未确定终身方向时，最可怕的不是“不知道选大模型、CV 还是 Agent”，而是**“过早将所有时间耗费在纯手工前端切图和 CRUD 业务中，导致算法理论与深度学习底座一片空白”**。

名校导师招收保研/考研学生、顶级大厂招聘核心算法与系统研发时，评估模型高度一致：

```
                      ┌────────────────────────────────────────┐
                      │    本科生终极竞争力评估模型 (T型架构)     │
                      └──────────────────┬─────────────────────┘
                                         │
                 ┌───────────────────────┴───────────────────────┐
                 ▼                                               ▼
       【竖杠：理论与算法上限 (Ceiling)】              【横杠：敏捷工程落地底线 (Floor)】
       · 西瓜书/南瓜书统计学习与优化理论                 · 现代 Python (uv) + Linux + Git
       · PyTorch 深入浅出 (thorough-pytorch)            · Streamlit 极速原型可视化 (1-2天出Demo)
       · Transformer 统一数学与网络架构 (base/happy-llm) · FastAPI 高性能接口封装
       · 懂模型边界、损失函数、权重变化与调优             · Docker 容器化打包与一键交付
       ─────────────────────────────────                ─────────────────────────────────
       【决定你能走多高，能否进顶尖组/大厂算法】        【决定你能否把论文/想法快速做成可用产品】
```

**本计划核心法则：**
1. **清空历史包袱：** 彻底抛弃“从头刷几周 HTML/CSS”的心理负担，全栈能力全部转向以 **Python 为核心的极速流（Streamlit + FastAPI）**。
2. **理论课内共振：** 借力这学期正在学的周志华《西瓜书》，用 Datawhale 开源矩阵打一场漂亮的“期末高绩点 + 算法真内功”双赢战役。
3. **直通现代 AI 核心：** 把省下的宝贵时间，全力注入 **PyTorch $\rightarrow$ Transformer $\rightarrow$ 大模型微调 $\rightarrow$ Agent 智能体** 的主流赛道。

---

## 二、全景技术矩阵与顶级开源生态

本计划直接联动当前国内认可度最高的 Datawhale 顶级开源教材体系，形成严丝合缝的技术链条：

| 领域 / 阶段 | 核心技术栈 | 推荐权威开源教材与实战库 | 角色与价值 |
| :--- | :--- | :--- | :--- |
| **基础工程底座** | Python 3.12+ / `uv` / Git / Linux | MIT *The Missing Semester* | 现代开发基石，告别环境混乱 |
| **统计机器学习** | 凸优化 / 决策树 / SVM / 集成学习 | **Datawhale: `pumpkin-book`（南瓜书）** + 西瓜书 | **期末 90+ 绩点**，手推公式终极利器 |
| **深度学习框架** | **PyTorch 2.x** / 张量计算 / 自动求导 | **Datawhale: `thorough-pytorch`（深入浅出PyTorch）** | 把抽象公式转化为 GPU 上飞驰的代码 |
| **极速原型交付** | **Streamlit** / 纯 Python 响应式 Web | **Streamlit 官方实战** + Hugging Face Spaces | 10 行代码出交互界面，论文/大作业神器 |
| **后端服务化** | **FastAPI** + Pydantic v2 + 异步并发 | FastAPI Official Tutorial | 现代 Python 接口标准，算法包装利器 |
| **现代大模型底座** | NLP 演进 / Transformer / 预训练微调 | **Datawhale: `base-llm`** + **`happy-llm`** | 深入模型内部，告别只会调包的黑盒思维 |
| **本地大模型部署** | Ollama / vLLM / GGUF / LoRA | **Datawhale: `self-llm`（开源大模型食用指南）** | Linux/显卡上跑通 Qwen2.5/DeepSeek |
| **智能体系统工程** | ReAct / Tool Use / LangGraph / MCP | **Datawhale: `hello-agents`** + **`tiny-universe`** | 50行代码手搓白盒，进阶工业级 Agent |

---

## 三、总体全景路线图 (Roadmap)

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 0：现代工程师工具箱 (2周) Python 3.12 (uv)+Linux Shell+Git/GitHub 工作流   │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 1：机器学习理论底座 (5周) 《西瓜书》+Datawhale《南瓜书》公式手推与实现     │
│ 【产出】：西瓜书重难点算法推导笔记 + 基于 NumPy/sklearn 的经典算法库             │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 2：深度学习与 PyTorch 核心 (5周) Datawhale thorough-pytorch 深入实战       │
│ 【产出】：从零搭建多层网络、ResNet 图像分类、训练循环模板与 TensorBoard 可视化   │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 3：Streamlit 极速原型交互 (3周)  纯 Python 快速构建 AI 交互台与FastAPI服务 │
│ 【产出】：西瓜书算法调参可视化平台 + Fashion-MNIST 实时预测面板 + HF Spaces 部署 │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 4：Transformer 与大模型底层 (6周) Datawhale base-llm + happy-llm + self-llm│
│ 【产出】：手写 Multi-Head Attention、215M 模型预训练剖析、LoRA 微调与本地部署    │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 5：智能体 Agent 复合系统 (5周) Datawhale tiny-universe (手搓)+hello-agents │
│ 【产出】：手写 TinyRAG/TinyAgent、基于 LangGraph 状态机与MCP协议的复杂任务智能体 │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Phase 6：毕业级标杆作品集 (4周) DeepResearch 自主研究智能体 / 垂直多模态系统     │
│ 【产出】：具有商业级质感的开源项目、架构图、线上演示 Demo、技术博客复盘          │
└──────────────────────────────────────────────────────────────────────────────────┘
```

---

## 四、分阶段详细周计划与动手落地

---

### Phase 0：现代工程师工具箱与环境（第 1-2 周）

> **目标：** 从零建立现代化 Python 开发环境，掌握科学高效的命令行与版本控制，为后续跑实验扫清一切障碍。

- **第 1 周：现代 Python 3.12+ 与极速包管理器 `uv`**
  - **环境搭建：** 安装 VS Code，配置 Python 插件与代码格式化工具（Ruff）。
  - **抛弃旧工具：** 学习新一代 Python 包管理器 **`uv`**（秒级创建虚拟环境、安装依赖、锁定版本，彻底替代臃肿缓慢的 Anaconda 与旧 pip）。
  - **核心语法强化：** Python 面向对象（类、魔术方法）、类型注解（Type Hints）、装饰器原理、`async/await` 异步初探。
  - **动手任务：** 用 `uv` 创建独立工作空间，手写一个包含类型注解和装饰器计时器的实用工具脚本。

- **第 2 周：Linux Shell 命令行与 Git 协作工程流**
  - **Shell 基础：** 文件与路径导航、管道与重定向（`|`, `>`, `>>`）、文本处理（`grep`, `awk`, `cat`, `head`, `tail`）。
  - **Git 规范工作流：** 仓库初始化、提交原则（Conventional Commits: `feat:`, `fix:`, `docs:`）、分支创建与切换、冲突解决、推送到 GitHub。
  - **动手任务：** 本仓库初始化新分支，将练习代码规范提交并推送到远端，编写第一个结构清晰的 `README.md`。

---

### Phase 1：机器学习数学底座与经典算法【击穿西瓜书】（第 3-7 周）

> **目标：** 紧密联动高校正在开设的《机器学习》课程，以周志华《西瓜书》为主线，以 Datawhale《南瓜书》为公式解密钥匙，拿下高绩点，打牢数理底盘。

#### 核心资料
- **周志华《机器学习》（西瓜书）**（学校官方教材，理论圣经）
- **[Datawhale: pumpkin-book（南瓜书）](https://github.com/datawhalechina/pumpkin-book)**（公式推导官方外挂，必看！）
- [Scikit-learn 官方教程](https://scikit-learn.org/stable/)

#### 周度学习与动手计划
- **第 3 周：模型评估与线性模型**
  - **西瓜书第 2 章：** 留出法、交叉验证法、偏差-方差分解、准确率/查准率/查全率/F1-Score/ROC-AUC 曲线。
  - **西瓜书第 3 章 + 南瓜书：** 线性回归矩阵求导展开、对数几率回归（Logistic Regression）极大似然估计推导。
  - **代码实操：** 用 NumPy 手写对数几率回归的梯度下降求解过程，对比 `sklearn.linear_model.LogisticRegression`。

- **第 4 周：决策树与分类准则**
  - **西瓜书第 4 章：** 信息熵（Information Entropy）、信息增益（ID3）、增益率（C4.5）、基尼指数（CART）。
  - **核心难点：** 剪枝处理（预剪枝与后剪枝抑制过拟合）、连续值与缺失值处理机制。
  - **代码实操：** 用 Python 手写计算数据集的信息增益，实现一个简易版决策树分类器。

- **第 5 周：神经网络与经典 BP 反向传播**
  - **西瓜书第 5 章 + 南瓜书：** 神经元模型、感知机局限性、多层前馈网络架构。
  - **南瓜书重难点精读：** 标准 BP 算法与累积 BP 算法的矩阵偏导数一步一步推导（链式法则展开）。
  - **代码实操：** 用纯 NumPy 实现一个包含单隐层的多层感知机（MLP），手动计算梯度的反向传播。

- **第 6 周：支持向量机 (SVM) 与对偶理论**
  - **西瓜书第 6 章 + 南瓜书：** 间隔最大化超平面、拉格朗日乘子法构建拉格朗日函数、对偶问题转换。
  - **南瓜书重难点精读：** KKT 条件的几何意义与代数形式、核技巧（高斯核/RBF）原理。
  - **代码实操：** 使用 `sklearn.svm.SVC` 在非线性二维数据集上绘制分类边界与支持向量。

- **第 7 周：集成学习与聚类/降维**
  - **西瓜书第 8 章：** Boosting 核心（AdaBoost 算法权重分布调整公式推导）、Bagging 与随机森林（Random Forest）。
  - **西瓜书第 9/10 章：** K-Means 聚类、主成分分析（PCA）方差最大化推导。
  - **阶段交付：** 整理西瓜书核心算法推导思维导图，汇总成一份高含金量的期末复习宝典。

---

### Phase 2：深度学习通用武器库与 PyTorch 核心（第 8-12 周）

> **目标：** 告别黑盒调包，掌握国际学术与工业界的统一普通话——PyTorch，把数学公式转化为工程模型。

#### 核心资料
- **[Datawhale: thorough-pytorch（深入浅出 PyTorch）](https://github.com/datawhalechina/thorough-pytorch)**（核心主线实战）
- [PyTorch 官方教程 (pytorch.org/tutorials)](https://pytorch.org/tutorials/)

#### 周度学习与动手计划
- **第 8 周：PyTorch 基础与张量运算（`thorough-pytorch` 第 1-2 章）**
  - 张量创建、数据类型、索引切片、变形操作（`view`, `reshape`, `permute`）。
  - **广播机制（Broadcasting）：** 深入掌握张量维度自动对齐规则。
  - **Autograd 自动求导：** `requires_grad=True`、计算图（Computation Graph）、反向传播 `backward()` 与梯度清零 `zero_grad()`。
  - **硬件加速：** CUDA、cuDNN、张量在 CPU 与 GPU 之间的转移（`.to(device)`）。

- **第 9 周：核心模块与工业级训练循环（`thorough-pytorch` 第 3-5 章）**
  - 数据管道构建：`Dataset`（数据加载）与 `DataLoader`（Batch 批处理、Shuffle 打乱、多进程加载）。
  - 模型架构声明：继承 `nn.Module`，定义 `__init__` 与 `forward` 函数，常用层（`nn.Linear`, `nn.Conv2d`, `nn.Dropout`, `nn.BatchNorm2d`）。
  - 损失函数与优化器：交叉熵 `nn.CrossEntropyLoss`、AdamW 与 SGD 算法。
  - **工业级标准训练模板：** 手写包含 Train Loop、Validation Loop、Accuracy 计算、最佳权重保存的完整训练脚本。

- **第 10 周：经典架构源码与图像分类实战（`thorough-pytorch` 第 4 & 10 章）**
  - 在 **Fashion-MNIST** 或 CIFAR-10 数据集上完成一次端到端训练与验证。
  - 卷积神经网络（CNN）原理：感受野、卷积核、步长、池化。
  - **经典源码研读：** 深入剖析 **ResNet**（残差连接 Residual Connection 为何能解决网络退化问题）。

- **第 11 周：进阶训练技巧与可视化监控（`thorough-pytorch` 第 6-7 章）**
  - 动态学习率衰减：`lr_scheduler`（StepLR, CosineAnnealingLR）。
  - 混合精度加速（AMP）：`torch.cuda.amp.autocast`，显存减半且计算加速。
  - 模型微调（Fine-Tuning）：利用 `torchvision.models` 加载预训练权重进行迁移学习。
  - 训练可视化：使用 **TensorBoard** 或 **Weights & Biases (wandb)** 实时绘制 Loss 与 Acc 曲线。

- **第 12 周：PyTorch 生态与模型保存部署（`thorough-pytorch` 第 8-9 章）**
  - 权重保存与加载：`torch.save(model.state_dict())` vs 完整模型保存。
  - 模型推理优化：`model.eval()` 与 `with torch.no_grad():` 的核心避坑原则。
  - ONNX 格式导出初探：将 PyTorch 模型导出为跨平台通用的 `.onnx` 格式。
  - **里程碑达成：** 掌握扎实的 PyTorch 深度学习全链路，随时能复现经典论文！

---

### Phase 3：Streamlit 极速原型开发与服务化（第 13-15 周）

> **目标：** 拒绝耗费 2 个月去学复杂的 React 前端切图！用 100% 纯 Python 的 Streamlit，在 1-2 天内把任何算法模型做成高颜值交互 Web 界面，同时掌握高性能后端服务化。

#### 核心资料
- **[Streamlit 官方文档](https://docs.streamlit.io/)**
- **[FastAPI 官方教程](https://fastapi.tiangolo.com/zh/)**
- [Hugging Face Spaces 部署指南](https://huggingface.co/docs/hub/spaces)

#### 周度学习与动手计划
- **第 13 周：Streamlit 极速上手与算法可视化实验台**
  - Streamlit 运行机制：脚本自上而下的响应式数据流。
  - 核心交互控件：`st.slider`, `st.button`, `st.selectbox`, `st.file_uploader`, `st.columns` 栅格布局。
  - 数据与图表渲染：直接把 Pandas 表格、Matplotlib/Plotly 动态交互图表一键展示。
  - **实战项目：西瓜书机器学习交互实验台**
    - 用 Streamlit 搭建一个可视化页面：用户通过滑块调节逻辑回归的学习率与正则化系数，界面右侧实时刷新分类超平面与 Loss 下降曲线！

- **第 14 周：PyTorch 模型 Web 化与大模型流式对话实战**
  - **实战项目 1：Fashion-MNIST 图像分类实时预测系统**
    - 用户上传图片（或在线画板涂鸦），后台加载 Phase 2 训练好的 PyTorch 权重执行推理，前端动态输出 Top-3 概率分布柱状图。
  - **实战项目 2：Streamlit AI 流式聊天室**
    - 掌握 `st.session_state` 会话持久化、`st.chat_message` 对话气泡、`st.chat_input` 固定底部输入。
    - 用 `st.write_stream` 接入大模型 API，实现毫秒级打字机流式输出。
    - 将项目免费上线到 **Hugging Face Spaces**，生成公网链接，随时可供他人在线体验！

- **第 15 周：FastAPI 高性能异步后端服务化**
  - 为什么还需要 FastAPI？当模型需要为多个终端提供稳定 API 时，FastAPI 是业界标准。
  - 路径参数、查询参数、Pydantic v2 请求体结构化验证。
  - 异步处理：`async def` 与非阻塞 I/O。
  - 自动生成交互式接口文档（Swagger UI `/docs`）。
  - **阶段交付：** 搭建一个提供 `/predict` 与 `/chat` 接口的标准化 FastAPI 后端服务。

---

### Phase 4：Transformer 统一架构与大模型底层（第 16-21 周）

> **目标：** 攻破现代 AI 的核心命脉——Transformer 架构，从词向量切片、注意力机制到小模型预训练与 LoRA 微调，建立深度的算法壁垒。

#### 核心资料
- **[Datawhale: base-llm（从 NLP 到 LLM 的算法全栈教程）](https://github.com/datawhalechina/base-llm)**（演化脉络）
- **[Datawhale: happy-llm（动手学大模型）](https://github.com/datawhalechina/happy-llm)**（手写模型与微调实战）
- **[Datawhale: self-llm（开源大模型食用指南）](https://github.com/datawhalechina/self-llm)**（本地部署与高效推理）

#### 周度学习与动手计划
- **第 16 周：从传统 NLP 到词嵌入（`base-llm` 第 1-2 部分）**
  - 文本预处理与分词：BPE（Byte-Pair Encoding）、WordPiece 分词算法。
  - 词表示的演进：One-Hot 的局限性 $\rightarrow$ 词向量（Word2Vec / Skip-Gram / CBOW）的几何意义。
  - 为什么自然语言需要注意力机制：Seq2Seq 瓶颈与 RNN 长期依赖遗忘问题。

- **第 17 周：手撕 Transformer 核心架构（`happy-llm` 第 1-3 章）**
  - **自注意力机制（Self-Attention）：** Query, Key, Value 矩阵运算、缩放点积注意力（Scaled Dot-Product）公式推导与代码实现。
  - **多头注意力（Multi-Head Attention）：** 为什么需要多个 Head？维度切分与拼接。
  - **位置编码（Positional Encoding）：** 绝对正弦位置编码 vs 现代 RoPE（旋转位置编码）的数学原理。
  - 规范化层与残差：Pre-LN vs Post-LN、Feed-Forward Network (FFN)。

- **第 18 周：现代大模型三大架构范式与推理加速**
  - Encoder-only (BERT: 掩码语言模型)、Encoder-Decoder (T5)、Decoder-only (GPT, Llama, Qwen, DeepSeek 为何成为绝对主流？)。
  - 自回归生成机制（Auto-regressive Generation）与采样策略（Temperature, Top-P, Top-K, Repetition Penalty）。
  - **推理加速利器：** 为什么大模型推理需要 **KV Cache**？手算 KV Cache 显存占用与显存瓶颈分析。

- **第 19 周：动手手写 215M 小模型预训练剖析（`happy-llm` 实战）**
  - 研读 `happy-llm` 配套代码：从零定义一个 Decoder-only 模型结构。
  - 数据集准备：分词编码、`input_ids` 与 `labels` 构建。
  - 预训练训练循环：交叉熵损失计算、梯度累积（Gradient Accumulation）、学习率预热（Warmup）。

- **第 20 周：指令微调 (SFT) 与参数高效微调 (PEFT/LoRA)**
  - 预训练模型（Base）到对话模型（Instruct/Chat）的关键飞跃：SFT 对齐格式（System, User, Assistant Prompt Template）。
  - 全参数微调的算力灾难 $\rightarrow$ **LoRA（Low-Rank Adaptation）** 核心原理：低秩分解矩阵 $W = W_0 + B \times A$。
  - 使用 Hugging Face `peft` 库配合 `transformers` 在开源数据集上跑通一次轻量级 LoRA 微调。

- **第 21 周：开源大模型本地部署与推理加速（`self-llm` 实战）**
  - 结合 `self-llm`：使用 **Ollama** 本地运行 Qwen2.5 / DeepSeek 开源模型。
  - 了解高并发推理框架 **vLLM** 核心原理（PagedAttention 解决显存碎片化）。
  - 结合 Phase 3 的 Streamlit，将本地运行的大模型无缝连入前端对话界面！

---

### Phase 5：智能体 Agent 与复合系统工程（第 22-26 周）

> **目标：** 从“单次提问回答”进阶到“具备自主规划、工具调用、长期记忆与复杂协同”的下一代 AI 架构。

#### 核心资料
- **[Datawhale: tiny-universe（手搓大模型微宇宙）](https://github.com/datawhalechina/tiny-universe)**（必读 TinyRAG 与 TinyAgent 模块）
- **[Datawhale: hello-agents（从零开始构建智能体）](https://github.com/datawhalechina/hello-agents)**（中文社区顶级智能体全栈教材）
- [LangGraph 官方文档](https://langchain-ai.github.io/langgraph/)

#### 周度学习与动手计划
- **第 22 周：白盒手搓检索增强生成（TinyRAG 实战）**
  - 摒弃庞大的第三方封装库，研读 `tiny-universe/TinyRAG` 源码。
  - 用 50 行纯 Python 手写实现：文档切片（Chunking）、Embedding 向量化、余弦相似度计算与 Top-K 召回。
  - 进阶生产级：学习使用 PostgreSQL 的 **pgvector** 扩展构建向量索引。

- **第 23 周：白盒手搓 Agent 与工具调用本质（TinyAgent 实战）**
  - 大模型 Tool Use / Function Calling 底层协议交互流程（JSON Schema 定义函数参数）。
  - 研读 `tiny-universe/TinyAgent` 与 `hello-agents` 第一部分：剖析 **ReAct（Thought $\rightarrow$ Action $\rightarrow$ Observation）** 思考循环。
  - 手写让大模型自主判断何时调用计算器、天气 API、数据库查询工具。

- **第 24 周：高级 RAG 优化与检索重排**
  - 纯向量检索的缺陷与混合检索（Hybrid Search）：结合 BM25 关键词匹配与稠密向量相似度。
  - 交叉编码重排模型（Reranker，如 BGE-Reranker）的原理与二次筛选。
  - 上下文压缩与多轮对话历史的 Query 意图重写。

- **第 25 周：基于状态机的工作流编排（LangGraph）**
  - 传统链式调用（Chain）在复杂业务中的失控问题。
  - 状态图（StateGraph）核心概念：节点（Node）、边（Edge）、条件分支（Conditional Edge）、循环回退。
  - 实现一个具备“自我反思与循环纠错（Self-Correction）”的代码生成或分析 Agent。

- **第 26 周：新一代智能体通信协议（MCP）与评估**
  - 学习行业最新主流开放协议：**MCP (Model Context Protocol)**，理解客户端与工具服务端解耦架构。
  - 智能体系统评估与可观测性：集成 **Langfuse** 记录每一轮 Agent 规划的思维链与 Token 成本。
  - **里程碑达成：** 掌握构建企业级复杂自主智能体系统的核心方法论！

---

### Phase 6：毕业级标杆作品集与求职/科研破局（第 27-30 周）

> **目标：** 拒绝平庸、千篇一律的玩具级 Demo，打造 1 个极具说服力、可公开运行、代码规范极高的标杆级复合项目。

#### 推荐选题方向（3 选 1）
1. **选项 A（DeepResearch 自主深度研究与研报生成系统 - 对标 hello-agents 高级实战）：**
   - **业务场景：** 用户输入复杂行业或科研课题，Agent 自主拆分子任务、并发网络检索、长文清洗总结、自我反思验证，最终输出万字长文研报与交互图表。
   - **技术亮点：** LangGraph 状态机 + 异步并发搜集 + Streamlit 流式展示思维链 + PDF 导出。
2. **选项 B（多模态智能视觉问答与图表分析 Agent）：**
   - **业务场景：** 用户上传财务报表截图、科研论文插图，结合多模态大模型（如 Qwen2-VL）自动识别图表内容，编写并执行 Python 代码沙箱生成深度分析结果。
   - **技术亮点：** 多模态大模型 + 代码沙箱执行（Code Interpreter）+ 安全防御。
3. **选项 C（基于混合检索与知识图谱的垂直领域问答平台）：**
   - **业务场景：** 面向特定专业领域（如医学、法律、高数习题），结合 pgvector 混合检索、图数据库关联召回与文档精确引用高亮定位。
   - **技术亮点：** 混合检索 + 页面级高亮引用溯源 + 生产级 Docker 一键启动。

#### 交付标准（打造能让导师与面试官惊艳的 GitHub 仓库）
- [ ] **高颜值 README：** 包含系统架构图（Mermaid/SVG 矢量图）、功能演示动图（GIF）、在线体验 URL。
- [ ] **可一键复现：** 根目录附带清晰的依赖锁定文件（`uv.lock`）或 `docker-compose.yml`，任何人克隆后一行命令即可跑通。
- [ ] **深度技术复盘长文：** 在博客或知乎/掘金/GitHub 撰写 1-2 篇核心难点技术复盘（如《如何解决长文档 RAG 幻觉与 Agent 循环陷入死死锁》）。

---

## 五、高校核心课程协同专栏（保绩点 + 涨实力的双赢打法）

### 5.1 本学期《机器学习》（周志华西瓜书）通关指南
1. **心态摆正：** 西瓜书重在建立“统计学习”的学术直觉与严密数学证明，期末考试通常偏向推导与计算。
2. **无敌组合拳：**
   - 老师课堂推导听不懂 $\rightarrow$ **立即翻开《南瓜书》对应公式编号**，查看展开步骤；
   - 觉得书上算法抽象看不见摸不着 $\rightarrow$ **立即翻开 `thorough-pytorch`**，写 30 行代码在合成数据上跑一跑；
   - 期末大作业展示 $\rightarrow$ **用 Streamlit 写一个算法交互演示台**，直接在全班展示中形成断层式降维打击！

---

## 六、附录：拓展知识库（按需选修）

> **特别说明：** 以下内容均为**可选拓展库**，不属于主线必修。当你未来有特定需求时（如接商业外包、学校特定课程要求）再查阅。

### 附录 A：商业级前端 Web 选修（React + TypeScript）
- **适用场景：** 未来希望独立开发商业化 SaaS 软件、参与复杂的企业级前端项目开发。
- **核心资料：** [University of Helsinki: Full Stack Open](https://fullstackopen.com/en/) + [shadcn/ui](https://ui.shadcn.com/)。
- **推荐路径：** TypeScript 接口契约 $\rightarrow$ React 19 函数式组件与 Hooks $\rightarrow$ Tailwind CSS 原子化样式 $\rightarrow$ Axios/TanStack Query 对接后端。
- **此前掌握的 HTML/CSS 资产：** 本仓库已归档于 `html_css/personal_UI/` 和 `html_css/Register_UI/`，涵盖基础静态布局知识，忘掉语法时直接查阅 `learning_file/` 下的对应指南即可。

### 附录 B：高校 Java / Spring Boot 课程协同备忘
- **核心哲学：** 绝不让 Java 企业级繁复开发打乱你的 AI 主线。
- **快速映射：** FastAPI 的路由、模型与依赖注入在概念上完全镜像对应 Spring Boot 的 `@RestController`、`DTO` 与 `@Autowired`。课程期末大作业直接套用成熟脚手架（Spring Initializr）快速交差即可。

---

## 七、避坑指南、自律系统与里程碑检查表

### 7.1 必须避开的五大致命陷阱
1. ❌ **教程狂热陷阱（Tutorial Hell）：** 永远觉得自己没准备好，把 HTML/Python 语法反复看五遍也不敢动手写项目。**法则：学完 60% 必须立刻写代码验证！**
2. ❌ **盲目死磕无用前端细节：** 花三天时间去调一个按钮的阴影动画或微调 2 像素间距。**法则：做 AI 原型首选 Streamlit，一分钟搞定布局。**
3. ❌ **调包侠陷阱：** 只会无脑 `import langchain` 或调 API，被问到注意力矩阵维度、KV Cache 原理、损失函数时目瞪口呆。**法则：跟着 `tiny-universe` 和 `happy-llm` 必须亲手写一次白盒逻辑。**
4. ❌ **脱离 Git 的本地堆积：** 代码只在本地改，不打 commit，不写 README。**法则：每天完成功能立刻 push，养成规范工程师习惯。**
5. ❌ **孤军奋战死磕 Bug：** 卡在一个环境报错上超过 1 小时。**法则：学会阅读完整 Traceback，善用官方 Issue、StackOverflow 与 AI 工具高效排错。**

### 7.2 关键里程碑交付物检查表

| 时间节点 | 阶段里程碑 | 必须拿出的可检验物 |
| :--- | :--- | :--- |
| **第 2 周末** | Phase 0 达成 | 拥有规范的 GitHub 仓库，现代 Python 环境与 Git 工具链顺畅运转 |
| **第 7 周末** | Phase 1 达成 | 攻克西瓜书核心算法，拥有手推公式笔记与经典分类算法实现 |
| **第 12 周末** | Phase 2 达成 | 独立编写 PyTorch 训练循环，完成 Fashion-MNIST/ResNet 分类与权重保存 |
| **第 15 周末** | Phase 3 达成 | 上线首个可交互的 Streamlit 算法实验台并部署到 Hugging Face Spaces |
| **第 21 周末** | Phase 4 达成 | 深入理解 Multi-Head Attention，跑通小模型预训练剖析与本地 LoRA 微调 |
| **第 26 周末** | Phase 5 达成 | 手写实现白盒 TinyRAG 与 TinyAgent，掌握 LangGraph 复杂状态工作流 |
| **第 30 周末** | 终极毕业 | 拥有 1 个展示级的开源毕业级 AI 项目，包含演示 Demo 与架构长文 |

---

> 🚀 **现在就扬帆起航：**  
> 不需要等待所谓的“准备好”。过去的包袱全部清零，今天就在新环境中敲下你的第一行现代 Python 与 PyTorch 代码！
