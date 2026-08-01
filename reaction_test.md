# 反应速度测试



## 说明

屏幕变绿时尽快点击，测试你的反应时间（毫秒）。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>反应测试</title>

<style>

*{margin:0;padding:0;box-sizing:border-box}

body{display:flex;align-items:center;justify-content:center;height:100vh;font-family:Arial;cursor:pointer;user-select:none}

#screen{width:100%;height:100%;display:flex;align-items:center;justify-content:center;font-size:36px;color:white;text-align:center;transition:background 0.1s}

.red{background:#e74c3c}

.green{background:#2ecc71}

.wait{background:#3498db}

#hist{margin:15px;font-size:16px;opacity:0.8}

</style></head>

<body>

<div id="screen" class="red" onclick="handleClick()">点击开始</div>



<script>

let state="idle",timeout,startTime,results=[];

const el=document.getElementById("screen");



function handleClick(){

  if(state==="idle"){startTest();return;}

  if(state==="waiting"){el.className="red";el.innerHTML="太快了!<br>等绿色再点<br><span id='hist'>最好:"+(results.length?Math.min(...results)+"ms":"-")+"</span>";clearTimeout(timeout);state="idle";return;}

  if(state==="ready"){let rt=Date.now()-startTime;results.push(rt);el.className="green";el.innerHTML="反应时间:"+rt+"ms<br>点击再来<br><span id='hist'>最好:"+Math.min(...results)+"ms</span>";state="idle";}

}

function startTest(){el.className="wait";el.innerHTML="等待绿色...<br>";state="waiting";let delay=1000+Math.random()*3000;timeout=setTimeout(()=>{el.className="green";el.innerHTML="点击!";startTime=Date.now();state="ready";},delay);}

</script></body></html>

```

