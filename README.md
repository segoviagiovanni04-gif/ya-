<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Te Amo Lilibeth</title>
<style>
  html,body{
    margin:0;height:100%;
    background:#000;
    overflow:hidden;
    font-family:monospace;
  }
  canvas{display:block}
  .center-msg{
    position:absolute;
    left:50%; top:50%;
    transform:translate(-50%,-50%);
    text-align:center;
    pointer-events:none;
  }
  .center-msg h1{
    margin:0;
    font-size:clamp(32px,7vw,72px);
    color:#c08cff;   /* lila */
    text-shadow:0 0 14px #a96cff;
  }
  .center-msg p{
    margin-top:10px;
    font-size:clamp(20px,4vw,32px);
    color:#c08cff;   /* lila */
    text-shadow:0 0 10px #a96cff;
  }
</style>
</head>
<body>
<canvas id="c"></canvas>

<div class="center-msg">
  <h1>TE AMO DEMASIADO</h1>
  <p>Lilibeth</p>
</div>

<script>
(() => {
  const canvas = document.getElementById('c');
  const ctx = canvas.getContext('2d');

  let w, h, fontSize;
  function resize(){
    w = canvas.width = innerWidth;
    h = canvas.height = innerHeight;
    fontSize = Math.max(12, Math.floor(w / 60));
    ctx.font = fontSize + 'px monospace';
    columns = Math.floor(w / fontSize);
    drops = new Array(columns).fill(0).map(()=>Math.floor(Math.random()*h));
  }
  addEventListener('resize', resize);

  const chars = ['T','E',' ','A','M','O','❤','♥'];
  let columns, drops;
  resize();

  // --- corazones interactivos ---
  const floatingHearts = []; // cada corazón será {x,y,alpha}
  canvas.addEventListener('click', e => addHeart(e));
  canvas.addEventListener('touchstart', e => {
    for (const touch of e.touches) addHeart(touch);
  });
  function addHeart(pos){
    const rect = canvas.getBoundingClientRect();
    floatingHearts.push({
      x: pos.clientX - rect.left,
      y: pos.clientY - rect.top,
      alpha: 1
    });
  }
  // -------------------------------

  function draw(){
    ctx.fillStyle = 'rgba(0,0,0,0.15)';
    ctx.fillRect(0,0,w,h);

    // lluvia verde
    ctx.fillStyle = '#00ff6a';
    for(let i=0;i<columns;i++){
      const x = i * fontSize;
      const y = drops[i] * fontSize;
      const ch = chars[Math.floor(Math.random()*chars.length)];
      ctx.fillText(ch, x, y);
      if(y > h + Math.random()*1000){
        drops[i] = 0;
      } else {
        drops[i]++;
      }
    }

    // corazones verdes flotantes
    ctx.font = '32px serif';
    for(let i=floatingHearts.length-1;i>=0;i--){
      const heart = floatingHearts[i];
      ctx.fillStyle = rgba(0,255,80,${heart.alpha}); // verde fuerte
      ctx.fillText('❤', heart.x, heart.y);
      heart.y -= 1.5;       // sube
      heart.alpha -= 0.01;  // se desvanece
      if(heart.alpha <= 0) floatingHearts.splice(i,1);
    }
  }

  function loop(){
    draw();
    requestAnimationFrame(loop);
  }
  loop();
})();
</script>
</body>
</html>
