# Canvas 简易画板



## 说明

最基础的 HTML5 Canvas 画板：鼠标按住即可涂鸦。支持颜色选择和画笔大小调整。是 Canvas 绘画 API 的入门项目。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>简易画板</title>

<style>

body{text-align:center;font-family:Arial;margin:10px}

canvas{border:2px solid #333;cursor:crosshair;background:white;box-shadow:0 2px 10px rgba(0,0,0,0.1)}

.tools{margin:10px;display:flex;justify-content:center;gap:15px;align-items:center}

.tools input[type=color]{width:40px;height:40px}

.tools input[type=range]{width:120px}

button{padding:8px 20px;border:none;border-radius:6px;cursor:pointer;background:#f44336;color:white}

</style></head>

<body>

<h2>🖌️ 简易画板</h2>

<div class="tools">

  <input type="color" id="colorPicker" value="#333333">

  <span>粗细:</span><input type="range" id="brushSize" min="1" max="20" value="3">

  <button onclick="clearCanvas()">清空</button>

  <button onclick="saveCanvas()">保存</button>

</div>

<canvas id="canvas" width="600" height="450"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let drawing = false;



canvas.addEventListener("mousedown", e => {

  drawing = true;

  ctx.beginPath();

  ctx.moveTo(e.offsetX, e.offsetY);

});

canvas.addEventListener("mousemove", e => {

  if (!drawing) return;

  ctx.lineTo(e.offsetX, e.offsetY);

  ctx.strokeStyle = document.getElementById("colorPicker").value;

  ctx.lineWidth = document.getElementById("brushSize").value;

  ctx.lineCap = "round"; ctx.lineJoin = "round";

  ctx.stroke();

});

canvas.addEventListener("mouseup", () => { drawing = false; ctx.closePath(); });

canvas.addEventListener("mouseleave", () => drawing = false);



// 触屏支持

canvas.addEventListener("touchstart", e => {

  e.preventDefault(); drawing = true;

  let t = e.touches[0];

  ctx.beginPath(); ctx.moveTo(t.clientX-canvas.offsetLeft, t.clientY-canvas.offsetTop);

});

canvas.addEventListener("touchmove", e => {

  e.preventDefault();

  if (!drawing) return;

  let t = e.touches[0];

  ctx.lineTo(t.clientX-canvas.offsetLeft, t.clientY-canvas.offsetTop);

  ctx.strokeStyle = document.getElementById("colorPicker").value;

  ctx.lineWidth = document.getElementById("brushSize").value;

  ctx.lineCap = "round"; ctx.lineJoin = "round";

  ctx.stroke();

});

canvas.addEventListener("touchend", () => { drawing = false; ctx.closePath(); });



function clearCanvas() { ctx.clearRect(0, 0, canvas.width, canvas.height); }

function saveCanvas() {

  let link = document.createElement("a");

  link.download = "我的画作.png";

  link.href = canvas.toDataURL();

  link.click();

}

</script></body></html>

```

