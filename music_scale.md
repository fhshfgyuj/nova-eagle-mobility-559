# 拖拽排序音阶游戏



## 说明

将音符从低到高排列，拖拽后点击播放按钮听音阶。学习音乐与编程的结合。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head>

<meta charset="UTF-8">

<title>音阶拖拽游戏</title>

<style>

  body { font-family: Arial; text-align: center; margin: 20px; background: #f3e5f5; }

  h2 { color: #7b1fa2; }

  .notes-area { display: flex; justify-content: center; gap: 10px; min-height: 80px; margin: 20px; flex-wrap: wrap; }

  .note { width: 70px; height: 70px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 28px; font-weight: bold; cursor: grab; color: white; user-select: none; }

  .target { display: flex; justify-content: center; gap: 5px; }

  .slot { width: 70px; height: 50px; border: 2px dashed #ce93d8; border-radius: 8px; display: flex; align-items: center; justify-content: center; color: #ce93d8; }

  .slot.over { background: #f3e5f5; }

  .slot.filled { background: #e1bee7; }

  #message { font-size: 20px; min-height: 30px; margin: 10px; }

  button { padding: 12px 30px; margin: 5px; border: none; border-radius: 25px; font-size: 16px; cursor: pointer; }

  .play-btn { background: #7b1fa2; color: white; }

</style>

</head>

<body>

<h2>🎵 音阶排序游戏</h2>

<p>把音符从低到高拖放到正确位置</p>

<div class="notes-area" id="notesArea"></div>

<div class="target" id="targetArea"></div>

<div id="message"></div>

<button class="play-btn" onclick="playScale()">🔊 播放音阶</button>

<button onclick="resetGame()" style="background:#e91e63;color:white">重新开始</button>



<script>

const notes = [

  { name: "Do", freq: 261.63, color: "#ff0000" },

  { name: "Re", freq: 293.66, color: "#ff7f00" },

  { name: "Mi", freq: 329.63, color: "#ffff00" },

  { name: "Fa", freq: 349.23, color: "#00ff00" },

  { name: "Sol",freq: 392.00, color: "#0000ff" },

  { name: "La", freq: 440.00, color: "#4b0082" },

  { name: "Si", freq: 493.88, color: "#8b00ff" },

];



let available = [...notes];

let placed = Array(notes.length).fill(null);



function render() {

  let notesArea = document.getElementById("notesArea");

  notesArea.innerHTML = "";

  available.forEach((n, idx) => {

    let div = document.createElement("div");

    div.className = "note";

    div.style.background = n.color;

    div.textContent = n.name;

    div.draggable = true;

    div.addEventListener("dragstart", e =>

      e.dataTransfer.setData("text/plain", JSON.stringify({...n, idx}))

    );

    notesArea.appendChild(div);

  });



  let targetArea = document.getElementById("targetArea");

  targetArea.innerHTML = "";

  for (let i = 0; i < notes.length; i++) {

    let slot = document.createElement("div");

    slot.className = "slot";

    if (placed[i]) {

      slot.classList.add("filled");

      let n = document.createElement("div");

      n.className = "note";

      n.style.background = placed[i].color;

      n.textContent = placed[i].name;

      n.style.width = "60px";

      n.style.height = "60px";

      n.style.fontSize = "24px";

      n.addEventListener("click", () => {

        // 点击放回

        available.push(placed[i]);

        placed[i] = null;

        render();

      });

      slot.appendChild(n);

    } else {

      slot.textContent = i+1;

    }

    slot.addEventListener("dragover", e => { e.preventDefault(); slot.classList.add("over"); });

    slot.addEventListener("dragleave", () => slot.classList.remove("over"));

    slot.addEventListener("drop", e => {

      e.preventDefault(); slot.classList.remove("over");

      if (placed[i]) return;

      let data = JSON.parse(e.dataTransfer.getData("text/plain"));

      placed[i] = data;

      available.splice(data.idx, 1);

      render();

      checkWin();

    });

    targetArea.appendChild(slot);

  }

}



function checkWin() {

  let correct = placed.every((n, i) => n && n.name === notes[i].name);

  if (correct) document.getElementById("message").textContent = "✅ 正确！";

}



function playScale() {

  let audioCtx = new (window.AudioContext || window.webkitAudioContext)();

  let playTime = 0;

  notes.forEach((note, i) => {

    setTimeout(() => {

      let osc = audioCtx.createOscillator();

      let gain = audioCtx.createGain();

      osc.connect(gain); gain.connect(audioCtx.destination);

      osc.frequency.value = note.freq;

      osc.type = "sine";

      gain.gain.setValueAtTime(0.3, audioCtx.currentTime);

      gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.5);

      osc.start(); osc.stop(audioCtx.currentTime + 0.5);

    }, playTime);

    playTime += 400;

  });

}



function resetGame() {

  available = [...notes];

  placed = Array(notes.length).fill(null);

  document.getElementById("message").textContent = "";

  render();

}



render();

</script>

</body>

</html>

```



## 教学重点

- Web Audio API 生成纯音：`createOscillator()` 创建振荡器 + 频率 + 包络

- `setTimeout` 实现音序播放（每个音符间隔 400ms）

- `AudioContext` 现代替代方案，比 `audio` 标签更灵活

- 每个音符有频率(Hz)、名称、颜色三个属性，用对象封装

- 颜色与真实八音彩铃对应（辅助视觉记忆）

