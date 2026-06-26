# Python 第三方库记录

## PyExecJS

-   **简介**: 一个让 Python 能够执行 JavaScript 代码的库。它通过调用你系统上已有的 JavaScript 运行时（如 Node.js）来实现。在爬虫项目中，它通常被用来运行目标网站中用于生成加密参数（如 `sign`, `token`）的 JS 代码。
-   **安装与导入**:
    -   安装名: `PyExecJS`
    -   导入名: `execjs`
    -   安装命令: `pip install PyExecJS`
-   **示例代码**:
    ```python
    import execjs

    # 一个简单的 JavaScript 函数，用于两数相加
    js_code = """
    function add(a, b) {
        return a + b;
    }
    """

    # 使用 execjs 编译 JavaScript 代码
    ctx = execjs.compile(js_code)

    # 调用 JavaScript 中的 add 函数
    result = ctx.call("add", 5, 10)

    # 打印结果
    print(f"在 Python 中调用 JavaScript 的 add(5, 10) 函数，结果是: {result}")
    # 期望输出: 在 Python 中调用 JavaScript 的 add(5, 10) 函数，结果是: 15
    ```
-   **仓库地址**: [https://github.com/doloopwhile/PyExecJS](https://github.com/doloopwhile/PyExecJS)

---
