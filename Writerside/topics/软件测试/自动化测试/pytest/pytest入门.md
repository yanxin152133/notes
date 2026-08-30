# pytest入门
<show-structure for="chapter,procedure"/>

## 安装pytest

使用pip安装pytest

```bash
pip install -U pytest
```

检查安装的版本

```bash
(.venv) ➜  python-selenium git:(main) pytest --version
pytest 8.4.0
```

## 创建你的第一个测试

创建一个名为`test_sample.py`的新文件，其中包含一个函数和一个测试。

```python
# content of test_sample.py
def func(x):
    return x + 1


def test_answer():
    assert func(3) == 5
```

执行结果：

```
/Users/yxc/GitHub/python-selenium/.venv/bin/python /Applications/PyCharm.app/Contents/plugins/python-ce/helpers/pycharm/_jb_pytest_runner.py --target test_sample.py::test_answer 
Testing started at 16:55 ...
Launching pytest with arguments test_sample.py::test_answer --no-header --no-summary -q in /Users/yxc/GitHub/python-selenium/pytest

============================= test session starts ==============================
collecting ... collected 1 item

test_sample.py::test_answer FAILED                                       [100%]
test_sample.py:5 (test_answer)
4 != 5

Expected :5
Actual   :4
<Click to see difference>

def test_answer():
>       assert func(3) == 5
E       assert 4 == 5
E        +  where 4 = func(3)

test_sample.py:7: AssertionError


============================== 1 failed in 0.02s ===============================

Process finished with exit code 1
```

[100%] 指的是运行所有测试用例的总体进度。结束后，pytest 会显示一个失败报告，因为 func (3) 没有返回 5。

> 你可以使用`assert`语句来验证测试预期。pytest 的`高级断言内省`将智能地报告断言表达式的中间值，这样你就可以避免使用
> `JUnit 遗留方法`中众多的名称。

## 运行多个测试

pytest 将运行当前目录及其子目录中所有格式为 test_*.py 或 *_test.py 的文件。更通用地说，它遵循 标准测试发现规则。

## 断言引发特定的异常

使用 raises 助手来断言某些代码会引发异常。

```Python
# content of test_sysexit.py
# 使用raises助手来断言某些代码会引发异常

import pytest


def f():
    raise SystemExit(1)


def test_mytest():
    with pytest.raises(SystemExit):
        f()

```

执行结果：

```
/Users/yxc/GitHub/python-selenium/.venv/bin/python /Applications/PyCharm.app/Contents/plugins/python-ce/helpers/pycharm/_jb_pytest_runner.py --target test_sysexit.py::test_mytest 
Testing started at 17:13 ...
Launching pytest with arguments test_sysexit.py::test_mytest --no-header --no-summary -q in /Users/yxc/GitHub/python-selenium/pytest

============================= test session starts ==============================
collecting ... collected 1 item

test_sysexit.py::test_mytest PASSED                                      [100%]

============================== 1 passed in 0.00s ===============================

Process finished with exit code 0

```

以`静默`报告模式执行测试函数。

```bash
(.venv) ➜  python-selenium git:(main) ✗ pytest -q pytest/test_sysexit.py 
.                                                                                                                                 [100%]
1 passed in 0.00s
```

> -q/--quiet 标志使本示例及后续示例的输出保持简洁。

## 在类中对多个测试进行分组

一旦你开发了多个测试，你可能希望将它们分组到一个类中。pytest 可以轻松创建包含多个测试的类。

```python
# content of test_class.py
# 在类中对多个测试进行分组

class TestClass:
    def test_one(self):
        x = "this"
        assert "h" in x

    def test_two(self):
        x = "hello"
        assert hasattr(x, "check")

```

执行结果：

```bash
/Users/yxc/GitHub/python-selenium/.venv/bin/python /Applications/PyCharm.app/Contents/plugins/python-ce/helpers/pycharm/_jb_pytest_runner.py --target test_class.py::TestClass 
Testing started at 17:18 ...
Launching pytest with arguments test_class.py::TestClass --no-header --no-summary -q in /Users/yxc/GitHub/python-selenium/pytest

============================= test session starts ==============================
collecting ... collected 2 items

test_class.py::TestClass::test_one 
test_class.py::TestClass::test_two 

========================= 1 failed, 1 passed in 0.02s ==========================
PASSED                                [ 50%]FAILED                                [100%]
test_class.py:8 (TestClass.test_two)
self = <test_class.TestClass object at 0x10507c7d0>

    def test_two(self):
        x = "hello"
>       assert hasattr(x, "check")
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_class.py:11: AssertionError

Process finished with exit code 1

```

以`静默`报告模式执行测试函数。

```Bash
(.venv) ➜  python-selenium git:(main) ✗ pytest -q pytest/test_class.py  
.F                                                                                                                                [100%]
=============================================================== FAILURES ================================================================
__________________________________________________________ TestClass.test_two ___________________________________________________________

self = <test_class.TestClass object at 0x1068f7610>

    def test_two(self):
        x = "hello"
>       assert hasattr(x, "check")
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

pytest/test_class.py:11: AssertionError
======================================================== short test summary info ========================================================
FAILED pytest/test_class.py::TestClass::test_two - AssertionError: assert False
1 failed, 1 passed in 0.04s
```

