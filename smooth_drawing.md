# 平滑画笔（贝塞尔曲线优化）



## 说明

使用贝塞尔曲线插值让鼠标画出的线条更加平滑，解决快速移动时线条变折线的问题。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>平滑画笔</title>

<style>

body{text-align:center;font-family:Arial;margin:10px}

canvas{border:2px solid #333;background:white}

</style></head>

<body>

<h2>✒️ 平滑贝塞尔画笔</h2>

<input type="color" id="color" value="#333"><input type="range" id="size" min="1" max="15" value="3">

<button onclick="ctx.clearRect(0,0,600,400)" style="background:#f44336;color:white;border:none;padding:6px 15px;border-radius:6px;cursor:pointer">清空</button>

<canvas id="canvas" width="600" height="400"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let points = [], drawing = false;



canvas.addEventListener("mousedown", e => {

  drawing = true; points = [];

  points.push({ x: e.offsetX, y: e.offsetY });

});



canvas.addEventListener("mousemove", e => {

  if (!drawing) return;

  points.push({ x: e.offsetX, y: e.offsetY });



  if (points.length < 3) {

    // 只有两点时直接连线

    ctx.beginPath(); ctx.moveTo(points[0].x, points[0].y);

    ctx.lineTo(points[1].x, points[1].y);

    ctx.strokeStyle = document.getElementById("color").value;

    ctx.lineWidth = +document.getElementById("size").value;

    ctx.lineCap = "round"; ctx.stroke();

    return;

  }



  // 用贝塞尔曲线平滑

  let p0 = points[points.length - 3];

  let p1 = points[points.length - 2];

  let p2 = points[points.length - 1];



  let cx1 = (p0.x + p1.x) / 2;

  let cy1 = (p0.y + p1.y) / 2;

  let cx2 = (p1.x + p2.x) / 2;

  let cy2 = (p1.y + p2.y) / 2;



  ctx.beginPath();

  ctx.moveTo(cx1, cy1);

  ctx.quadraticCurveTo(p1.x, p1.y, cx2, cy2);

  ctx.strokeStyle = document.getElementById("color").value;

  ctx.lineWidth = +document.getElementById("size").value;

  ctx.lineCap = "round";

  ctx.stroke();

});



canvas.addEventListener("mouseup", () => { drawing = false; points = []; });

</script></body></html>

```

