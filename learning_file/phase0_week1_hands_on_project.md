# Phase 0 Week 1 Hands-on Project | 第一周动手实战：现代 Python 实用工具库 (AI-Toolkit)

> 紧密对齐 `Learning_plan.md` 第一周动手目标：“用 `uv` 创建独立工作空间，手写一个包含类型注解和装饰器计时器的实用工具脚本”。本实战指南带领你亲手从零搭建一个符合工业级开源规范的现代 Python 工具库，贯通 uv、面向对象魔术方法、强类型系统、高精度计时装饰器与异步并发。

---

## 目录

1. [实战目标与成果物规范](#一实战目标与成果物规范)
2. [项目架构与目录拓扑设计](#二项目架构与目录拓扑设计)
3. [Step 1：使用 uv 初始化现代独立工作空间](#三step-1使用-uv-初始化现代独立工作空间)
4. [Step 2：配置工业级 pyproject.toml 规范](#四step-2配置工业级-pyprojecttoml-规范)
5. [Step 3：核心模块 1 —— 生产级装饰器模块 (decorators.py)](#五step-3核心模块-1--生产级装饰器模块-decoratorspy)
6. [Step 4：核心模块 2 —— 面向对象与魔术方法数据集批处理器 (data_batch.py)](#六step-4核心模块-2--面向对象与魔术方法数据集批处理器-data_batchpy)
7. [Step 5：核心模块 3 —— 异步并发任务模拟调度器 (async_runner.py)](#七step-5核心模块-3--异步并发任务模拟调度器-async_runnerpy)
8. [Step 6：聚合控制台入口脚本 (main.py)](#八step-6聚合控制台入口脚本-mainpy)
9. [Step 7：运行、性能压测与代码质量门禁 (Ruff)](#九step-7运行性能压测与代码质量门禁-ruff)
10. [第一周项目交付自检清单](#十第一周项目交付自检清单)

---

## 一、实战目标与成果物规范

通过本项目的实操，你将获得以下核心能力与具体交付成果：

```
                    ┌────────────────────────────────────────┐
                    │      Phase 0 Week 1 实战成果物架构     │
                    └───────────────────┬────────────────────┘
                                        │
      ┌──────────────────┬──────────────┴─────┬──────────────────┐
      ▼                  ▼                    ▼                  ▼
【uv 独立工作空间】  【类型化装饰器】     【协议化数据容器】    【异步并发调度】
 pyproject.toml +     @timer 高精度测量    实现 __len__/__iter__ asyncio.TaskGroup 
 uv.lock 完美锁定     @retry 指数退避      实现 __getitem__      模拟多接口并发
```

- **成果物 1：** 一个使用 `uv` 初始化的现代化标准工程结构；
- **成果物 2：** 包含泛型类型注解（Type Hints）与 `@functools.wraps` 的高精度运行耗时统计装饰器 `@timer`；
- **成果物 3：** 遵从 Python 容器协议（`__len__`, `__getitem__`, `__repr__`）的通用数据批加载容器 `DataBatch`；
- **成果物 4：** 基于 Python 3.11+ `asyncio.TaskGroup` 实现的模拟批量异步并发调度器；
- **成果物 5：** 通过 Ruff 零警告（Zero-warning）代码质量与格式化检查。

---

## 二、项目架构与目录拓扑设计

我们将打造一个名为 `ai_toolkit` 的轻量实用库，工程目录结构如下：

```
ai_toolkit_workspace/
├── pyproject.toml              # 现代标准构建与依赖配置 (PEP 621)
├── README.md                   # 工具库说明文档
├── .python-version             # 锁定 Python 3.12 解释器
├── src/
│   └── ai_toolkit/
│       ├── __init__.py         # 导出对外核心接口
│       ├── decorators.py       # 高精度计时器与指数退避重试装饰器
│       ├── data_batch.py       # 基于魔术方法的样本批加载容器类
│       └── async_runner.py     # 异步并发执行与延迟评测
└── main.py                     # 综合实战演示与性能基准跑分入口
```

---

## 三、Step 1：使用 uv 初始化现代独立工作空间

打开终端（PowerShell 或 Bash），执行以下命令完成项目的极速初始化：

```powershell
# 1. 创建并进入实战工作区目录
mkdir ai_toolkit_workspace
cd ai_toolkit_workspace

# 2. 锁定 Python 3.12 版本
uv python pin 3.12

# 3. 使用 uv 初始化标准项目
uv init

# 4. 创建 src 目录结构
mkdir -p src/ai_toolkit
```

---

## 四、Step 2：配置工业级 pyproject.toml 规范

将项目根目录下的 `pyproject.toml` 修改为以下内容，规范化项目元数据与代码质量检查规则：

```toml
[project]
name = "ai-toolkit"
version = "0.1.0"
description = "Phase 0 Week 1 现代工程工具箱：类型化装饰器、魔术方法容器与异步调度"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "rich>=13.7.0",       # 终端高颜值彩色渲染与表格输出
]

[project.optional-dependencies]
dev = [
    "ruff>=0.4.0",        # 极速代码检查与格式化
    "mypy>=1.10.0",       # 严格静态类型校验
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

# 配置 Ruff 检查规则
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I", "N", "UP"]
```

安装依赖：
```powershell
uv add rich
uv add --dev ruff
```

---

## 五、Step 3：核心模块 1 —— 生产级装饰器模块 (`decorators.py`)

在 `src/ai_toolkit/decorators.py` 中编写包含严格类型注解、参数控制与元数据保护的装饰器：

```python
"""ai_toolkit.decorators

提供生产级通用装饰器：高精度计时器与指数退避重试器。
"""

import functools
import time
from typing import Any, Callable, ParamSpec, Type, TypeVar

# 使用 Python 泛型保留原函数的参数签名与返回值类型
P = ParamSpec("P")
R = TypeVar("R")


def timer(
    unit: str = "ms",
    verbose: bool = True,
) -> Callable[[Callable[P, R]], Callable[P, R]]:
    """高精度函数执行耗时统计装饰器。

    :param unit: 输出单位，'ms' 为毫秒，'s' 为秒，默认为 'ms'
    :param verbose: 是否在终端输出耗时日志
    :return: 装饰后的函数，保留原函数的类型签名与 docstring
    """

    def decorator(func: Callable[P, R]) -> Callable[P, R]:
        @functools.wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            start_time = time.perf_counter()
            result = func(*args, **kwargs)
            duration = time.perf_counter() - start_time

            if verbose:
                if unit.lower() == "ms":
                    display_time = f"{duration * 1000.0:.3f} ms"
                else:
                    display_time = f"{duration:.4f} s"
                print(f"⏱️  [Timer] 函数 `{func.__name__}` 执行耗时: {display_time}")

            return result

        return wrapper

    return decorator


def retry(
    max_attempts: int = 3,
    initial_delay: float = 0.5,
    backoff_factor: float = 2.0,
    exceptions: tuple[Type[BaseException], ...] = (Exception,),
) -> Callable[[Callable[P, R]], Callable[P, R]]:
    """API 调用与网络请求指数退避重试装饰器。

    :param max_attempts: 最大尝试次数
    :param initial_delay: 首次重试等待秒数
    :param backoff_factor: 延迟时间倍乘系数 (默认每次翻倍)
    :param exceptions: 触发重试的目标异常类型元组
    """

    def decorator(func: Callable[P, R]) -> Callable[P, R]:
        @functools.wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            delay = initial_delay
            last_err: BaseException | None = None

            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    last_err = e
                    if attempt == max_attempts:
                        print(
                            f"❌ [Retry-Out] 函数 `{func.__name__}` 达到最大重试次数 ({max_attempts})，放弃重试。"
                        )
                        raise
                    print(
                        f"⚠️ [Retry {attempt}/{max_attempts}] `{func.__name__}` 触发异常: {e}，"
                        f"{delay:.2f} 秒后重试..."
                    )
                    time.sleep(delay)
                    delay *= backoff_factor

            assert last_err is not None
            raise last_err

        return wrapper

    return decorator
```

---

## 六、Step 4：核心模块 2 —— 面向对象与魔术方法数据集批处理器 (`data_batch.py`)

在 `src/ai_toolkit/data_batch.py` 中编写符合 Python 序列协议与容器规范的通用数据载体，深刻体会 `__len__`, `__getitem__`, `__repr__`, `__iter__` 的魅力：

```python
"""ai_toolkit.data_batch

基于面向对象与 Python 容器协议构建的样本批处理载体。
"""

from dataclasses import dataclass
from typing import Generic, Iterator, TypeVar

T = TypeVar("T")


@dataclass(frozen=True)
class Sample(Generic[T]):
    """单条样本数据载体（不可变对象）"""

    sample_id: int
    payload: T
    label: int


class DataBatch(Generic[T]):
    """支持切片、索引与迭代的通用数据集批容器类"""

    def __init__(self, samples: list[Sample[T]]) -> None:
        if not isinstance(samples, list):
            raise TypeError("samples 必须是 list 类型")
        self._samples: list[Sample[T]] = samples

    def __len__(self) -> int:
        """支持 len(batch)"""
        return len(self._samples)

    def __getitem__(self, index: int | slice) -> Sample[T] | list[Sample[T]]:
        """支持 batch[0] 以及切片 batch[0:2]"""
        return self._samples[index]

    def __iter__(self) -> Iterator[Sample[T]]:
        """支持 for sample in batch 迭代"""
        return iter(self._samples)

    def __repr__(self) -> str:
        """面向开发者的清晰字符串展示"""
        sample_count = len(self._samples)
        sample_preview = (
            f"first_id={self._samples[0].sample_id}" if sample_count > 0 else "empty"
        )
        return f"DataBatch(size={sample_count}, {sample_preview})"

    @property
    def labels(self) -> list[int]:
        """提取该批次所有样本的标签列表"""
        return [s.label for s in self._samples]
```

---

## 七、Step 5：核心模块 3 —— 异步并发任务模拟调度器 (`async_runner.py`)

在 `src/ai_toolkit/async_runner.py` 中编写一个模拟同时请求多个 AI 服务或数据源的异步调度器：

```python
"""ai_toolkit.async_runner

基于 Python 3.11+ TaskGroup 的异步并发模拟调度引擎。
"""

import asyncio
import time
from dataclasses import dataclass


@dataclass
class ServiceTaskResult:
    service_name: str
    status_code: int
    data: str
    elapsed_ms: float


async def mock_remote_fetch(
    service_name: str, delay_seconds: float, fail: bool = False
) -> ServiceTaskResult:
    """模拟一个异步网络/模型调用请求"""
    start_t = time.perf_counter()
    await asyncio.sleep(delay_seconds)  # 非阻塞等待

    if fail:
        raise ConnectionError(f"无法连接到服务: {service_name}")

    duration = (time.perf_counter() - start_t) * 1000.0
    return ServiceTaskResult(
        service_name=service_name,
        status_code=200,
        data=f"来自 {service_name} 的推理结果",
        elapsed_ms=duration,
    )


async def run_concurrent_services(
    service_configs: list[tuple[str, float]],
) -> list[ServiceTaskResult]:
    """使用 TaskGroup 结构化并发批量调度多个服务"""
    tasks: list[asyncio.Task[ServiceTaskResult]] = []

    async with asyncio.TaskGroup() as tg:
        for name, delay in service_configs:
            task = tg.create_task(mock_remote_fetch(name, delay))
            tasks.append(task)

    return [t.result() for t in tasks]
```

---

## 八、Step 6：聚合控制台入口脚本 (`main.py`)

在工作区根目录下创建 `main.py`，串联所有核心模块进行综合展示：

```python
"""ai_toolkit 综合实战入口脚本"""

import asyncio
from rich.console import Console
from rich.table import Table

from ai_toolkit.decorators import timer, retry
from ai_toolkit.data_batch import Sample, DataBatch
from ai_toolkit.async_runner import run_concurrent_services

console = Console()


# 1. 验证计时装饰器
@timer(unit="ms", verbose=True)
def heavy_matrix_computation(n: int) -> float:
    """模拟耗时计算"""
    return sum(i * 0.001 for i in range(n))


# 2. 验证重试装饰器
call_count = 0


@retry(max_attempts=3, initial_delay=0.2, backoff_factor=1.5)
def unstable_network_call() -> str:
    global call_count
    call_count += 1
    if call_count < 3:
        raise TimeoutError("远端 API 连接超时！")
    return "✅ 握手成功，Token 获取就绪！"


def test_oop_and_magic_methods() -> None:
    console.rule("[bold cyan]1. 测试面向对象与容器协议 (DataBatch)[/bold cyan]")
    samples = [
        Sample(sample_id=101, payload=[0.1, 0.4, 0.9], label=1),
        Sample(sample_id=102, payload=[0.3, 0.2, 0.5], label=0),
        Sample(sample_id=103, payload=[0.8, 0.9, 0.1], label=1),
    ]

    batch = DataBatch(samples)
    console.print(f"• 批次对象打印: [green]{batch}[/green]")
    console.print(f"• 样本总数 len(batch): [yellow]{len(batch)}[/yellow]")
    console.print(f"• 索引访问 batch[1]: {batch[1]}")
    console.print(f"• 批次所有标签: [magenta]{batch.labels}[/magenta]")


def test_decorators() -> None:
    console.rule("[bold cyan]2. 测试现代函数装饰器 (@timer 与 @retry)[/bold cyan]")
    heavy_matrix_computation(500_000)

    console.print("正在测试模拟故障接口的指数退避重试机制...")
    res = unstable_network_call()
    console.print(f"重试最终结果: [bold green]{res}[/bold green]")


async def test_async_concurrency() -> None:
    console.rule("[bold cyan]3. 测试异步并发调度 (asyncio.TaskGroup)[/bold cyan]")
    services = [
        ("LLM-Reasoning-Core", 0.6),
        ("Dense-Vector-Retriever", 0.4),
        ("Web-Search-Engine", 0.5),
    ]

    console.print("🚀 启动 3 个远程子系统的并发请求...")
    results = await run_concurrent_services(services)

    table = Table(title="并发执行性能评测报告")
    table.add_column("服务名称", style="cyan")
    table.add_column("状态码", style="green")
    table.add_column("执行耗时 (ms)", justify="right", style="yellow")
    table.add_column("返回摘要", style="white")

    for r in results:
        table.add_row(r.service_name, str(r.status_code), f"{r.elapsed_ms:.2f}", r.data)

    console.print(table)


def main() -> None:
    console.print("[bold purple]========================================[/bold purple]")
    console.print("[bold purple]   AI-Toolkit 现代 Python 工具箱验证    [/bold purple]")
    console.print("[bold purple]========================================[/bold purple]")

    test_oop_and_magic_methods()
    test_decorators()
    asyncio.run(test_async_concurrency())


if __name__ == "__main__":
    main()
```

---

## 九、Step 7：运行、性能压测与代码质量门禁 (Ruff)

在终端执行以下指令，感受全套现代化工具链的极速体验：

```powershell
# 1. 使用 uv 直接运行项目（自动同步依赖并在隔离环境中执行）
uv run python main.py

# 2. 一键执行代码规范静态检查（毫秒级扫描）
uvx ruff check .

# 3. 一键执行代码自动对齐格式化
uvx ruff format .
```

---

## 十、第一周项目交付自检清单

在完成本实战后，对照以下清单自检，打上勾即代表你已圆满攻克 Phase 0 第 1 周：

- [ ] **现代工程结构**：拥有标准的 `pyproject.toml`，依赖全部通过 `uv add` 精准管理；
- [ ] **装饰器透彻理解**：亲手写出 `@timer` 并能在控制台打印执行耗时，使用 `@functools.wraps` 保留了函数签名；
- [ ] **魔术方法实战**：能够清晰解释 `__len__`, `__getitem__`, `__repr__` 的作用，并用实例跑通了容器遍历；
- [ ] **异步心智模型**：理解了为什么大模型和 Agent 必须使用 `async/await`，并用 `TaskGroup` 完成了一次并发调度；
- [ ] **代码风格合规**：终端执行 `uvx ruff check .` 显示 `All checks passed!`。