> 将测试分组到类中具有以下好处
> - 测试组织
> - 仅在该特定类中共享测试的固件（fixtures）
> - 在类级别应用标记，并使他们隐式应用于所有测试

### 类中对多个测试进行分组的注意事项

> 在类中对测试进行分组时需要注意的一点是，每个测试都有该类的一个唯一实例。让每个测试共享同一个类实例将非常不利于测试隔离，并会助长不良的测试实践。

```python
# content of test_class_demo.py
# 在类中对测试进行分组时的注意事项：每个测试都有该类的一个唯一实例。让每个测试共享同一个类实例将非常不利于测试隔离，并会助长不良的测试实践。

class TestClassDemoInstance:
    value = 0

    def test_one(self):
        self.value = 1
        assert self.value == 1

    def test_two(self):
        assert self.value == 1

```

执行结果：

```Bash
/Users/yxc/GitHub/python-selenium/.venv/bin/python /Applications/PyCharm.app/Contents/plugins/python-ce/helpers/pycharm/_jb_pytest_runner.py --target test_class_demo.py::TestClassDemoInstance 
Testing started at 17:30 ...
Launching pytest with arguments test_class_demo.py::TestClassDemoInstance --no-header --no-summary -q in /Users/yxc/GitHub/python-selenium/pytest

============================= test session starts ==============================
collecting ... collected 2 items

test_class_demo.py::TestClassDemoInstance::test_one 
test_class_demo.py::TestClassDemoInstance::test_two 

========================= 1 failed, 1 passed in 0.02s ==========================
PASSED               [ 50%]FAILED               [100%]
test_class_demo.py:10 (TestClassDemoInstance.test_two)
0 != 1

Expected :1
Actual   :0
<Click to see difference>

self = <test_class_demo.TestClassDemoInstance object at 0x1022a87d0>

    def test_two(self):
>       assert self.value == 1
E       assert 0 == 1
E        +  where 0 = <test_class_demo.TestClassDemoInstance object at 0x1022a87d0>.value

test_class_demo.py:12: AssertionError

Process finished with exit code 1

```

## 使用pytest.approx比较浮点值

使用 pytest.approx () 来比较可能存在微小舍入误差的浮点值。

```Python
# content of test_approx.py
# 使用 pytest.approx() 来比较可能存在微小舍入误差的浮点值
import pytest


def test_sum():
    assert (0.1 + 0.2) == pytest.approx(0.3)

```

执行结果：

```bash
/Users/yxc/GitHub/python-selenium/.venv/bin/python /Applications/PyCharm.app/Contents/plugins/python-ce/helpers/pycharm/_jb_pytest_runner.py --target test_approx.py::test_sum 
Testing started at 20:59 ...
Launching pytest with arguments test_approx.py::test_sum --no-header --no-summary -q in /Users/yxc/GitHub/python-selenium/pytest

============================= test session starts ==============================
collecting ... collected 1 item

test_approx.py::test_sum PASSED                                          [100%]

============================== 1 passed in 0.01s ===============================

Process finished with exit code 0

```

> 这避免了手动进行容差检查或使用`math.isclose`的需要，并且适用于标量、列表和 NumPy 数组。

## 为功能测试请求一个唯一的临时目录

pytest 提供了 内置固件/函数参数 来请求任意资源，例如唯一的临时目录。

```Python
# content of test_tmp_path.py
# 唯一的临时目录
def test_needsfiles(tmp_path):
    print(tmp_path)
    assert 0

```

执行结果：

```Bash
/Users/yxc/GitHub/python-selenium/.venv/bin/python /Applications/PyCharm.app/Contents/plugins/python-ce/helpers/pycharm/_jb_pytest_runner.py --target test_tmp_path.py::test_needsfiles 
Testing started at 21:02 ...
Launching pytest with arguments test_tmp_path.py::test_needsfiles --no-header --no-summary -q in /Users/yxc/GitHub/python-selenium/pytest

============================= test session starts ==============================
collecting ... collected 1 item

test_tmp_path.py::test_needsfiles FAILED                                 [100%]/private/var/folders/m1/70psm0q1105gmztg4spwxrvr0000gn/T/pytest-of-yxc/pytest-0/test_needsfiles0

test_tmp_path.py:1 (test_needsfiles)
tmp_path = PosixPath('/private/var/folders/m1/70psm0q1105gmztg4spwxrvr0000gn/T/pytest-of-yxc/pytest-0/test_needsfiles0')

    def test_needsfiles(tmp_path):
        print(tmp_path)
>       assert 0
E       assert 0

test_tmp_path.py:4: AssertionError


============================== 1 failed in 0.02s ===============================

Process finished with exit code 1

```