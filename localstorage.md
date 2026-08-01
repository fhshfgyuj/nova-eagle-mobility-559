# JavaScript：本地存储 localStorage



## 说明

用 localStorage 实现数据持久化：保存/读取设置、待办事项、用户偏好。页面刷新数据不丢失。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>本地存储</title>

<style>

body{font-family:Arial;max-width:500px;margin:20px auto;background:#f5f5f5}

.card{background:white;padding:20px;border-radius:12px;box-shadow:0 2px 10px rgba(0,0,0,0.08);margin:15px 0}

input{padding:10px;border:2px solid #ddd;border-radius:8px;width:100%;box-sizing:border-box;font-size:16px}

button{padding:10px 20px;background:#2196F3;color:white;border:none;border-radius:8px;cursor:pointer;margin:5px}

.todo-item{display:flex;justify-content:space-between;padding:10px;background:#f0f0f0;border-radius:8px;margin:5px 0;align-items:center}

</style></head>

<body>

<div class="card">

<h2>🎨 用户偏好</h2>

<label><input type="text" id="username" placeholder="你的名字"> <button onclick="saveName()">保存名称</button></label>

<p id="greeting"></p>

</div>



<div class="card">

<h2>📝 备忘录</h2>

<input id="todoInput" placeholder="输入待办事项...">

<button onclick="addTodo()">添加</button>

<div id="todoList"></div>

</div>



<div class="card">

<h2>🔢 页面访问计数器</h2>

<p id="visitCount"></p>

</div>



<button onclick="clearAll()" style="background:#f44336">清除所有数据</button>



<script>

// 页面加载时读取

window.addEventListener("DOMContentLoaded", () => {

    loadName();

    loadTodos();

    loadVisitCount();

});



// ====== 1. 用户名称 ======

function saveName() {

    const name = document.getElementById("username").value.trim();

    if (!name) return;

    localStorage.setItem("user_name", name);

    document.getElementById("greeting").textContent = `你好，${name}！👋`;

}



function loadName() {

    const name = localStorage.getItem("user_name");

    if (name) {

        document.getElementById("username").value = name;

        document.getElementById("greeting").textContent = `欢迎回来，${name}！👋`;

    }

}



// ====== 2. 待办事项 ======

function addTodo() {

    const input = document.getElementById("todoInput");

    const text = input.value.trim();

    if (!text) return;



    const todos = JSON.parse(localStorage.getItem("todos") || "[]");

    todos.push({ text, date: new Date().toLocaleDateString(), done: false });

    localStorage.setItem("todos", JSON.stringify(todos));



    input.value = "";

    loadTodos();

}



function toggleTodo(index) {

    const todos = JSON.parse(localStorage.getItem("todos") || "[]");

    todos[index].done = !todos[index].done;

    localStorage.setItem("todos", JSON.stringify(todos));

    loadTodos();

}



function deleteTodo(index) {

    const todos = JSON.parse(localStorage.getItem("todos") || "[]");

    todos.splice(index, 1);

    localStorage.setItem("todos", JSON.stringify(todos));

    loadTodos();

}



function loadTodos() {

    const todos = JSON.parse(localStorage.getItem("todos") || "[]");

    const list = document.getElementById("todoList");

    list.innerHTML = "";



    todos.forEach((todo, i) => {

        const div = document.createElement("div");

        div.className = "todo-item";

        div.innerHTML = `

            <span style="text-decoration:${todo.done?'line-through':'none'};cursor:pointer"

                  onclick="toggleTodo(${i})">

                ${todo.text} <small>(${todo.date})</small>

            </span>

            <button onclick="deleteTodo(${i})" style="background:#f44336;padding:4px 10px;font-size:12px">✕</button>

        `;

        list.appendChild(div);

    });

}



// ====== 3. 访问计数 ======

function loadVisitCount() {

    let count = parseInt(localStorage.getItem("visit_count") || "0");

    count++;

    localStorage.setItem("visit_count", count);

    document.getElementById("visitCount").textContent = `你已经访问了 ${count} 次`;

}



// ====== 清除 ======

function clearAll() {

    const msg = document.getElementById("greeting").textContent;

    localStorage.clear();  // 清空所有数据

    document.getElementById("greeting").textContent = msg + " (数据已清除)";

}

</script>

</body>

</html>

```



## 教学重点

- `setItem(key, value)` 写入、`getItem(key)` 读取、`removeItem(key)` 删除

- localStorage 只能存字符串，对象需 `JSON.stringify/parse`

- 数据容量约 5MB，同源下所有页面共享

- 数据永不过期（除非手动清除或用户清浏览器）

- `sessionStorage` 类似，但关闭标签页数据就消失



## 常见错误

- 忘记 `JSON.parse` 导致 `"[object Object]"` 字符串

- 数据太大（>5MB）导致写入失败 → `QuotaExceededError`

- 存放敏感信息（密码等）→ 可以用 `sessionStorage` 或加密

- `localStorage.clear()` 会清空同源所有数据

