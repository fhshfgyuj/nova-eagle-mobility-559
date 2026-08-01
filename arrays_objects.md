# JavaScript：数组与对象操作



## 说明

JS 数组的 map/filter/reduce 三板斧，对象的解构展开，以及 JSON 序列化。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>数组与对象</title></head>

<body>

<pre id="out" style="background:#f5f5f5;padding:15px;font-family:monospace;border-radius:8px"></pre>

<script>

const out = document.getElementById("out");

function print(label, val) {

    out.innerHTML += `${label}: ${JSON.stringify(val, null, 2)}\n\n`;

}



// ====== 数 组 ======

const students = [

    { name: "小明", score: 85, grade: "七" },

    { name: "小红", score: 92, grade: "八" },

    { name: "小刚", score: 78, grade: "七" },

    { name: "小美", score: 95, grade: "八" },

];



// map：转换数组（不改原数组）

const names = students.map(s => s.name);

print("map 提取姓名", names);



// filter：过滤

const passing = students.filter(s => s.score >= 80);

print("filter >=80", passing.map(s => s.name));



// reduce：归约

const total = students.reduce((sum, s) => sum + s.score, 0);

const avg = total / students.length;

print(`reduce 总分=${total} 均分=${avg}`, "");



// 链式调用

const topTwo = students

    .filter(s => s.grade === "八")

    .map(s => s.name.toUpperCase());

print("链式调用", topTwo);



// find/findIndex/some/every

print("find 第一个>=90", students.find(s => s.score >= 90));

print("some 是否有100", students.some(s => s.score === 100));

print("every 是否都≥60", students.every(s => s.score >= 60));



// sort

students.sort((a, b) => b.score - a.score);

print("按分数降序", students);



// 展开运算符

const arr1 = [1, 2, 3];

const arr2 = [4, 5, 6];

print("展开合并", [...arr1, ...arr2]);



// ====== 对 象 ======

const user = { name: "张三", age: 25, city: "北京" };



// 解构

const { name, age, ...rest } = user;

print("解构", { name, age, rest });



// 展开合并

const userExtra = { ...user, hobby: "游泳", age: 26 };

print("展开覆盖 age", userExtra);



// Object.keys/values/entries

print("keys", Object.keys(user));

print("entries", Object.entries(user));



// ====== JSON ======

const jsonStr = JSON.stringify(user);

const parsed = JSON.parse(jsonStr);

print("JSON 序列化", jsonStr);

</script>

</body>

</html>

```



## 教学重点

- **map** 变换每个元素；**filter** 筛选；**reduce** 聚合（总和、最大、对象拼接）

- 链式调用 `.filter().map().sort()` 写出声明式代码

- 对象解构 `{ name, age }` 和数组解构 `[first, second]`

- 展开符 `...` 可合并数组/对象、传参

- `JSON.stringify/parse` 序列化与反序列化



## 常见错误

- `map/filter` 返回新数组，忘记赋值接收

- `sort()` 默认按字符串排序（数字也按字符串）

- `reduce` 忘记初始值，空数组不传入初始值报错

- `JSON.parse` 遇到非法 JSON 会抛异常（用 `try/catch`）

