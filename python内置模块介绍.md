# Python 内置模块介绍

本篇文档总结了 Python 内置模块的核心用法。

## 一、 urllib 模块

`urllib` 是 Python 用于处理 URL 的标准库，在爬虫中主要利用其 `urllib.parse` 子模块来完成 URL 的解析、拼接、编码与解码。

### 1. urllib.parse 常用方法一览

| 方法 | 功能描述 | 代码示例 |
| :--- | :--- | :--- |
| **`urlparse(url)`** | 将 URL 拆解为 6 个部分（协议、域名、路径等）并返回 `ParseResult` 元组 | `urlparse("https://example.com/index?a=1")` |
| **`urljoin(base, url)`** | 智能拼接绝对路径，自动处理斜杠问题 | `urljoin("https://example.com/a", "../b")` -> `https://example.com/b` |
| **`parse_qs(qs)`** | 将查询参数字符串解析为字典（值是列表） | `parse_qs("tag=py&tag=crawl")` -> `{'tag': ['py', 'crawl']}` |
| **`parse_qsl(qs)`** | 将查询参数解析为键值对列表（保留顺序） | `parse_qsl("a=1&b=2")` -> `[('a', '1'), ('b', '2')]` |
| **`urlencode(query_dict)`** | 将字典或键值对列表转换为 URL 查询字符串 | `urlencode({'q': '爬虫'})` -> `'q=%E7%88%AC%E8%99%AB'` |
| **`quote(string)`** | 对单个字符串进行 URL 编码 | `quote("Python 爬虫")` -> `'Python%20%E7%88%AC%E8%99%AB'` |
| **`unquote(string)`** | 对单个 URL 编码字符串进行解码 | `unquote("Python%20%E7%88%AC%E8%99%AB")` -> `'Python 爬虫'` |

#### 1.1 urlparse() 详尽剖析

`urlparse()` 将传入的 URL 划分为 6 个核心组件。

##### 📌 实例分析：
以小红书用户主页 URL 为例：
```python
from urllib.parse import urlparse

user_url = "https://www.xiaohongshu.com/user/profile/5f97b69c0000000001006e86?xsec_token=AB123456&xsec_source=pc_search"
urlParse = urlparse(user_url)
print(urlParse)
```

**运行结果：**
```python
ParseResult(
    scheme='https', 
    netloc='www.xiaohongshu.com', 
    path='/user/profile/5f97b69c0000000001006e86', 
    params='', 
    query='xsec_token=AB123456&xsec_source=pc_search', 
    fragment=''
)
```

##### 📌 字段及访问方式：
可以通过属性名直接访问解析结果中的各部分：
* `urlParse.scheme`: 协议方案，此处为 `'https'`。
* `urlParse.netloc`: 域名和网络位置，此处为 `'www.xiaohongshu.com'`。
* `urlParse.path`: 资源路径，此处为 `'/user/profile/5f97b69c0000000001006e86'`。在项目中，可以使用 `urlParse.path.split('/')[-1]` 提取路径末端的 `user_id`。
* `urlParse.query`: 获取 `?` 后的查询参数字符串，此处为 `'xsec_token=AB123456&xsec_source=pc_search'`。

#### 1.2 URL 编码与解码（quote / unquote）深层解析

##### (1) 为什么需要编码？
* **非 ASCII 字符限制**：URL 规范只允许英文字母、数字和少数符号，直接写入中文在网络传输中会乱码或报错。
* **特殊字符冲突**：URL 中有许多保留字（如 `?`, `&`, `=`, `+`, 空格 等）。当我们要将这些符号作为“普通参数内容”传输时，必须对其编码（如空格转为 `%20`），避免破坏 URL 结构。

##### (2) 核心使用场景
* **场景一：手动拼接 URL**。在代码中用 f-string 手动拼接带中文或特殊字符的 API 时（例如项目中推荐词搜索接口拼接）。
* **场景二：解码抓包数据**。还原从浏览器/抓包工具里复制出来的、包含百分号 `%` 的乱码链接以方便展示或存入数据库。
* **场景三：爬虫签名与加密**。许多平台（如小红书、抖音）在生成防爬签名（Signature）时，算法对参数是否进行了 URL 编码极其敏感，必须手动精确编码保证一致性。

> **💡 贴士**：如果是使用 `requests` 库且通过 `params=dict` 传入参数，`requests` 内部会自动进行 URL 编码，无需手动调用 `quote`。
