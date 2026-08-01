# Python：文件读写



## 说明

读写文本文件和 CSV/JSON 数据文件。学习 `with open()` 上下文管理器、异常处理 `try/except`。



## 代码

```python

import json

import csv

import os



# ====== 文本文件写入 ======

with open("日记.txt", "w", encoding="utf-8") as f:

    f.write("=== 我的日记 ===\n")

    f.write("今天学习了 Python 文件操作。\n")

    # writelines 接受字符串列表

    f.writelines(["第一行\n", "第二行\n", "第三行\n"])



# ====== 文本文件读取 ======

try:

    with open("日记.txt", "r", encoding="utf-8") as f:

        content = f.read()          # 读取全部

        print("--- 读取全部 ---")

        print(content)

except FileNotFoundError:

    print("文件不存在！")



# 逐行读取

print("\n--- 逐行读取 ---")

try:

    with open("日记.txt", "r", encoding="utf-8") as f:

        for line_num, line in enumerate(f, 1):

            print(f"第{line_num}行: {line.strip()}")

except FileNotFoundError:

    print("文件不存在！")



# ====== 追加模式 ======

with open("日记.txt", "a", encoding="utf-8") as f:

    f.write("又加了一行新内容。\n")



# ====== JSON 读写 ======

data = {

    "name": "小明",

    "age": 12,

    "hobbies": ["编程", "篮球"],

    "scores": {"语文": 90, "数学": 95}

}



# 写入 JSON

with open("student.json", "w", encoding="utf-8") as f:

    json.dump(data, f, ensure_ascii=False, indent=2)



# 读取 JSON

with open("student.json", "r", encoding="utf-8") as f:

    loaded = json.load(f)

    print(f"\nJSON 读取: {loaded['name']} 的爱好: {loaded['hobbies']}")



# ====== CSV 读写 ======

# 写入 CSV

with open("students.csv", "w", encoding="utf-8", newline="") as f:

    writer = csv.writer(f)

    writer.writerow(["姓名", "年龄", "分数"])

    writer.writerow(["小明", 12, 90])

    writer.writerow(["小红", 13, 85])



# 读取 CSV

with open("students.csv", "r", encoding="utf-8") as f:

    reader = csv.reader(f)

    for row in reader:

        print(row)



# ====== os 模块文件操作 ======

print(f"\n文件是否存在: {os.path.exists('日记.txt')}")

print(f"文件大小: {os.path.getsize('日记.txt')} 字节")

print(f"当前目录: {os.getcwd()}")



# 清理临时文件

os.remove("日记.txt")

os.remove("student.json")

os.remove("students.csv")

```



## 教学重点

- 文件打开模式：`r`读 `w`写(覆盖) `a`追加 `b`二进制

- `with open() as f:` 自动关闭文件（推荐方式）

- `encoding="utf-8"` 避免中文乱码

- `try/except` 捕获可能的文件异常

- `json.dumps/loads` 处理字符串，`json.dump/load` 处理文件



## 常见错误

- 忘记指定 `encoding="utf-8"` 导致中文乱码

- 用 `w` 模式打开已有文件 → 内容被清空

- 文件打开后忘记关闭（用 with 解决）

- 路径使用反斜杠 `\` 需要转义 `\\` 或使用 `r"..."` 原始字符串

