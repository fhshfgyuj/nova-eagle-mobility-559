# 对称画板



## 说明

在左侧画图，右侧自动镜像对称生成另一半。非常适合画蝴蝶、人脸等对称图案。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>对称画板</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#f5f5f5}

canvas{border:2px solid #333;background:white;cursor:crosshair}

.tools{margin:10px}

.tools input[type=color]{width:30px;height:30px}

.tools input[type=range]{width:100px}

button{padding:6px 16px;border:none;border-radius:6px;background:#f44336;color:white;cursor:pointer}

</style></head>

<body>

<h2>🦋 对称画板</h2>

<div class="tools">

  <input type="color" id="color" value="#ff69b4">

  <input type="range" id="size" min="1" max="15" value="3">

  <button onclick="clearCanvas()">清空</button>

  <button onclick="document.getElementById('vertical').checked=!document.getElementById('vertical').checked">切换轴</button>

</div>

<div>

  <canvas id="canvas" width="700" height="500"></canvas>

</div>

<div><label><input type="checkbox" id="vertical" checked> 垂直镜像</label></div>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let drawing = false;

let midX = canvas.width / 2, midY = canvas.height / 2;



function drawPoint(x, y) {

  let c = document.getElementById("color").value, s = +document.getElementById("size").value;

  let vertical = document.getElementById("vertical").checked;



  ctx.fillStyle = c;

  // 原点

  ctx.beginPath(); ctx.arc(x, y, s, 0, Math.PI*2); ctx.fill();

  // 对称点

  let sx = vertical ? 2*midX - x : x;

  let sy = vertical ? y : 2*midY - y;

  ctx.beginPath(); ctx.arc(sx, sy, s, 0, Math.PI*2); ctx.fill();

}



function getPos(e) {

  let rect = canvas.getBoundingClientRect();

  return { x: e.clientX - rect.left, y: e.clientY - rect.top };

}



canvas.addEventListener("mousedown", e => { drawing = true; let p = getPos(e); drawPoint(p.x, p.y); });

canvas.addEventListener("mousemove", e => { if (drawing) { let p = getPos(e); drawPoint(p.x, p.y); } });

canvas.addEventListener("mouseup", () => drawing = false);



canvas.addEventListener("touchstart", e => { e.preventDefault(); drawing = true; let t=e.touches[0]; drawPoint(t.clientX-canvas.offsetLeft,t.clientY-canvas.offsetTop); });

canvas.addEventListener("touchmove", e => { e.preventDefault(); if(drawing){ let t=e.touches[0]; drawPoint(t.clientX-canvas.offsetLeft,t.clientY-canvas.offsetTop); }});

canvas.addEventListener("touchend", () => drawing = false);



function clearCanvas() { ctx.clearRect(0, 0, canvas.width, canvas.height); }

</script></body></html>

```

