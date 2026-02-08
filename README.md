<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Un recuerdo para ti ✨</title>

<style>
* { margin:0; padding:0; box-sizing:border-box; }

body {
  height: 100vh;
  background: radial-gradient(circle at top, #1a2b5c, #040816);
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: 'Segoe UI', sans-serif;
  color: #fff;
  perspective: 1200px;
}

/* CIELO */
.sky {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

/* ESTRELLAS */
.star {
  position: absolute;
  width: 2px;
  height: 2px;
  background: white;
  border-radius: 50%;
  opacity: .8;
  animation: twinkle 4s infinite alternate;
}

@keyframes twinkle {
  from { opacity: .2; }
  to { opacity: 1; }
}

/* COMETA */
.comet {
  position: absolute;
  width: 2px;
  height: 140px;
  background: linear-gradient(to top, rgba(255,255,255,.9), transparent);
  transform: rotate(35deg);
  animation: comet 10s linear infinite;
  opacity: .7;
}

@keyframes comet {
  0% { transform: translate(-30vw, -40vh) rotate(35deg); }
  100% { transform: translate(130vw, 140vh) rotate(35deg); }
}

/* TARJETA */
.card {
  position: relative;
  background: linear-gradient(135deg,
    rgba(255,255,255,.18),
    rgba(255,255,255,.04));
  backdrop-filter: blur(16px);
  border-radius: 26px;
  padding: 30px 22px;
  width: min(92vw, 380px);
  text-align: center;
  box-shadow: 0 50px 120px rgba(0,0,0,.7);
  transform-style: preserve-3d;
  animation: float 7s ease-in-out infinite;
}

@keyframes float {
  0%,100% { transform: translateZ(140px) rotateX(6deg) rotateY(-6deg); }
  50% { transform: translateZ(180px) rotateX(8deg) rotateY(-8deg); }
}

/* TEXTO */
.line {
  font-size: clamp(18px, 5vw, 24px);
  line-height: 1.7;
  margin-bottom: 16px;
  min-height: 1.7em;
  text-shadow: 0 0 12px rgba(255,255,255,.4);
}

.glow {
  color: #a9d7ff;
  text-shadow:
    0 0 15px rgba(169,215,255,.8),
    0 0 30px rgba(169,215,255,.6);
  font-weight: 600;
}

.signature {
  font-size: 14px;
  opacity: .75;
  margin-top: 10px;
}

.tap {
  position: absolute;
  bottom: 12px;
  font-size: 12px;
  opacity: .6;
}
</style>
</head>

<body>

<div class="sky" id="sky"></div>

<div class="card" id="card">
  <div class="line" id="t1"></div>
  <div class="line glow" id="t2"></div>
  <div class="line glow" id="t3"></div>
  <div class="signature" id="t4"></div>
</div>

<div class="tap">Toca la pantalla ✨</div>

<script>
// ESTRELLAS
const sky = document.getElementById("sky");
for (let i = 0; i < 80; i++) {
  const s = document.createElement("div");
  s.className = "star";
  s.style.left = Math.random()*100 + "vw";
  s.style.top = Math.random()*100 + "vh";
  s.style.animationDelay = Math.random()*5 + "s";
  sky.appendChild(s);
}

// COMETA
const comet = document.createElement("div");
comet.className = "comet";
sky.appendChild(comet);

// TEXTO ANIME
const lines = [
  { id:"t1", text:"Espero que te guste este hermoso detalle…" },
  { id:"t2", text:"Tu pincesa está orgullosa" },
  { id:"t3", text:"de lo que estás logrando 👑" },
  { id:"t4", text:"Este recuerdo siempre vivirá en nosotros ✨" }
];

function type(el, text, speed = 65) {
  return new Promise(res => {
    let i = 0;
    const interval = setInterval(() => {
      el.textContent += text.charAt(i);
      i++;
      if (i >= text.length) {
        clearInterval(interval);
        setTimeout(res, 700);
      }
    }, speed);
  });
}

(async () => {
  for (const l of lines) {
    await type(document.getElementById(l.id), l.text);
  }
})();

// EFECTO 3D
const card = document.getElementById("card");
function tilt(x,y) {
  const rx = (y - innerHeight/2) / 40;
  const ry = (x - innerWidth/2) / -40;
  card.style.transform =
    `translateZ(160px) rotateX(${rx}deg) rotateY(${ry}deg)`;
}
addEventListener("mousemove", e => tilt(e.clientX,e.clientY));
addEventListener("touchmove", e => {
  const t = e.touches[0];
  tilt(t.clientX,t.clientY);
});
addEventListener("touchend", () => {
  card.style.transform =
    "translateZ(140px) rotateX(6deg) rotateY(-6deg)";
});
</script>

</body>
</html>
