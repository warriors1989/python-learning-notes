# Python 字典与常用操作总结

本文用于记录 Python 字典（Dictionary）、字典推导式（Dict Comprehension）以及相关的常用内置操作。后续有新的字典相关知识点将持续追加至此。

---

## 1. 字典基础

字典是 Python 中的键值对映射类型（Map）。
* 键（Key）必须是不可变类型（如字符串、数字、元组），且唯一。
* 值（Value）可以是任意类型。

```python
user = {"name": "张三", "age": 18}
print(user["name"])  # 张三
```

---

## 2. 字典推导式 (Dictionary Comprehension)

这是 Python 中特有的一种简洁语法，用于快速从一个可迭代对象构造新字典。

### 语法模板
```text
{ key_expression : value_expression for item in iterable }
  └───── 键 ─────┘ └───── 值 ─────┘ └──── 循环与数据源 ────┘
```

### 经典实例分析（Cookie 字符串转字典）
在爬虫工具类中，常见的转换逻辑如下：
```python
cookies_str = "webId=123; sex=1"

# 字典推导式写法
ck = {i.split('=')[0]: '='.join(i.split('=')[1:]) for i in cookies_str.split('; ')}
```

**等价的标准多行 `for` 循环写法**：
```python
ck = {}
for i in cookies_str.split('; '):
    # 提取键 (Key)
    key = i.split('=')[0]
    # 提取值 (Value)，用 '=' 拼接防止原值中也带 '=' 导致丢失
    value = '='.join(i.split('=')[1:])
    # 写入字典
    ck[key] = value
```

---

## 3. 推导式对比：列表 vs 字典

| 类型 | 外层括号 | 语法表达 | 结果示例 |
| :--- | :--- | :--- | :--- |
| **列表推导式** | `[]` 中括号 | `[x**2 for x in [1, 2]]` | `[1, 4]` |
| **字典推导式** | `{}` 大括号 | `{x: x**2 for x in [1, 2]}` | `{1: 1, 2: 4}` (包含 `:` 键值对) |

### 进阶用法（带 if 过滤）
推导式中还可以加入 `if` 条件过滤：
```python
# 仅保留偶数作为键的字典
squares = {x: x**2 for x in range(5) if x % 2 == 0}
# 结果: {0: 0, 2: 4, 4: 16}
```
