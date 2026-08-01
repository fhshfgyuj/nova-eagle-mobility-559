# 拖动拼图游戏



## 说明

一个简单的 HTML+JS 拖动游戏，将乱序的拼图块拖到正确的网格位置。完成后弹出恭喜提示。适合初学者学习 `dragstart/dragover/drop` 事件。



## 运行方式

用浏览器打开此 HTML 文件即可。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head>

<meta charset="UTF-8">

<title>拖动拼图游戏</title>

<style>

  body { font-family: Arial; text-align: center; margin-top: 20px; }

  .pieces { display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; margin: 20px; }

  .piece {

    width: 80px; height: 80px;

    background: #4CAF50; color: white; font-size: 28px; font-weight: bold;

    display: flex; align-items: center; justify-content: center;

    border-radius: 8px; cursor: grab; user-select: none;

  }

  .grid {

    display: grid; grid-template-columns: repeat(3, 80px);

    gap: 5px; justify-content: center; margin: 20px;

  }

  .slot {

    width: 80px; height: 80px;

    border: 3px dashed #aaa; border-radius: 8px;

    display: flex; align-items: center; justify-content: center;

    font-size: 14px; color: #aaa;

  }

  .slot.over { border-color: #4CAF50; background: #e8f5e9; }

  .slot.filled { border-color: #333; background: #c8e6c9; color: #333; font-size: 28px; }

  #message { font-size: 24px; color: #ff9800; margin: 10px; min-height: 30px; }

</style>

</head>

<body>

<h2>🧩 拖动拼图 - 把数字拖到正确位置</h2>

<div class="pieces" id="pieces"></div>

<div class="grid" id="grid"></div>

<div id="message"></div>

<button onclick="resetGame()">重新开始</button>



<script>

const SIZE = 3; // 3x3 拼图

let pieces = [1,2,3,4,5,6,7,8,9];

let placed = Array(SIZE*SIZE).fill(null);



function shuffle(arr) {

  for (let i = arr.length-1; i > 0; i--) {

    let j = Math.floor(Math.random() * (i+1));

    [arr[i], arr[j]] = [arr[j], arr[i]];

  }

}



function render() {

  // 渲染待拖动拼图块

  let piecesDiv = document.getElementById("pieces");

  piecesDiv.innerHTML = "";

  pieces.forEach(n => {

    let div = document.createElement("div");

    div.className = "piece";

    div.textContent = n;

    div.draggable = true;

    div.addEventListener("dragstart", e => {

      e.dataTransfer.setData("text/plain", n);

    });

    piecesDiv.appendChild(div);

  });



  // 渲染目标网格

  let gridDiv = document.getElementById("grid");

  gridDiv.innerHTML = "";

  for (let i = 0; i < SIZE*SIZE; i++) {

    let slot = document.createElement("div");

    slot.className = "slot";

    if (placed[i] !== null) {

      slot.classList.add("filled");

      slot.textContent = placed[i];

    } else {

      slot.textContent = i+1;

    }

    slot.addEventListener("dragover", e => {

      e.preventDefault();

      slot.classList.add("over");

    });

    slot.addEventListener("dragleave", () => slot.classList.remove("over"));

    slot.addEventListener("drop", e => {

      e.preventDefault();

      slot.classList.remove("over");

      let num = parseInt(e.dataTransfer.getData("text/plain"));

      let idx = pieces.indexOf(num);

      if (idx < 0) return;

      // 检查位置是否已被占用

      if (placed[i] !== null) return;

      pieces.splice(idx, 1);

      placed[i] = num;

      render();

      checkWin();

    });

    gridDiv.appendChild(slot);

  }

}



function checkWin() {

  let win = placed.every((v,i) => v === i+1);

  if (win) {

    document.getElementById("message").textContent = "🎉 恭喜完成！";

  }

}



function resetGame() {

  pieces = [1,2,3,4,5,6,7,8,9];

  placed = Array(SIZE*SIZE).fill(null);

  shuffle(pieces);

  document.getElementById("message").textContent = "";

  render();

}



// 启动

shuffle(pieces);

render();

</script>

</body>

</html>

```



## 教学重点

- `draggable="true"` 让元素可拖动

- `dragstart` 事件中用 `setData("text/plain", value)` 传递数据

- 目标元素用 `dragover` (需 `preventDefault()`) + `drop` 接收

- 状态管理：`pieces` 数组 + `placed` 数组驱动渲染

- `render()` 统一重绘，保证 UI 与数据一致



## 常见错误

- 忘记 `dragover` 中的 `e.preventDefault()` → 无法 drop

- `dataTransfer` 数据在 `dragstart` 设置、`drop` 读取，顺序不能错

- 拼图数索引从 0 开始，显示序号应 +1 或直接用值

