# 数字华容道



## 说明

滑动数字方块排列成 1~8 的顺序，经典的 3x3 华容道。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>数字华容道</title>

<style>

body{text-align:center;font-family:Arial;margin:20px;background:#fff3e0}

.puzzle{display:grid;grid-template-columns:repeat(3,80px);gap:3px;width:fit-content;margin:20px auto;background:#bf360c;padding:3px;border-radius:8px}

.tile{width:80px;height:80px;display:flex;align-items:center;justify-content:center;font-size:32px;font-weight:bold;background:#ffcc80;border-radius:6px;cursor:pointer;color:#bf360c}

.tile.empty{background:transparent;cursor:default}

</style></head>

<body>

<h2>🔢 数字华容道</h2>

<div class="puzzle" id="board"></div>

<div id="msg"></div>

<button onclick="shuffle()" style="margin:10px;padding:10px 25px;background:#ff9800;color:white;border:none;border-radius:8px;cursor:pointer;font-size:16px">打乱</button>



<script>

let board=[1,2,3,4,5,6,7,8,0],emptyIdx=8;



function render(){

  let b=document.getElementById("board");b.innerHTML="";

  board.forEach((v,i)=>{

    let d=document.createElement("div");d.className="tile";

    if(v===0)d.classList.add("empty");

    d.textContent=v||"";

    d.addEventListener("click",()=>move(i));

    b.appendChild(d);

  });

}

function move(i){

  let er=Math.floor(emptyIdx/3),ec=emptyIdx%3,ir=Math.floor(i/3),ic=i%3;

  if(Math.abs(er-ir)+Math.abs(ec-ic)===1){

    [board[i],board[emptyIdx]]=[board[emptyIdx],board[i]];

    emptyIdx=i;render();

    if(board.join()==="1,2,3,4,5,6,7,8,0")document.getElementById("msg").textContent="🎉 完成!";

  }

}

function shuffle(){

  for(let i=0;i<100;i++){

    let moves=[],er=Math.floor(emptyIdx/3),ec=emptyIdx%3;

    [[er-1,ec],[er+1,ec],[er,ec-1],[er,ec+1]].forEach(([r,c])=>{if(r>=0&&r<3&&c>=0&&c<3)moves.push(r*3+c);});

    let next=moves[Math.floor(Math.random()*moves.length)];

    [board[next],board[emptyIdx]]=[board[emptyIdx],board[next]];emptyIdx=next;

  }

  document.getElementById("msg").textContent="";render();

}

render();

</script></body></html>

```

