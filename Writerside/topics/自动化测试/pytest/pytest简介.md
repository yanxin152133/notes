# pytest简介
<show-structure for="chapter,procedure"/>

## 简介

> pytest框架使编写小型、可读的测试变得简单，并且可以扩展以支持应用程序和库的复杂功能测试。
> PyPI包名：pytest

## 简单示例

```python
# content of test_sample.py
def inc(x):
    return x + 1


def test_answer():
    assert inc(3) == 5
```

执行结果：

```
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-9.x.y, pluggy-1.x.y
rootdir: /home/sweet/project
collected 1 item

test_sample.py F                                                     [100%]

================================= FAILURES =================================
_______________________________ test_answer ________________________________

    def test_answer():
>       assert inc(3) == 5
E       assert 4 == 5
E        +  where 4 = inc(3)

test_sample.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_sample.py::test_answer - assert 4 == 5
============================ 1 failed in 0.12s =============================
```

## 特性

- 提供失败`断言语句`的详细信息（无需记忆`self.assert*`的名称）
- `自动发现`测试模块和函数
- `模块化Fixtures`，用于管理小型或参数化的长生命周期测试资源
- 可以直接运行`unittest`（包括trial）测试套件
- python3.10+或PyPy3
- 丰富的插件架构，拥有超过1300+个外部插件和蓬勃发展的社区