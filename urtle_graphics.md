# 代码控制的海龟绘图



## 说明

仿照 Logo 海龟绘图，输入前进/旋转/颜色指令，小海龟在画布上画出漂亮的图形。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>海龟绘图</title>

<style>

body{text-align:center;font-family:monospace;margin:10px}

canvas{border:2px solid #333;background:white}

.controls{margin:10px;max-width:700px;margin-left:auto;margin-right:auto}

textarea{width:100%;height:80px;font-family:monospace;font-size:14px}

button{padding:8px 16px;border:none;border-radius:6px;cursor:pointer;margin:3px;background:#4CAF50;color:white}

.examples{text-align:left;margin:5px 0}

</style></head>

<body>

<h2>🐢 海龟绘图</h2>

<canvas id="canvas" width="600" height="500"></canvas>

<div class="controls">

  <textarea id="code"># 五角星

repeat 5 [

  fd 150

  rt 144

]

# 正方形

fd 80 rt 90

fd 80 rt 90

fd 80 rt 90

fd 80 rt 90</textarea>

  <br>

  <button onclick="runCode()">▶ 运行</button>

  <button onclick="clearCanvas()" style="background:#f44336">清空</button>

  <button onclick="loadStar()" style="background:#ff9800">五角星</button>

  <button onclick="loadSpiral()" style="background:#2196F3">螺旋</button>

  <button onclick="loadTree()" style="background:#9c27b0">分形树</button>

</div>

<div class="examples">

  命令: fd N(前进) | bk N(后退) | rt N(右转°) | lt N(左转°) | color #rrggbb(颜色) | pu(抬笔) | pd(落笔) | repeat N [ ... ]

</div>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let x, y, angle, penDown;



function init() {

  ctx.clearRect(0,0,600,500);

  x = 300; y = 250; angle = -90; penDown = true;

  ctx.beginPath(); ctx.moveTo(x,y);

}



function fd(d) {

  let rad = angle * Math.PI / 180;

  let nx = x + Math.cos(rad) * d;

  let ny = y + Math.sin(rad) * d;

  if (penDown) { ctx.lineTo(nx, ny); ctx.stroke(); }

  else ctx.moveTo(nx, ny);

  x = nx; y = ny;

}



function rt(a) { angle += a; }

function lt(a) { angle -= a; }



function runCode() {

  init();

  let code = document.getElementById("code").value;

  let lines = code.split("\n").filter(l => l.trim() && !l.trim().startsWith("#"));



  // 处理 repeat 块

  let expanded = [];

  for (let i = 0; i < lines.length; i++) {

    let line = lines[i].trim();

    let m = line.match(/^repeat\s+(\d+)\s*\[/);

    if (m) {

      let count = parseInt(m[1]);

      let block = [];

      for (i++; i < lines.length && !lines[i].trim().startsWith("]"); i++) {

        block.push(lines[i].trim());

      }

      for (let j = 0; j < count; j++) expanded.push(...block);

    } else if (!line.startsWith("]")) {

      expanded.push(line);

    }

  }



  for (let cmd of expanded) {

    cmd = cmd.trim();

    if (!cmd) continue;

    let parts = cmd.split(/\s+/);

    let op = parts[0].toLowerCase();

    let val = parseFloat(parts[1]);



    switch(op) {

      case "fd": case "forward": fd(val); break;

      case "bk": case "backward": fd(-val); break;

      case "rt": case "right": rt(val); break;

      case "lt": case "left": lt(val); break;

      case "pu": case "penup": penDown = false; break;

      case "pd": case "pendown": penDown = true; break;

      case "color":

        ctx.strokeStyle = parts[1];

        ctx.beginPath(); ctx.moveTo(x, y);

        break;

    }

  }

}



function clearCanvas() { init(); }



function loadStar() { document.getElementById("code").value = "repeat 5 [\n  fd 150\n  rt 144\n]"; runCode(); }

function loadSpiral() { document.getElementById("code").value = "repeat 100 [\n  fd 4\n  rt 15\n]"; runCode(); }

function loadTree() { document.getElementById("code").value = "# 在浏览器开发者控制台运行 showTree() 查看分形树\nfd 100\nrt 30\nfd 60\nbk 60\nlt 60\nfd 60"; runCode(); }



init();

</script></body></html>

```

