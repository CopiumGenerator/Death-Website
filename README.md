<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Death Web</title>
<link href="https://fonts.googleapis.com/css2?family=Creepster&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{
  height:100vh;
  background:#000;
  font-family:'Creepster', cursive;
  overflow:hidden;
  color:#ff1a1a;
}

/* Background smoke flicker */
body::before{
  content:"";
  position:absolute;
  inset:0;
  background:url('https://i.imgur.com/4Rpj8yL.png') center/cover no-repeat;
  opacity:0.15;
  animation:slowSmoke 60s linear infinite;
}
@keyframes slowSmoke{0%{transform:translateY(0);}100%{transform:translateY(-100px);}}

/* Flickering red overlay */
body::after{
  content:"";
  position:absolute;
  inset:0;
  background:rgba(50,0,0,0.25);
  animation:bloodFlicker 8s infinite ease-in-out;
}
@keyframes bloodFlicker{
  0%,100%{opacity:0.24;}
  50%{opacity:0.32;}
}

/* Blood drips (animated) */
.bloodDrip{
  position:absolute;
  top:-20px;
  width:4px;
  height:20px;
  background:red;
  animation:drip 2s linear infinite;
  opacity:0.8;
  border-radius:50%;
}
@keyframes drip{
  0%{transform:translateY(0) scaleY(1);}
  50%{transform:translateY(50vh) scaleY(1.5);}
  100%{transform:translateY(100vh) scaleY(1);}
}

