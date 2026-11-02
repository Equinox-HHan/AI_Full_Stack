# Python Decorators & Closure Deep Dive | Python 装饰器原理、闭包机制与生产级实战手册

> 彻底击穿 Python 装饰器的底层逻辑：从头等公民、LEGB 作用域到闭包内存机制，推导无参、带参、类装饰器的完整形态，掌握生产级高精度计时器与指数退避重试器。

---

## 目录

1. [前置认知：函数是一等公民 (First-Class Citizens)](#一前置认知函数是一等公民-first-class-citizens)
2. [变量作用域与 LEGB 规则](#二变量作用域与-legb-规则)
3. [闭包 (Closure) 的底层机制与内存原理](#三闭包-closure-的底层机制与内存原理)
4. [装饰器 (Decorator) 的推导与语法糖本质](#四装饰器-decorator-的推导与语法糖本质)
   - [4.1 语法糖还原公式](#41-语法糖还原公式)
   - [4.2 基础无参装饰器标准模板](#42-基础无参装饰器标准模板)
   - [4.3 为什么必须使用 @functools.wraps？](#43-为什么必须使用-functoolswraps)
   - [4.4 带参数的装饰器（三层闭包拆解）](#44-带参数的装饰器三层闭包拆解)
   - [4.5 基于类实现的装饰器 (__call__)](#45-基于类实现的装饰器-__call__)
   - [4.6 装饰器修饰类](#46-装饰器修饰类)
5. [AI 与工程实战：5 大生产级必备装饰器源码库](#五ai-与工程实战5-大生产级必备装饰器源码库)
   - [5.1 高精度耗时统计装饰器 (@timer)](#51-高精度耗时统计装饰器-timer)
   - [5.2 API 指数退避重试装饰器 (@retry)](#52-api-指数退避重试装饰器-retry)
   - [5.3 内存缓存与 LRU 机制 (@memoize)](#53-内存缓存与-lru-机制-memoize)
   - [5.4 严格运行时类型校验装饰器 (@type_guard)](#54-严格运行时类型校验装饰器-type_guard)
   - [5.5 单例模式装饰器 (@singleton)](#55-单例模式装饰器-singleton)
6. [多个装饰器的执行顺序与洋葱模型](#六多个装饰器的执行顺序与洋葱模型)
7. [常见避坑指南](#七常见避坑指南)

---

## 一、前置认知：函数是一等公民 (First-Class Citizens)

在 Python 中，**函数是真正的一等对象**。这意味着函数与整数、字符串、列表没有任何地位区别：

```
                    ┌────────────────────────────────────────┐
                    │      函数作为“一等公民”的四大核心特权  │
                    └───────────────────┬────────────────────┘
                                        │
      ┌──────────────────┬──────────────┴─────┬──────────────────┐
      ▼                  ▼                    ▼                  ▼
【可以被赋值给变量】  【可以存入数据结构】   【可以作为参数传递】  【可以作为返回值返回】
 fn = my_func        fns = [f1, f2]       def run(callback)     def get_handler()
 fn()                fns[0]()             run(my_func)          return sub_func
```

```python
def square(x: int) -> int:
    return x * x


# 1. 赋值给其他变量
f = square
print(f(5))  # 25


# 2. 作为参数传入另一个函数（高阶函数）
def apply_twice(func, val):
    return func(func(val))


print(apply_twice(square, 3))  # square(square(3)) = 81


# 3. 在函数内部定义嵌套函数并将其作为返回值返回
def get_multiplier(factor: int):
    def multiply(x: int) -> int:
        return x * factor

    return multiply


double = get_multiplier(2)
print(double(10))  # 20
```

---

## 二、变量作用域与 LEGB 规则

理解装饰器之前，必须精准理解 Python 解释器在查找一个变量名时的顺序——**LEGB 规则**：

```
┌────────────────────────────────────────────────────────┐
│  L (Local): 当前函数内部局部作用域                     │
│    └─► E (Enclosing): 外部嵌套函数的局部作用域 (闭包关键!)│
│          └─► G (Global): 当前模块全局作用域            │
│                └─► B (Built-in): Python 内置预定义符号 │
└────────────────────────────────────────────────────────┘
```

### 关键关键字：`global` vs `nonlocal`

- **`global`**：在局部作用域中声明修改模块顶层的全局变量。
- **`nonlocal`**：在嵌套函数内部声明修改**上一层外层函数（Enclosing）**中的局部变量（编写有状态闭包时的核心武器）：

```python
def outer():
    count = 0  # 位于 Enclosing 作用域

    def inner():
        nonlocal count  # 声明修改 outer 中的 count，而非在本地新建一个同名局部变量
        count += 1
        return count

    return inner


counter = outer()
print(counter())  # 1
print(counter())  # 2
```

---

## 三、闭包 (Closure) 的底层机制与内存原理

### 3.1 什么是闭包？

**闭包**是指：**一个嵌套函数不仅记住了自己的代码逻辑，还“绑定/捕获”了其定义时外层作用域中存在的自由变量（Free Variables），即使外层函数已经执行结束、调用栈帧已经弹出销毁，这些被捕获的变量依然常驻在内存中供该函数访问。**

```python
def make_exponential_evaluator(power: int):
    # power 是外层函数的局部变量
    def evaluate(base: int) -> int:
        # evaluate 函数内部引用了外层的 power（自由变量）
        return base**power

    return evaluate


cube = make_exponential_evaluator(3)
# 此时 make_exponential_evaluator 函数已经执行结束！但 cube 依然记得 power=3
print(cube(2))  # 8
print(cube(5))  # 125
```

### 3.2 深入底层：`__closure__` 属性探测

Python 是如何在底层保存这些自由变量的？我们可以通过 `__closure__` 魔法属性直接查看：

```python
print(cube.__closure__)
# 输出: (<cell at 0x...: int object at 0x...>,)

# 打印出被捕获绑定的具体数值
print(cube.__closure__[0].cell_contents)  # 输出: 3
```
每个闭包函数内部维护着一个包含若干 **Cell 对象** 的元组，每个 Cell 指向被引用的外部变量所在的堆内存，因此即使外层函数栈帧消亡，变量也不会被垃圾回收器释放。

---

## 四、装饰器 (Decorator) 的推导与语法糖本质

### 4.1 语法糖还原公式

所谓装饰器，**本质就是一个接收函数作为输入、并返回一个被包装后的新函数的高阶函数**。

当你写下：
```python
@my_decorator
def my_func():
    pass
```
在 Python 底层，它**百分之百等价于**以下这一行赋值语句：
```python
my_func = my_decorator(my_func)
```
记住这一行还原公式，所有复杂的装饰器语法都能迎刃而解！

---

### 4.2 基础无参装饰器标准模板

```python
from typing import Callable, Any


def simple_logger(func: Callable[..., Any]) -> Callable[..., Any]:
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        print(f"👉 [调用开始] 函数 {func.__name__} 正在被调用...")
        # 执行被装饰的原始函数
        result = func(*args, **kwargs)
        print(f"👈 [调用结束] 函数 {func.__name__} 执行完成！")
        return result

    return wrapper


@simple_logger
def add(a: int, b: int) -> int:
    return a + b


print(add(3, 5))
```

---

### 4.3 为什么必须使用 `@functools.wraps`？

如果你在上面的代码后打印 `add.__name__` 和文档：

```python
print(add.__name__)  # 输出: 'wrapper'！而不是 'add'！
print(add.__doc__)  # 原函数的 docstring 彻底丢失！
```

**为什么会这样？**  
因为 `add = simple_logger(add)` 后，`add` 变量实际上指向了内部的 `wrapper` 函数！原函数的元数据（函数名、文档、参数签名、类型注解）全部被冲刷掉了，这会导致自动化文档工具（如 Sphinx）和依赖签名的框架（如 FastAPI、Pydantic）发生灾难性故障！

**规范解法：使用 `@functools.wraps`：**

```python
import functools
from typing import Callable, Any


def robust_logger(func: Callable[..., Any]) -> Callable[..., Any]:
    @functools.wraps(func)  # 👈 核心：自动将 func 的元数据完整复制给 wrapper！
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        return func(*args, **kwargs)

    return wrapper


@robust_logger
def calculate_accuracy(tp: int, total: int) -> float:
    """计算分类准确率指标"""
    return tp / total


print(calculate_accuracy.__name__)  # 恢复为 'calculate_accuracy'
print(calculate_accuracy.__doc__)  # 恢复为 '计算分类准确率指标'
```

---

### 4.4 带参数的装饰器（三层闭包拆解）

如果我们需要让装饰器本身接收配置参数（例如 `@retry(max_attempts=3)`），就必须在外部再套一层函数：

```
                    ┌────────────────────────────────────────┐
                    │      带参数装饰器的三层函数嵌套结构    │
                    └───────────────────┬────────────────────┘
                                        │
┌───────────────────────────────────────┴───────────────────────────────────────┐
│ 第 1 层: 最外层函数 (接收装饰器参数, 如 max_attempts, prefix)                   │
│   def retry(max_attempts: int):                                               │
│                                                                               │
│   ┌────────────────────────────────────────────────────────────────────────┐  │
│   │ 第 2 层: 真正的装饰器函数 (接收被装饰的目标函数 func)                     │  │
│   │   def decorator(func):                                                 │  │
│   │                                                                        │  │
│   │   ┌──────────────────────────────────────────────────────────────────┐ │  │
│   │   │ 第 3 层: 包装替换函数 (接收 func 运行时接收的动态参数 *args, **kwargs)│ │ │  │
│   │   │   @functools.wraps(func)                                         │ │ │  │
│   │   │   def wrapper(*args, **kwargs):                                  │ │ │  │
│   │   │       # 业务拦截逻辑                                             │ │ │  │
│   │   │       return func(*args, **kwargs)                               │ │ │  │
│   │   └──────────────────────────────────────────────────────────────────┘ │  │
│   │   return wrapper                                                       │  │
│   └────────────────────────────────────────────────────────────────────────┘  │
│   return decorator                                                            │
└───────────────────────────────────────────────────────────────────────────────┘
```

**还原公式推导：**
```python
@retry(max_attempts=3)
def request_api():
    pass


# 还原为底层执行：
# 1. 先执行 retry(max_attempts=3)，返回第 2 层的 decorator 函数
# 2. 再执行 decorator(request_api)，返回第 3 层的 wrapper 函数
# 3. request_api = wrapper
```

---

### 4.5 基于类实现的装饰器 (`__call__`)

当装饰器内部需要维护复杂的内部状态、计数器或多方法时，使用**类**实现装饰器往往比嵌套函数更加清晰：

```python
import functools
from typing import Callable, Any


class CallCounter:
    """统计函数被调用的总次数"""

    def __init__(self, func: Callable[..., Any]) -> None:
        self.func = func
        self.count: int = 0
        functools.update_wrapper(self, func)  # 类装饰器保留元数据

    def __call__(self, *args: Any, **kwargs: Any) -> Any:
        self.count += 1
        print(f"📊 函数 {self.func.__name__} 累计已被调用 {self.count} 次")
        return self.func(*args, **kwargs)


@CallCounter
def process_token(token: str) -> str:
    return token.strip().lower()


process_token(" Apple ")
process_token(" BANANA ")
print(f"最终计数: {process_token.count}")  # 2
```

---

## 五、AI 与工程实战：5 大生产级必备装饰器源码库

### 5.1 高精度耗时统计装饰器 (`@timer`)

不仅精准计算执行时间，还支持切换单位（ms / s），并能捕获函数入参摘要，是后续算法跑实验时评测性能的核心工具。

```python
import time
import functools
from typing import Callable, TypeVar, ParamSpec

P = ParamSpec("P")
R = TypeVar("R")


def timer(
    unit: str = "ms", print_args: bool = False
) -> Callable[[Callable[P, R]], Callable[P, R]]:
    """生产级高精度函数耗时统计装饰器

    :param unit: 耗时输出单位，支持 'ms' (毫秒) 或 's' (秒)
    :param print_args: 是否在日志中打印函数输入参数
    """

    def decorator(func: Callable[P, R]) -> Callable[P, R]:
        @functools.wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            arg_str = ""
            if print_args:
                arg_repr = [repr(a) for a in args]
                kwarg_repr = [f"{k}={v!r}" for k, v in kwargs.items()]
                arg_str = f"({', '.join(arg_repr + kwarg_repr)})"

            start_t = time.perf_counter()
            result = func(*args, **kwargs)
            end_t = time.perf_counter()

            duration = end_t - start_t
            if unit.lower() == "ms":
                time_display = f"{duration * 1000:.3f} ms"
            else:
                time_display = f"{duration:.4f} s"

            print(f"⏱️  [Benchmark] {func.__name__}{arg_str} -> 耗时: {time_display}")
            return result

        return wrapper

    return decorator


# 实战调用
@timer(unit="ms", print_args=True)
def matrix_dot_product(dim: int) -> float:
    return sum(i * 0.5 for i in range(dim))


matrix_dot_product(500_000)
```

---

### 5.2 API 指数退避重试装饰器 (`@retry`)

在调用不稳定的大模型 API（如 OpenAI、Ollama、DeepSeek）或抓取网页时，网络抖动或限流是常态。指数退避（Exponential Backoff）是行业标准的容灾算法：

```python
import time
import functools
from typing import Callable, Any, Type


def retry(
    max_attempts: int = 3,
    initial_delay: float = 1.0,
    backoff_factor: float = 2.0,
    exceptions: tuple[Type[BaseException], ...] = (Exception,),
) -> Callable[..., Any]:
    """工业级指数退避重试装饰器

    :param max_attempts: 最大尝试次数
    :param initial_delay: 首次重试等待秒数
    :param backoff_factor: 每次等待时间的倍乘系数 (1s -> 2s -> 4s)
    :param exceptions: 触发重试的目标异常类型元组
    """

    def decorator(func: Callable[..., Any]) -> Callable[..., Any]:
        @functools.wraps(func)
        def wrapper(*args: Any, **kwargs: Any) -> Any:
            delay = initial_delay
            last_exception = None

            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    last_exception = e
                    if attempt == max_attempts:
                        print(
                            f"❌ [Retry-Out] 函数 {func.__name__} 达最大重试次数 ({max_attempts})，彻底失败！"
                        )
                        raise
                    print(
                        f"⚠️ [Retry {attempt}/{max_attempts}] 函数 {func.__name__} 抛出异常: {e!r}，"
                        f"{delay:.1f} 秒后重试..."
                    )
                    time.sleep(delay)
                    delay *= backoff_factor

            raise last_exception  # 理论不可达，用于类型兜底

        return wrapper

    return decorator
```

---

### 5.3 内存缓存与 LRU 机制 (`@memoize`)

对于纯计算函数（相同输入必定产生相同输出），缓存计算结果能够避免大量重复计算。标准库自带的 `@functools.lru_cache` 是最优秀的实现：

```python
import functools


@functools.lru_cache(maxsize=128)  # 最近最少使用 (LRU) 缓存，容量 128
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)


# 瞬间计算第 100 项，无任何卡顿！
print(fibonacci(100))
# 查看缓存命中状态统计
print(fibonacci.cache_info())
# CacheInfo(hits=98, misses=101, maxsize=128, currsize=101)
```

---

### 5.4 严格运行时类型校验装饰器 (`@type_guard`)

通过读取函数的 `__annotations__` 字典，在执行前对传入实参进行实时动态类型校验：

```python
import functools
import inspect
from typing import Callable, Any


def type_guard(func: Callable[..., Any]) -> Callable[..., Any]:
    """运行时根据 Type Hints 自动校验参数类型"""
    sig = inspect.signature(func)

    @functools.wraps(func)
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        bound_args = sig.bind(*args, **kwargs)
        bound_args.apply_defaults()

        for name, value in bound_args.arguments.items():
            expected_type = func.__annotations__.get(name)
            if expected_type and isinstance(expected_type, type):
                if not isinstance(value, expected_type):
                    raise TypeError(
                        f"参数 [{name}] 类型错误！预期: {expected_type.__name__}, 实际传入: {type(value).__name__}"
                    )

        return func(*args, **kwargs)

    return wrapper


@type_guard
def set_learning_rate(lr: float, epoch: int) -> None:
    print(f"成功设置 lr={lr}, epoch={epoch}")


set_learning_rate(0.01, 10)  # 成功
# set_learning_rate("0.01", 10) # 抛出 TypeError: 参数 [lr] 类型错误！预期: float, 实际传入: str
```

---

### 5.5 单例模式装饰器 (`@singleton`)

确保整个系统运行期间，目标类只有唯一一个实例（如模型权重加载器、数据库连接池、系统配置对象）：

```python
import functools
from typing import Any, TypeVar, Type

T = TypeVar("T")


def singleton(cls: Type[T]) -> Callable[..., T]:
    """单例类装饰器"""
    instances: dict[Type[T], T] = {}

    @functools.wraps(cls)
    def get_instance(*args: Any, **kwargs: Any) -> T:
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]

    return get_instance


@singleton
class ModelWeightCache:
    def __init__(self) -> None:
        print("🚀 [初始化] 正在将 7B 大模型权重加载到显存 (仅此一次)...")
        self.weights = {"layer_0": "tensor_data"}


w1 = ModelWeightCache()
w2 = ModelWeightCache()
print("是否为同一对象:", w1 is w2)  # True！不会重复加载！
```

---

## 六、多个装饰器的执行顺序与洋葱模型

当一个函数被叠加多个装饰器时，其加载与执行遵循 **"洋葱模型"**：

```python
@decorator_a
@decorator_b
def target_func():
    pass
```

- **装饰加载阶段（自下而上）：** 先执行 `b = decorator_b(target_func)`，再执行 `a = decorator_a(b)`；
- **调用执行阶段（自外而内）：** 先进入 `decorator_a` 的前半部分代码，再进入 `decorator_b` 的前半部分代码，执行 `target_func`，再依次退出 `b` 与 `a`。

---

## 七、常见避坑指南

1. ❌ **忘记写 `@functools.wraps`**：导致原函数名字变成 `wrapper`，排查调用栈时极度痛苦。
2. ❌ **三层嵌套写错括号**：带参装饰器调用时如果忘记加括号 `@my_decorator()`，会导致把参数误当作 `func` 传入。
3. ❌ **在装饰器里改变原函数的返回值**：包装器函数末尾务必显式 `return result`，否则原函数调用方将拿到意外的 `None`。
