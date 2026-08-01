# Python：常用标准库合集



## 说明

15 个 Python 最常用的标准库的实战示例：random、math、os、sys、re、collections、itertools、functools 等。



## 代码

```python

import random, math, os, sys, re

from collections import Counter, defaultdict, deque

from itertools import permutations, combinations, chain

from functools import lru_cache, reduce



# ====== random：随机数 ======

print("随机 1-100:", random.randint(1, 100))

print("随机浮点:", random.random())

print("随机选择:", random.choice(["A","B","C"]))

items = [1,2,3,4,5]

random.shuffle(items); print("洗牌:", items)

print("5 个样本:", random.sample(range(100), 5))



# ====== math：数学 ======

print(f"π={math.pi:.4f}, e={math.e:.4f}")

print("sqrt(16)=", math.sqrt(16))

print("2^10=", math.pow(2, 10))

print("ceil(3.1)=", math.ceil(3.1), "floor(3.9)=", math.floor(3.9))

print("sin(30°)=", math.sin(math.radians(30)))



# ====== os + sys：系统交互 ======

print("当前目录:", os.getcwd())

print("Python 版本:", sys.version[:20])

print("平台:", sys.platform)



# 目录操作

demo_dir = "demo_folder"

os.makedirs(demo_dir, exist_ok=True)

with open(f"{demo_dir}/test.txt", "w") as f:

    f.write("test")

os.remove(f"{demo_dir}/test.txt")

os.rmdir(demo_dir)



# ====== re：正则表达式 ======

text = "联系方式: Email: test@example.com, Phone: 138-1234-5678"



email_match = re.search(r'[\w.]+@[\w.]+\w+', text)

phone_match = re.findall(r'\d{3}-\d{4}-\d{4}', text)



if email_match: print("邮箱:", email_match.group())

print("手机号:", phone_match)



# 替换

clean = re.sub(r'\d{3}-\d{4}-\d{4}', '***-****-****', text)

print(clean)



# ====== collections：高级容器 ======

# Counter 计数器

words = "the quick brown fox jumps over the lazy dog".split()

count = Counter(words)

print("\n词频统计:", count.most_common(3))



# defaultdict 默认字典

dd = defaultdict(list)

dd['a'].append(1); dd['a'].append(2)

print("defaultdict:", dict(dd))



# deque 双端队列

d = deque([1,2,3])

d.appendleft(0); d.append(4)

print("deque:", list(d))



# ====== itertools：迭代器工具 ======

print("\n排列组合:")

print("  3选2排列:", list(permutations("ABC", 2)))

print("  3选2组合:", list(combinations("ABC", 2)))



# chain 串联迭代器

print("  合并:", list(chain([1,2], [3,4], "ab")))



# ====== functools：函数工具 ======

# 缓存

@lru_cache(maxsize=128)

def fib(n):

    if n < 2: return n

    return fib(n-1) + fib(n-2)



print(f"\nfib(30) = {fib(30)}")  # 有缓存，瞬间计算



# reduce

nums = [1, 2, 3, 4, 5]

print("累加:", reduce(lambda a, b: a + b, nums))

print("阶乘:", reduce(lambda a, b: a * b, range(1, 6)))

```



## 教学重点

- `random` 的 `randint/choice/shuffle/sample` 是最常用的 4 个函数

- `math` 的三角函数参数是弧度不是度

- `re.search` vs `re.findall` vs `re.sub` 的区别

- `Counter/OrderedDict/defaultdict/deque` 各司其职

- `itertools` 处理排列组合和迭代器链式调用

- `@lru_cache` 是简单缓存神器



## 常见错误

- 正则表达式中 `.` 不匹配换行符，用 `re.DOTALL` flag

- `math.radians/degrees` 角度弧度转换

- `Counter` 访问不存在的 key 返回 0（不报错）

- `@lru_cache` 的缓存大小无限增长（设置 maxsize 限制）

