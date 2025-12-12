<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<title>Past Continuous vs Past Perfect</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<style>
:root{
  --brand:#64d404;
  --bg:#f2fbf6;
  --card:#ffffff;
  --text:#1f2d1f;
  --muted:#5f6f5f;
  --border:#d9ead3;
  --ok:#2e9e44;
  --bad:#d9534f;
}

*{box-sizing:border-box}

body{
  margin:0;
  font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
  background:linear-gradient(135deg,#eafff2,#eef7ff);
  color:var(--text);
  padding:20px;
}

.app{
  max-width:900px;
  margin:auto;
  background:var(--card);
  border-radius:20px;
  border:1px solid var(--border);
  overflow:hidden;
}

header{
  padding:20px;
  background:linear-gradient(90deg,var(--brand),#a5ef7c);
}

.slide{display:none;padding:24px}
.slide.active{display:block}

.card{
  background:#fff;
  border-radius:16px;
  padding:20px;
  border:1px solid var(--border);
}

.box{
  border:1px solid var(--border);
  border-radius:12px;
  padding:14px;
  margin-bottom:14px;
}

.example{
  background:#f1fbee;
  border-left:4px solid var(--brand);
  padding:10px;
  border-radius:8px;
  margin-top:8px;
}

button{
  border-radius:10px;
  border:1px solid var(--border);
  background:#f6fff2;
  padding:8px 14px;
  cursor:pointer;
  font-weight:600;
}

button.primary{
  background:var(--brand);
  color:#143000;
  border:none;
}

.nav{
  display:flex;
  justify-content:space-between;
  padding:14px;
  border-top:1px solid var(--border);
}

input[type=text], textarea{
  padding:6px 8px;
  border-radius:8px;
  border:1px solid var(--border);
  font-size:15px;
}

.opt{
  padding:10px;
  border:1px solid var(--border);
  border-radius:10px;
  margin:6px 0;
  cursor:pointer;
  background:#f8fff6;
}

.correct{background:#c9f2c9}
.wrong{background:#ffd4d4}

.small{color:var(--muted); font-size:14px}
</style>
</head>

<body>
<div class="app">

<header>
  <h1>Past Continuous vs Past Perfect</h1>
  <div class="small">Spiegazione + esercizi progressivi</div>
</header>

<!-- SLIDE 1 -->
<section class="slide active">
<div class="card">
<h2>1️⃣ Spiegazione in italiano</h2>

<div class="box">
<p><b>Past Continuous</b> = azione in corso in un momento preciso del passato.</p>
<p><b>Forma:</b> was / were + verbo + ING</p>
<div class="example">At 8 pm I <b>was studying</b>.</div>
<p class="small">👉 “stavo studiando”</p>
</div>

<div class="box">
<p><b>Past Perfect</b> = azione successa prima di un’altra azione passata.</p>
<p><b>Forma:</b> had + past participle</p>
<div class="example">When I arrived, they <b>had already left</b>.</div>
<p class="small">👉 “avevano già fatto qualcosa”</p>
</div>
</div>
</section>

<!-- SLIDE 6 -->
<section class="slide">
<div class="card">
<h2>6️⃣ Scrivi nei gap</h2>
<p class="small">Suggerimento: usa il verbo tra parentesi, scegliendo il tempo corretto.</p>

<p>I <input type="text" data-a="was working"> (work) when the phone rang.</p>
<p>She was tired because she <input type="text" data-a="had studied"> (study) all day.</p>
<p>By the time we arrived, the meeting <input type="text" data-a="had finished"> (finish).</p>

<button class="primary" onclick="checkInputs()">Check</button>
</div>
</section>

<!-- SLIDE 7 -->
<section class="slide">
<div class="card">
<h2>7️⃣ Listening (hard)</h2>
<p class="small">Ascolta attentamente: le frasi sono molto simili.</p>

<button class="primary" onclick="playListening()">🔊 Play</button>
<div id="listen"></div>
</div>
</section>

<!-- SLIDE 8 -->
<section class="slide">
<div class="card">
<h2>8️⃣ Speaking guidato</h2>

<p class="small">
Completa la frase e dilla ad alta voce.<br>
Usa un’azione reale.
</p>

<div class="example">
At 9 pm yesterday, I <b>was ________</b>.
</div>

<button onclick="speakExample()">🔊 Listen example</button>
<button class="primary" onclick="startSpeaking()">🎙️ Speak</button>

<p class="small">Trascrizione:</p>
<div id="spokenText" class="example">—</div>
</div>
</section>

<!-- SLIDE 9 -->
<section class="slide">
<div class="card">
<h2>9️⃣ Pronuncia</h2>

<div class="example" id="pronSentence">
I was reading when the lights went out.
</div>

<button class="primary" onclick="playPron()">🔊 Listen</button>
</div>
</section>

<div class="nav">
<button onclick="prev()">⬅ Prev</button>
<button onclick="next()">Next ➡</button>
</div>

</div>

<script>
let i=0;
const slides=document.querySelectorAll('.slide');
function show(){slides.forEach((s,n)=>s.classList.toggle('active',n===i))}
function next(){if(i<slides.length-1){i++;show()}}
function prev(){if(i>0){i--;show()}}

function checkInputs(){
document.querySelectorAll('input[data-a]').forEach(i=>{
i.style.background=i.value.trim().toLowerCase()===i.dataset.a?'#c9f2c9':'#ffd4d4'
})
}

/* NATIVE VOICE */
function getVoice(){
const voices=speechSynthesis.getVoices();
return voices.find(v=>v.lang.startsWith('en-GB')) ||
       voices.find(v=>v.lang.startsWith('en-US')) ||
       voices[0];
}

/* Listening */
const listens=[
"I was working when you called.",
"I had worked when you called.",
"I worked when you called.",
"I had been working when you called."
];
let correct=0;

function playListening(){
correct=Math.floor(Math.random()*listens.length);
const u=new SpeechSynthesisUtterance(listens[correct]);
u.voice=getVoice();
speechSynthesis.speak(u);

const d=document.getElementById('listen');
d.innerHTML='';
listens.forEach((s,n)=>{
const o=document.createElement('div');
o.className='opt';
o.innerText=s;
o.onclick=()=>o.classList.add(n===correct?'correct':'wrong');
d.appendChild(o);
});
}

/* Speaking */
const rec=window.SpeechRecognition||window.webkitSpeechRecognition;
let recognition=rec?new rec():null;
if(recognition){
recognition.lang='en-GB';
recognition.onresult=e=>{
document.getElementById('spokenText').innerText=e.results[0][0].transcript;
};
}

function startSpeaking(){
if(recognition) recognition.start();
}

function speakExample(){
const u=new SpeechSynthesisUtterance("At 9 pm yesterday, I was watching TV.");
u.voice=getVoice();
speechSynthesis.speak(u);
}

/* Pronunciation */
function playPron(){
const t=document.getElementById('pronSentence').innerText;
const u=new SpeechSynthesisUtterance(t);
u.voice=getVoice();
speechSynthesis.speak(u);
}
</script>

</body>
</html>