/* Intro overlay */
.fullscreen{
  position:absolute;
  inset:0;
  display:flex;
  justify-content:center;
  align-items:center;
  flex-direction:column;
  background:#000;
  transition:opacity 1.5s ease;
  z-index:1000;
  text-align:center;
  overflow:hidden;
}
.fullscreen.hidden{opacity:0; pointer-events:none;}
.fullscreen h1{
  font-family:'Creepster', cursive;
  font-size:96px;
  color:red;
  text-shadow:2px 2px 2px #000,-2px -2px 2px #000,0 0 15px darkred;
  margin-bottom:40px;
  animation:glowPulse 2.5s infinite alternate;
}
.fullscreen p{
  font-family:'Creepster', cursive;
  font-size:28px;
  color:#ff5555;
  text-shadow:0 0 6px #000;
  margin:8px 0;
  line-height:1.5;
}
.fullscreen p strong{
  font-size:32px;color:#ff0000;text-shadow:0 0 12px #000;
}
@keyframes glowPulse{
  0%{text-shadow:2px 2px 2px #000,-2px -2px 2px #000,0 0 12px darkred;}
  50%{text-shadow:4px 4px 8px #000,-4px -4px 8px #000,0 0 24px red;}
  100%{text-shadow:2px 2px 2px #000,-2px -2px 2px #000,0 0 12px darkred;}
}

/* Main panel */
#mainPanel{display:none;position:relative;width:100%;height:100%;flex-direction:column;justify-content:center;align-items:center;}
.timer{
  position:absolute;top:40px;text-align:center;color:#ff1a1a;
  font-size:48px;letter-spacing:4px;text-shadow:0 0 20px rgba(255,26,26,0.9);
}
.timer span{display:block;font-size:18px;color:#888;margin-top:6px;letter-spacing:2px;}
.panel{
  width:500px;padding:40px;text-align:center;
  background:rgba(0,0,0,0.65);
  border:1px solid rgba(255,0,0,0.3);
  backdrop-filter:blur(6px);
  box-shadow:0 0 140px rgba(255,0,0,0.7);
  z-index:2;
  transition:transform 0.5s ease, box-shadow 0.5s ease;
}
.panel:hover{
  transform:scale(1.02);
  box-shadow:0 0 180px rgba(255,0,0,0.9);
}
/* Carved horror heading */
.panel h1{
  font-family:'Creepster', cursive;
  font-size:64px;
  letter-spacing:4px;
  margin-bottom:28px;
  color:red;
  text-shadow:2px 2px 0 #000,-2px -2px 0 #000,2px -2px 0 #000,-2px 2px 0 #000,0 0 12px darkred;
  position:relative;
  text-align:center;
}
.panel h1::after{
  content:''; position:absolute; width:3px; height:20px; background:red;
  left:30%; bottom:-10px; animation:dripAnim 1s infinite linear;
}
.panel h1::before{
  content:''; position:absolute; width:3px; height:15px; background:red;
  left:70%; bottom:-12px; animation:dripAnim 1.3s infinite linear;
}
/* Inputs and buttons */
input{
  width:100%;padding:18px;
  background:rgba(0,0,0,.85);
  border:1px solid rgba(255,255,255,.25);
  color:#fff;font-size:20px;text-align:center;
}
input::placeholder{color:#777;}
button{
  margin-top:18px;width:100%;padding:14px;
  background:transparent;border:1px solid rgba(255,255,255,.3);
  color:#fff;letter-spacing:2px;cursor:pointer;font-size:20px;
}
button:hover{background:#ff0000;color:#000;transition:0.2s;}
/* Result text with drips */
.result{
  font-family:'Creepster', cursive;
  font-size:32px;
  color:red;
  text-shadow:1px 1px 0 #000,-1px -1px 0 #000,1px -1px 0 #000,-1px 1px 0 #000,0 0 6px darkred;
  white-space: pre-line;
  min-height:120px;
  position:relative;
}
.result::after{content:''; position:absolute; width:3px; height:20px; background:red; left:20%; bottom:-5px; animation:dripAnim 1.2s infinite linear;}
.result::before{content:''; position:absolute; width:2px; height:15px; background:red; left:70%; bottom:-8px; animation:dripAnim 1.5s infinite linear;}
.rules{display:none;font-size:16px;color:#aaa;line-height:1.6;margin-top:18px;}
.fade{
  position:absolute; inset:0; background:black;
  opacity:0; pointer-events:none; transition:opacity 2.5s ease; z-index:10;
}
.doneText{
  position:absolute; inset:0; display:flex; justify-content:center; align-items:center;
  font-family:'Creepster', cursive; font-size:48px;color:red;
  opacity:0; pointer-events:none; z-index:15; text-shadow:0 0 12px #000;
  transition:opacity 1s ease;
}
.killEffect{
  position:absolute; inset:0; background:red; mix-blend-mode:screen; opacity:0; pointer-events:none; z-index:20;
}
.glitchEffect{
  position:absolute; inset:0;
  background:repeating-linear-gradient(rgba(255,255,255,0.05) 0 2px, transparent 2px 4px);
  opacity:0; pointer-events:none; z-index:25;
  animation:glitchAnim 0.15s infinite;
}
@keyframes glitchAnim{
  0%{transform:translate(0,0);}
  25%{transform:translate(-5px,3px);}
  50%{transform:translate(5px,-2px);}
  75%{transform:translate(-3px,2px);}
  100%{transform:translate(0,0);}
}
@keyframes dripAnim{0%{height:10px;}50%{height:25px;}100%{height:10px;}
}

</style>
</head>
<body>
<!-- Scary music -->
<audio id="ambientSound" loop autoplay>
  <source src="https://cdn.pixabay.com/download/audio/2022/02/15/audio_d869b3e1cb.mp3" type="audio/mpeg">
</audio>
<audio id="glitchSound">
  <source src="https://cdn.pixabay.com/download/audio/2021/09/26/audio_3d8bdf23e1.mp3" type="audio/mpeg">
</audio>

<!-- Intro -->
<div class="fullscreen" id="intro">
  <h1>⚠ WARNING ⚠</h1>
  <p>You are about to access forbidden records.</p>
  <p>All entries are permanent.</p>
  <p>Proceed at your own risk.</p>
  <p>Press <strong>ENTER</strong></p>
</div>

<!-- Main Panel -->
<div id="mainPanel" style="display:flex;">
  <div class="timer">
    <div id="countdown">72:00:00</div>
    <span>TIME REMAINING</span>
  </div>
  <div class="panel">
    <h1>DEATH WEB</h1>
    <input id="nameInput" placeholder="Enter a name">
    <button onclick="record()">SUBMIT</button>
    <div class="result" id="result"></div>
    <div class="rules" id="rules">
      <p>• Every name leaves a trace.</p>
      <p>• Patterns are remembered forever.</p>
      <p>• The more entries you make...</p>
      <p>...the clearer the consequences become.</p>
    </div>
  </div>
  <div class="fade" id="fade"></div>
  <div class="doneText" id="doneText">DONE</div>
  <div class="killEffect" id="killEffect"></div>
  <div class="glitchEffect" id="glitchEffect"></div>
</div>

<script>
const audio=document.getElementById('ambientSound');
audio.volume=0.6;
function playMusic(){audio.play().catch(()=>{});document.removeEventListener('keydown',playMusic);document.removeEventListener('click',playMusic);}
document.addEventListener('keydown',playMusic);
document.addEventListener('click',playMusic);

document.addEventListener('keydown', e=>{
  if(e.key==='Enter'){
    document.getElementById('intro').classList.add('hidden');
    setTimeout(()=>{
      document.getElementById('intro').style.display='none';
      document.getElementById('mainPanel').style.display='flex';
      createBloodDrips(15); // create blood drips on main panel
    },1500);
  }
});

let totalSeconds=72*60*60;
setInterval(()=>{
  if(totalSeconds<=0) return;
  totalSeconds--;
  let h=String(Math.floor(totalSeconds/3600)).padStart(2,'0');
  let m=String(Math.floor((totalSeconds%3600)/60)).padStart(2,'0');
  let s=String(totalSeconds%60).padStart(2,'0');
  countdown.textContent=`${h}:${m}:${s}`;
},1000);

function typeText(el,text,speed=35){
  el.innerHTML="";
  let i=0;
  const t=setInterval(()=>{el.innerHTML+=text.charAt(i++);if(i>=text.length) clearInterval(t);},speed);
}

const traits=[
  "has a lingering dark impression.",
  "seems to carry the echoes of broken trust.",
  "draws turmoil like iron to a magnet.",
  "leaves shadows where light should be.",
  "harbors silent disquiet beneath calm words.",
  "patterns suggest motives unspoken.",
  "shows restlessness in quiet moments.",
  "weaves hesitation into every choice.",
  "fails to recognize peace as a refuge.",
  "balances intent with hidden unrest."
];

function record(){
  const name=nameInput.value.trim();
  if(!name) return;
  const resEl=document.getElementById("result");
  const rules=document.getElementById("rules");
  const fade=document.getElementById("fade");
  const doneText=document.getElementById("doneText");
  const kill=document.getElementById("killEffect");
  const glitch=document.getElementById("glitchEffect");
  const glitchAudio=document.getElementById("glitchSound");

  let seed=0;for(let c of name) seed+=c.charCodeAt(0);
  const amount=(seed%450)+50;
  const trait=traits[seed%traits.length];

  let message=`"${name}" has been recorded.\nBalance: ₱${amount}\n\nObservation: ${trait}`;
  if(name.toLowerCase()==="audrey"||name.toLowerCase()==="death") message=`"${name}" was already known.\nNo balance required.\n\nObservation: Incomplete.`;

  typeText(resEl,message);
  rules.style.display="block";

  setTimeout(()=>{
    fade.style.opacity=1;
    doneText.style.opacity=1;
    kill.style.opacity=1;
    kill.style.animation="killGlitch 10s ease-in-out";
    glitch.style.opacity=1;
    glitchAudio.currentTime=0;
    glitchAudio.play();
    setTimeout(()=>{
      resEl.innerHTML="";
      rules.style.display="none";
      fade.style.opacity=0;
      doneText.style.opacity=0;
      kill.style.opacity=0;
      glitch.style.opacity=0;
      nameInput.value="";
      const intro=document.getElementById("intro");
      intro.style.display="flex";
      intro.classList.remove("hidden");
      document.getElementById("mainPanel").style.display="none";
      clearBloodDrips();
    },10000);
  },7000);
}

/* Blood drips generator */
function createBloodDrips(count){
  const container = document.body;
  for(let i=0;i<count;i++){
    const drip = document.createElement('div');
    drip.className='bloodDrip';
    drip.style.left=Math.random()*100+'vw';
    drip.style.animationDuration=(1.5+Math.random()*2)+'s';
    drip.style.height=(10+Math.random()*40)+'px';
    container.appendChild(drip);
  }
}

function clearBloodDrips(){
  document.querySelectorAll('.bloodDrip').forEach(el=>el.remove());
}

</script>
</body>
</html>
