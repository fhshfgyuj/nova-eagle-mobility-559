# JavaScript：调试技巧大全



## 说明

学习 Chrome DevTools 调试、console 各种方法、断点、性能分析。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>调试技巧</title></head>

<body>

<h2>🐛 JavaScript 调试技巧</h2>

<button onclick="debugDemo()">运行调试演示</button>

<button onclick="perfTest()">性能测试</button>

<pre id="log" style="background:#1e1e1e;color:#d4d4d4;padding:15px;font-family:monospace"></pre>



<script>

const logEl = document.getElementById("log");

function capture(msg) { logEl.textContent += msg + "\n"; }



// 拦截 console 输出

const origLog = console.log;

console.log = function (...args) {

    capture(args.join(" "));

    origLog.apply(console, args);

};



function debugDemo() {

    logEl.textContent = "";



    // 1. console 各种方法

    console.log("✅ 普通日志");

    console.warn("⚠️ 警告信息");

    console.error("❌ 错误信息");

    console.info("ℹ️ 提示信息");



    // 2. 表格输出（数组/对象）

    const users = [

        { name: "小明", age: 12, score: 90 },

        { name: "小红", age: 13, score: 85 },

    ];

    console.table(users);



    // 3. 分组输出

    console.group("🔍 数据详情");

    console.log("名称: test");

    console.log("数值: 42");

    console.groupEnd();



    // 4. 计时

    console.time("计时器");

    let sum = 0;

    for (let i = 0; i < 1000000; i++) sum += i;

    console.timeEnd("计时器");



    // 5. 断言

    console.assert(1 + 1 === 3, "数学错了？");  // 条件为假才输出



    // 6. trace 调用栈

    function a() { b(); }

    function b() { c(); }

    function c() { console.trace("调用栈"); }

    a();



    // 7. debugger 断点

    const x = 10;

    const y = 20;

    // debugger;  // ← 取消注释这行，代码会在此暂停

    console.log(`x + y = ${x + y}`);



    // 8. 条件断点（在 DevTools 中右键行号→Add conditional breakpoint）

    for (let i = 0; i < 5; i++) {

        console.log(`循环: i = ${i}`);

/    }

}



function perfTest() {

    logEl.textContent = "";

    console.log("=== 性能对比 ===");



    const arr = new Array(10000).fill(0).map((_, i) => i);



    // for 循环

    console.time("for");

    let s1 = 0;

    for (let i = 0; i < arr.length; i++) s1 += arr[i];

    console.timeEnd("for");



    // for...of

    console.time("for-of");

    let s2 = 0;

    for (const v of arr) s2 += v;

    console.timeEnd("for-of");



    // reduce

    console.time("reduce");

    const s3 = arr.reduce((a, b) => a + b, 0);

    console.timeEnd("reduce");



    // forEach

    console.time("forEach");

    let s4 = 0;

    arr.forEach(v => s4 += v);

    console.timeEnd("forEach");



    console.log(`结果: ${s1} ${s2} ${s3} ${s4}`);

}

</script>

</body>

</html>

```



## 教学重点

- 打开 Chrome DevTools: F12 → Sources 面板 → 点击行号设断点

- `debugger` 语句强制代码在此处暂停

- `console.table` 查看数组/对象；`console.group` 分组日志

- `console.time/timeEnd` 测量代码块执行时间

- `console.trace` 打印函数调用栈

- Sources 面板中 Watch、Scope、Call Stack 实时查看变量



## 常见错误

- 在生产环境保留 `debugger`/`console.log` → 可能泄露信息

- 性能测试时被 JIT 优化影响（多次运行取平均）

- 异步代码中的断点可能混乱（用 await 或 .then 方式先同步化）

- 忽略 Network 面板 → 接口调用失败在 Console 不一定报错

