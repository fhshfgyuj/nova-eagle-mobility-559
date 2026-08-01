# 连线画板（点连线）



## 说明

在画布上点击放置顶点，系统自动连线组成多边形或折线。可闭合图形并填充颜色。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>连线画板</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#e8eaf6}

canvas{border:2px solid #3f51b5;background:white;cursor:crosshair}

.tools{margin:10px}

button{padding:6px 16px;border:none;border-radius:6px;cursor:pointer;margin:3px}

</style></head>

<body>

<h2>📐 连线画板 - 点击放置顶点</h2>

<div class="tools">

  <button style="background:#4CAF50;color:white" onclick="closeShape()">闭合图形</button>

  <button style="background:#ff9800;color:white" onclick="fillShape()">填充</button>

  <button style="background:#f44336;color:white" onclick="clearAll()">清空</button>

  <button style="background:#2196F3;color:white" onclick="undoPoint()">撤销点</button>

  <input type="color" id="strokeColor" value="#3f51b5">

  <input type="color" id="fillColor" value="#c5cae9">

</div>

<canvas id="canvas" width="650" height="450"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let points = [];

let closed = false;



canvas.addEventListener("click", e => {

  if (closed) return;  // 已闭合不能加新点

  let rect = canvas.getBoundingClientRect();

  let x = e.clientX - rect.left;

  let y = e.clientY - rect.top;

  points.push({x, y});

  drawAll();

});



function drawAll() {

  ctx.clearRect(0, 0, canvas.width, canvas.height);

  if (points.length === 0) return;



  ctx.strokeStyle = document.getElementById("strokeColor").value;

  ctx.lineWidth = 2;

  ctx.fillStyle = document.getElementById("fillColor").value;



  ctx.beginPath();

  ctx.moveTo(points[0].x, points[0].y);

  for (let i = 1; i < points.length; i++) {

    ctx.lineTo(points[i].x, points[i].y);

  }

  if (closed && points.length > 2) {

    ctx.closePath();

    ctx.fill();

  }

  ctx.stroke();



  // 画顶点

  points.forEach((p, i) => {

    ctx.beginPath();

    ctx.arc(p.x, p.y, 5, 0, Math.PI*2);

    ctx.fillStyle = i === 0 ? "#4CAF50" : "#ff9800";

    ctx.fill();

    ctx.fillStyle = "#333";

    ctx.font = "12px Arial";

    ctx.fillText(i+1, p.x+8, p.y-8);

  });

}



function closeShape() {

  if (points.length < 3) { alert("至少需要3个点！"); return; }

  closed = true;

  drawAll();

}



function fillShape() {

  if (!closed) closeShape();

  drawAll();

}



function clearAll() {

  points = []; closed = false;

  ctx.clearRect(0,0,canvas.width,canvas.height);

}



function undoPoint() {

  if (closed) { closed = false; drawAll(); return; }

  points.pop();

  drawAll();

}

</script></body></html>

```

