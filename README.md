# index.html
<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Feliz cumpleaños Amochiss</title>
<style>
  :root{
    --bg:#100722;
    --card:#fdf6ff;
    --lilas1:#c19ad6;
    --lilas2:#b89be0;
  }
  html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,"Helvetica Neue",Arial;}
  body{
    background: radial-gradient(circle at 10% 10%, rgba(180,160,230,0.12), transparent 10%),
                linear-gradient(180deg, #0b0220 0%, #12022b 100%);
    display:flex;align-items:center;justify-content:center;overflow:hidden;color:#1b0b25;
  }

  /* Card */
  .card{
    width:360px; max-width:90%;
    background: linear-gradient(180deg, rgba(255,255,255,0.98), rgba(255,250,255,0.95));
    border-radius:16px; padding:24px;
    box-shadow: 0 12px 40px rgba(10,5,20,0.6), inset 0 1px 0 rgba(255,255,255,0.6);
    transform-origin:center top;
    transition:transform .5s cubic-bezier(.2,.9,.2,1), height .5s;
    cursor:pointer;
    position:relative;
  }
  .card .main{
    text-align:center;
  }
  .title{
    font-size:1.6rem; font-weight:700; color:#24032a;
    text-shadow: 0 2px 8px rgba(178,110,220,0.25);
  }
  .subtitle{ margin-top:8px; color:#6b3c73; font-weight:600;}

  /* closed vs open */
  .card.closed{ transform: scale(1); }
  .card.open{ transform: scale(1.03); }

  /* extra content */
  .extra{
    margin-top:18px; opacity:0; max-height:0; overflow:hidden;
    transition:opacity .6s ease, max-height .6s ease;
  }
  .card.open .extra{ opacity:1; max-height:800px; }

  .extra p{ color:#3a0b33; line-height:1.4; }

  /* balloons */
  .balloons{ position:absolute; inset: -80px 0 auto 0; height:200px; pointer-events:none; }
  .balloon{
    width:48px; height:68px; border-radius:50% 50% 50% 50%;
    position:absolute; bottom:30px; opacity:0; transform: translateY(0) scale(.8);
    animation: floatUp 6s infinite ease-in-out;
    box-shadow: inset -6px -10px 18px rgba(255,255,255,0.2);
  }
  .balloon.show{ opacity:1; }
  .balloon::after{ content:""; position:absolute; left:50%; top:90%; width:2px; height:36px; background:rgba(80,30,80,0.6); transform:translateX(-50%); }

  @keyframes floatUp {
    0%{ transform: translateY(0) rotate(-6deg); }
    50%{ transform: translateY(-40px) rotate(6deg); }
    100%{ transform: translateY(0) rotate(-6deg); }
  }

  /* candle */
  .candle-wrap{ display:flex; align-items:center; gap:12px; justify-content:center; margin-top:14px; }
  .candle{ width:18px; height:56px; background:linear-gradient(#fff,#ffe8f0); border-radius:6px; position:relative; box-shadow: 0 4px 10px rgba(0,0,0,0.2);}
  .wick{ position:absolute; left:50%; top:-8px; width:4px; height:8px; background:#222; transform:translateX(-50%); border-radius:2px;}
  .flame{
    position:absolute; left:50%; top:-26px; width:16px; height:24px; transform:translateX(-50%) rotate(10deg);
    background: radial-gradient(circle at 40% 30%, #fff3c2 10%, #ffd25b 30%, #ff8a6b 60%, transparent 70%);
    filter:drop-shadow(0 4px 10px rgba(255,170,120,0.18));
    border-radius:50% 50% 40% 40%;
    animation: flicker .18s infinite;
  }
  @keyframes flicker {
    0%{ transform:translateX(-50%) scaleY(1) rotate(10deg); opacity:1; }
    50%{ transform:translateX(-50%) scaleY(0.92) rotate(8deg); opacity:.95; }
    100%{ transform:translateX(-50%) scaleY(1) rotate(12deg); opacity:1; }
  }
  .flame.extinguished{ opacity:0; transition:opacity .6s ease; }

  .sopla-btn{
    background:linear-gradient(180deg,#6d3aac,#4e2a7a); color:white; border:none; padding:8px 12px; border-radius:8px; cursor:pointer;
    box-shadow: 0 6px 18px rgba(90,40,140,0.25);
  }

  .go-btn{
    display:inline-block;margin-top:18px;background:linear-gradient(90deg,var(--lilas1),var(--lilas2));color:white;padding:10px 16px;border-radius:12px;text-decoration:none;font-weight:600;
    box-shadow: 0 12px 30px rgba(180,120,220,0.18);
  }

  /* smoke */
  .smoke{
    position:absolute; left:50%; top:-40px; transform:translateX(-50%);
    width:8px;height:8px;border-radius:50%; background:rgba(120,90,140,0.18); opacity:0; pointer-events:none;
  }
  .smoke.rise{ animation: smokeRise 2s forwards; }
  @keyframes smokeRise{
    0%{ transform:translate(-50%,0) scale(0.6); opacity:1; }
    100%{ transform:translate(-50%,-60px) scale(2); opacity:0; }
  }

  /* small note */
  .hint{ font-size:0.82rem;color:#6b3c73;margin-top:8px;text-align:center;}
</style>
</head>
<body>
  <!-- Música de fondo: reemplaza assets/music.mp3 con tu archivo -->
  <audio id="music" src="./assets/music.mp3" preload="auto" loop></audio>

  <div class="card closed" id="card">
    <div class="main">
      <div class="title">Feliz cumpleaños Amochiss</div>
      <div class="subtitle">Haz clic para abrir tu sorpresa</div>
    </div>

    <div class="extra" id="extra">
      <p>Que este día esté lleno de magia, risas y dulces deseos. 🎂✨</p>
      <div class="candle-wrap">
        <div style="position:relative;">
          <div class="candle" id="candle">
            <div class="wick"></div>
            <div class="flame" id="flame"></div>
            <div class="smoke" id="smoke"></div>
          </div>
        </div>
        <button class="sopla-btn" id="sopla">Sopla</button>
      </div>

      <p style="margin-top:12px;">Mira más abajo para leer un mensaje especial.</p>

      <div style="text-align:center;">
        <a class="go-btn" id="goSecret" href="#" style="display:none;">Ir a la sorpresa</a>
      </div>
    </div>

    <!-- globos lilas -->
    <div class="balloons" id="balloons" aria-hidden="true">
      <div class="balloon" style="left:6%; background:linear-gradient(180deg,var(--lilas1),var(--lilas2)); animation-delay:0s;"></div>
      <div class="balloon" style="left:22%; background:linear-gradient(180deg,#d6b3e6,#cfa7e8); animation-delay:0.6s; width:42px;height:60px;"></div>
      <div class="balloon" style="left:48%; background:linear-gradient(180deg,#cdb7e0,#c49be6); animation-delay:1.2s;"></div>
      <div class="balloon" style="left:72%; background:linear-gradient(180deg,#e0c6f2,#cfb1e8); animation-delay:0.9s; width:44px;height:66px;"></div>
    </div>

    <div class="hint">Si el audio no suena, da clic de nuevo o pulsa play en el reproductor del navegador.</div>
  </div>

<script>
  const card = document.getElementById('card');
  const extra = document.getElementById('extra');
  const music = document.getElementById('music');
  const balloons = document.getElementById('balloons');
  const flame = document.getElementById('flame');
  const sopla = document.getElementById('sopla');
  const smoke = document.getElementById('smoke');
  const goSecret = document.getElementById('goSecret');

  let opened = false;
  let blown = false;

  // Click en la tarjeta abre (o no) la carta
  card.addEventListener('click', function(e){
    // evitar que click en botones internos vuelva a disparar la apertura/despliegue
    if (e.target === sopla || e.target === goSecret) return;
    if (!opened){
      openCard();
    } else {
      // opcional: al volver a dar click la mantiene abierta (no cerrar)
    }
  });

  function openCard(){
    opened = true;
    card.classList.remove('closed');
    card.classList.add('open');
    // mostrar globos (aparecen y flotan)
    Array.from(balloons.children).forEach((b,i)=>{
      setTimeout(()=> b.classList.add('show'), i*200);
    });

    // intentar reproducir música (la interacción del click habilita reproducción en la mayoría de navegadores)
    music.play().catch(err=>{
      console.log('Reproducción automática bloqueada:', err);
    });

    // mostrar extra text está controlado por CSS; aseguramos que botón aparezca tras secuencia
    setTimeout(()=> {
      // nada aquí, el extra ya aparece con CSS
    }, 600);
  }

  // Soplar la vela
  sopla.addEventListener('click', function(ev){
    ev.stopPropagation();
    if (blown) return;
    blown = true;
    flame.classList.add('extinguished');
    // crear humo: animar varios puntitos
    for(let i=0;i<3;i++){
      const s = document.createElement('div');
      s.className = 'smoke';
      s.style.left = (50 + (i-1)*8) + '%';
      card.querySelector('.candle').appendChild(s);
      // trigger animation
      requestAnimationFrame(()=> s.classList.add('rise'));
      setTimeout(()=> s.remove(), 2200);
    }

    // tras apagar, mostrar el botón para ir a la página secreta
    setTimeout(()=> {
      goSecret.style.display = 'inline-block';
    }, 900);
  });

  // ir a la página secreta (se abre en la misma pestaña)
  goSecret.addEventListener('click', function(ev){
    ev.preventDefault();
    window.location.href = './secret.html';
  });

  // Prevent accidental page unload while music
  // (opcional; coméntalo si no quieres aviso)
  window.addEventListener('beforeunload', (e) => {
    if (music && !music.paused) {
      e.preventDefault();
      e.returnValue = '';
    }
  });
</script>
</body>
</html>