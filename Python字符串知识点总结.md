# Python 字符串知识点总结

字符串是 Python 中最常用的数据类型之一。本文档用于总结 Python 字符串的核心概念、常见操作以及特殊字面量表示法（如原始字符串 `r`），并随着学习的深入持续补充。

---

## 一、 转义字符与普通字符串

在 Python 的普通字符串中，反斜杠 `\` 是一个特殊的**转义字符**，用于引入不能直接输入的特殊字符（如换行符）或对具有特殊含义的字符进行转义。

### 1. 常见转义字符表

| 转义字符 | 含义 | 示例 |
| :--- | :--- | :--- |
| `\\` | 反斜杠本身 (`\`) | `'C:\\Users'` |
| `\'` | 单引号 (`'`) | `'It\'s a book.'` |
| `\"` | 双引号 (`"`) | `"He said \"Hello\""` |
| `\n` | 换行符 (Line Feed) | `'Line1\nLine2'` |
| `\t` | 水平制表符 (Tab) | `'Col1\tCol2'` |
| `\xXX` | 十六进制数对应的字符 | `'\x41'` (即字母 `'A'`) |
| `\uXXXX` | Unicode 字符 (16位) | `'\u4e2d'` (即汉字 `'中'`) |

---

## 二、 原始字符串 (Raw String)

在字符串字面量前加上前缀 `r` 或 `R`，该字符串就变成了**原始字符串（Raw String）**。

### 1. 核心作用
在原始字符串中，**所有的反斜杠 `\` 都会被当作普通字符处理，转义机制完全失效**。

```python
# 普通字符串：\n 会被解析为换行符
print("Hello\nWorld")
# 输出：
# Hello
# World

# 原始字符串：\n 被当作反斜杠和字母 n 两个独立字符
print(r"Hello\nWorld")
# 输出：
# Hello\nWorld
```

### 2. 经典应用场景

#### ① 正则表达式 (Regular Expressions)
正则表达式中大量使用反斜杠来表示特殊匹配模式（如 `\d` 表示数字，`\w` 表示字母数字，`\s` 表示空白字符）。
若不使用原始字符串，则需要写成双反斜杠 `\\d`，非常冗长且易错。
* **推荐做法**：`pattern = r"\d+\.\d+"`

#### ② Windows 文件路径
Windows 系统下的路径分隔符使用的是反斜杠 `\`。
如果使用普通字符串，`C:\Users\new_folder` 中的 `\U` 和 `\n` 会被误识别为 Unicode 转义和换行，导致程序报错或逻辑错误。
* **推荐做法**：`path = r"C:\Users\new_folder"`

#### ③ 复杂 URL / 加密 Token（如爬虫参数）
小红书等平台的加密参数或 URL 有时可能随机生成包含 `\x`、`\u` 开头的字符串段落。
使用 `r` 前缀可以避免 Python 在解析字符串时因为把它们当作无效的十六进制/Unicode 转义字符而抛出 `SyntaxError`。
* **小红书爬虫示例**：
  ```python
  # 避免参数中的特殊字符导致 Python 解释器抛出 unicodeescape 错误
  notes = [
      r'https://www.xiaohongshu.com/explore/683fe17f0000000023017c6a?xsec_token=ABBr_cMzallQeLyKSRdPk9fwzA0torkbT_ubuQP1ayvKA=&xsec_source=pc_user',
  ]
  ```

---

## 三、 原始字符串的局限与避坑指南

> [!WARNING]
> **原始字符串不能以单数个反斜杠 `\` 结尾**。
>
> 即使在原始字符串中，反斜杠依然会对紧随其后的引号起一定的“转义/屏蔽”作用（虽然反斜杠本身最终也会被保留在字符串中）。如果最后一个字符是反斜杠，它会转义末尾的闭合引号，导致 Python 认为字符串没有结束而报语法错误。

### 错误示例
```python
# 语法错误 (SyntaxError: unterminated string literal)
path = r"C:\Users\" 
```

### 解决方案
如果路径最后必须保留反斜杠，可以采用以下几种方法：
1. **使用普通字符串并对反斜杠转义**：
   ```python
   path = "C:\\Users\\"
   ```
2. **字符串拼接**：
   ```python
   path = r"C:\Users" + "\\"
   ```
3. **（推荐）使用 `os.path` 或 `pathlib` 库进行路径拼接**。
