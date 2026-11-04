<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#000000">
<title>Calculator</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;margin:0;background:#000;font-family:Arial,sans-serif}
.app{height:100%;max-width:480px;margin:0 auto;display:flex;flex-direction:column;padding:12px}
#display{flex:1;color:#fff;font-size:72px;font-weight:300;display:flex;align-items:flex-end;justify-content:flex-end;word-break:break-all;padding:12px}
.pad{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;padding-bottom:16px}
button{height:72px;border:0;border-radius:36px;font-size:30px;color:#fff;background:#333}
button:active{opacity:.6}
.f{background:#a5a5a5;color:#000}
.o{background:#ff9f0a}
.z{grid-column:span 2}
</style>
</head>
<body>
<div class="app">
  <div id="display">0</div>
  <div class="pad">
    <button class="f">C</button><button class="f">±</button><button class="f">%</button><button class="o">÷</button>
    <button>7</button><button>8</button><button>9</button><button class="o">×</button>
    <button>4</button><button>5</button><button>6</button><button class="o">−</button>
    <button>1</button><button>2</button><button>3</button><button class="o">+</button>
    <button class="z">0</button><button>.</button><button class="o">=</button>
  </div>
</div>
<script>
let d='0', prev=null, op=null, fresh=false;
const el=document.getElementById('display');

const calc=(a,b,o)=> o=='+'?a+b : o=='−'?a-b : o=='×'?a*b : o=='÷'?(b==0?NaN:a/b) : b;
const fmt=n=> isNaN(n)?'Error':String(parseFloat(n.toPrecision(12)));

function press(k){
  if(/^\d$/.test(k)){
    if(fresh||d==='0'||d==='Error'){d=k;fresh=false}else d+=k;
  } else if(k==='.'){
    if(fresh||d==='Error'){d='0.';fresh=false}
    else if(!d.includes('.')) d+='.';
  } else if('÷×−+'.includes(k)){
    if(op&&!fresh){const r=calc(prev,parseFloat(d),op);d=fmt(r);prev=r}
    else prev=parseFloat(d);
    op=k;fresh=true;
  } else if(k==='='){
    if(op){const r=calc(prev,parseFloat(d),op);d=fmt(r);prev=null;op=null;fresh=true}
  } else if(k==='C'){
    d='0';prev=null;op=null;fresh=false;
  } else if(k==='±'){
    if(d!=='0'&&d!=='Error') d=d[0]==='-'?d.slice(1):'-'+d;
  } else if(k==='%'){
    if(d!=='Error') d=fmt(parseFloat(d)/100);
  }
  el.textContent=d;
  el.style.fontSize = d.length>12?'36px' : d.length>8?'48px' : '72px';
}

document.querySelectorAll('button').forEach(b=>{
  b.addEventListener('click',()=>press(b.textContent));
});
</script>
</body>
</html>
