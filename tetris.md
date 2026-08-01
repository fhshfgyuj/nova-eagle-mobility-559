# 俄罗斯方块



## 说明

经典俄罗斯方块。上下左右控制，消除满行得分。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>俄罗斯方块</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#111;color:white}

canvas{background:#1a1a1a;border:2px solid #333}

</style></head>

<body>

<h2>🧱 俄罗斯方块</h2><div id="score">得分: 0</div>

<canvas id="c" width="200" height="400"></canvas>

<script>

const ctx=document.getElementById("c").getContext("2d"),cs=20,w=10,h=20;

let board=Array(h).fill().map(()=>Array(w).fill(0));

let pieces=[[[1,1,1,1]],[[1,1],[1,1]],[[0,1,0],[1,1,1]],[[1,0,0],[1,1,1]],[[0,0,1],[1,1,1]],[[1,1,0],[0,1,1]],[[0,1,1],[1,1,0]]];

let cur,px,py,score=0,gamespeed=500;



function newP(){cur=pieces[Math.floor(Math.random()*pieces.length)];px=Math.floor(w/2)-Math.floor(cur[0].length/2);py=0;if(!valid(px,py)){alert("Game Over! 得分:"+score);location.reload();}}

function valid(x,y,p){for(let r=0;r<cur.length;r++)for(let c=0;c<cur[0].length;c++)if(cur[r][c]){let nx=x+c,ny=y+r;if(nx<0||nx>=w||ny>=h||(ny>=0&&board[ny][nx]))return false;}return true;}

function freeze(){for(let r=0;r<cur.length;r++)for(let c=0;c<cur[0].length;c++)if(cur[r][c])board[py+r][px+c]=1;

  for(let r=h-1;r>=0;r--){if(board[r].every(v=>v)){board.splice(r,1);board.unshift(Array(w).fill(0));score+=100;document.getElementById("score").textContent="得分: "+score;}}newP();}



function draw(){ctx.clearRect(0,0,200,400);

  board.forEach((row,r)=>row.forEach((v,c)=>{if(v){ctx.fillStyle="#2196F3";ctx.fillRect(c*cs,r*cs,cs-1,cs-1);}}));

  cur.forEach((row,r)=>row.forEach((v,c)=>{if(v){ctx.fillStyle="#ff9800";ctx.fillRect((px+c)*cs,(py+r)*cs,cs-1,cs-1);}}));

}



document.addEventListener("keydown",e=>{

  if(e.key==="ArrowLeft"&&valid(px-1,py))px--;

  if(e.key==="ArrowRight"&&valid(px+1,py))px++;

  if(e.key==="ArrowDown"){if(valid(px,py+1))py++;else freeze();}

  if(e.key==="ArrowUp"){let rot=cur[0].map((_,i)=>cur.map(r=>r[i]).reverse());let old=cur;cur=rot;if(!valid(px,py))cur=old;}

  draw();

});



function tick(){if(valid(px,py+1))py++;else freeze();draw();}

newP();setInterval(tick,gamespeed);draw();

</script></body></html>

```

