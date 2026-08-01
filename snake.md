# 贪吃蛇



## 说明

经典贪吃蛇游戏。方向键控制蛇吃食物变长，撞墙或撞到自己则结束。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>贪吃蛇</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#111;color:white}

canvas{border:2px solid #4CAF50;background:#1a1a1a}

#score{font-size:24px;margin:10px}

</style></head>

<body>

<h2>🐍 贪吃蛇</h2>

<div id="score">得分: 0</div>

<canvas id="c" width="400" height="400"></canvas>

<script>

const c=document.getElementById("c"),ctx=c.getContext("2d");

const g=20;let snake=[{x:5,y:5}],dx=1,dy=0,food={},score=0,speed=100,game;

function sp(){food={x:Math.floor(Math.random()*20),y:Math.floor(Math.random()*20)};}

sp();

function tick(){

  let h={x:snake[0].x+dx,y:snake[0].y+dy};

  if(h.x<0||h.x>=20||h.y<0||h.y>=20||snake.some(s=>s.x===h.x&&s.y===h.y)){clearInterval(game);alert("Game Over! 得分:"+score);return;}

  snake.unshift(h);

  if(h.x===food.x&&h.y===food.y){score+=10;document.getElementById("score").textContent="得分: "+score;sp();}else snake.pop();

  ctx.clearRect(0,0,400,400);

  ctx.fillStyle="#4CAF50";snake.forEach(s=>ctx.fillRect(s.x*g,s.y*g,g-2,g-2));

  ctx.fillStyle="#f44336";ctx.fillRect(food.x*g,food.y*g,g-2,g-2);

}

document.addEventListener("keydown",e=>{switch(e.key){case"ArrowUp":if(dy!==1){dx=0;dy=-1}break;case"ArrowDown":if(dy!==-1){dx=0;dy=1}break;case"ArrowLeft":if(dx!==1){dx=-1;dy=0}break;case"ArrowRight":if(dx!==-1){dx=1;dy=0}break;}});

game=setInterval(tick,speed);

</script></body></html>

```

