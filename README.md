<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cronología Digital — Historia de Internet</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;700;800&display=swap" rel="stylesheet">
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background: #0a0a0f; }
.game-wrap {
  font-family: 'Syne', sans-serif;
  background: #0a0a0f;
  min-height: 100vh;
  padding: 1.5rem;
  color: #f0ede6;
  position: relative;
  overflow: hidden;
  max-width: 720px;
  margin: 0 auto;
}
.game-wrap::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  background: repeating-linear-gradient(0deg, transparent, transparent 39px, rgba(255,255,255,0.03) 39px, rgba(255,255,255,0.03) 40px),
              repeating-linear-gradient(90deg, transparent, transparent 39px, rgba(255,255,255,0.03) 39px, rgba(255,255,255,0.03) 40px);
  pointer-events: none;
}
.header { text-align: center; margin-bottom: 1.5rem; position: relative; }
.title-main { font-size: 22px; font-weight: 800; letter-spacing: -0.5px; color: #f0ede6; }
.title-sub { font-size: 12px; font-family: 'Space Mono', monospace; color: #6b6b80; margin-top: 4px; letter-spacing: 0.05em; }
.score-bar { display: flex; justify-content: center; gap: 24px; margin-bottom: 1.2rem; }
.stat { text-align: center; }
.stat-val { font-size: 22px; font-weight: 800; font-family: 'Space Mono', monospace; color: #c8ff57; }
.stat-label { font-size: 10px; color: #6b6b80; letter-spacing: 0.08em; text-transform: uppercase; }
.instructions { text-align: center; font-size: 12px; color: #6b6b80; margin-bottom: 1.2rem; font-family: 'Space Mono', monospace; }
.cards-area {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(145px, 1fr));
  gap: 8px;
  margin-bottom: 1.2rem;
  min-height: 80px;
}
.tech-card {
  background: #13131f;
  border: 1px solid #2a2a3d;
  border-radius: 8px;
  padding: 10px 12px;
  cursor: grab;
  user-select: none;
  transition: transform 0.15s, border-color 0.15s, box-shadow 0.15s;
  position: relative;
  overflow: hidden;
}
.tech-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0;
  width: 3px;
  height: 100%;
  background: var(--accent, #c8ff57);
  opacity: 0.6;
}
.tech-card:hover { border-color: #4a4a6a; transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0,0,0,0.4); }
.tech-card.dragging { opacity: 0.4; cursor: grabbing; transform: scale(0.97); }
.tech-card.correct { border-color: #c8ff57; background: #151f0d; }
.tech-card.correct::before { background: #c8ff57; opacity: 1; }
.tech-card.wrong { border-color: #ff5757; background: #1f0d0d; animation: shake 0.4s ease; }
.tech-card.wrong::before { background: #ff5757; opacity: 1; }
.card-emoji { font-size: 20px; margin-bottom: 4px; display: block; }
.card-name { font-size: 12px; font-weight: 700; color: #f0ede6; line-height: 1.2; }
.card-hint { font-size: 10px; color: #6b6b80; margin-top: 2px; font-family: 'Space Mono', monospace; }
.card-year { font-size: 14px; font-weight: 800; font-family: 'Space Mono', monospace; color: #c8ff57; display: none; margin-top: 4px; }
.tech-card.correct .card-year { display: block; }
.drop-zone {
  border: 1.5px dashed #2a2a3d;
  border-radius: 8px;
  padding: 10px 12px;
  min-height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: border-color 0.15s, background 0.15s;
  position: relative;
}
.drop-zone.drag-over { border-color: #c8ff57; background: rgba(200,255,87,0.05); }
.drop-zone-label { font-size: 10px; font-family: 'Space Mono', monospace; color: #3a3a5a; text-align: center; }
.timeline-area { margin-bottom: 1.2rem; }
.timeline-title { font-size: 10px; font-family: 'Space Mono', monospace; color: #6b6b80; letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 8px; }
.timeline-slots { display: grid; grid-template-columns: repeat(auto-fill, minmax(145px, 1fr)); gap: 8px; }
.slot { position: relative; }
.slot-num { position: absolute; top: -8px; left: 8px; background: #0a0a0f; font-size: 9px; font-family: 'Space Mono', monospace; color: #3a3a5a; padding: 0 4px; z-index: 1; }
.btn-row { display: flex; gap: 8px; justify-content: center; flex-wrap: wrap; }
.btn { padding: 8px 20px; border-radius: 6px; font-family: 'Syne', sans-serif; font-size: 13px; font-weight: 700; cursor: pointer; border: 1px solid; transition: all 0.15s; letter-spacing: 0.03em; }
.btn-primary { background: #c8ff57; color: #0a0a0f; border-color: #c8ff57; }
.btn-primary:hover { background: #d8ff77; }
.btn-secondary { background: transparent; color: #f0ede6; border-color: #2a2a3d; }
.btn-secondary:hover { border-color: #4a4a6a; background: #13131f; }
.feedback-msg { text-align: center; font-size: 13px; font-family: 'Space Mono', monospace; min-height: 20px; margin-bottom: 8px; color: #c8ff57; transition: opacity 0.3s; }
.feedback-msg.error { color: #ff5757; }
.result-screen { display: none; text-align: center; padding: 2rem 1rem; }
.result-screen.show { display: block; }
.result-title { font-size: 28px; font-weight: 800; margin-bottom: 8px; }
.result-sub { font-size: 13px; font-family: 'Space Mono', monospace; color: #6b6b80; margin-bottom: 1.5rem; }
.result-timeline { text-align: left; max-width: 400px; margin: 0 auto 1.5rem; }
.result-item { display: flex; align-items: center; gap: 12px; padding: 8px 0; border-bottom: 1px solid #1a1a2a; font-size: 12px; }
.result-item .yr { font-family: 'Space Mono', monospace; color: #c8ff57; font-weight: 700; min-width: 40px; }
.result-item .nm { color: #f0ede6; }
.result-item .em { font-size: 16px; }
@keyframes shake { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-6px)} 75%{transform:translateX(6px)} }

/* Touch drag support */
.tech-card { touch-action: none; }
</style>
</head>
<body>
<div class="game-wrap" id="gameWrap">
  <div id="gameScreen">
    <div class="header">
      <div class="title-main">// CRONOLOGÍA DIGITAL</div>
      <div class="title-sub">ordená los hitos del más antiguo al más reciente</div>
    </div>
    <div class="score-bar">
      <div class="stat"><div class="stat-val" id="scoreVal">0</div><div class="stat-label">puntos</div></div>
      <div class="stat"><div class="stat-val" id="correctVal">0</div><div class="stat-label">correctos</div></div>
      <div class="stat"><div class="stat-val" id="totalVal">8</div><div class="stat-label">total</div></div>
    </div>
    <div class="instructions">arrastrá cada tarjeta al lugar correcto en la línea de tiempo ↓</div>
    <div class="feedback-msg" id="feedbackMsg"></div>
    <div class="cards-area" id="cardsArea"></div>
    <div class="timeline-area">
      <div class="timeline-title">→ línea de tiempo (más antiguo primero)</div>
      <div class="timeline-slots" id="timelineSlots"></div>
    </div>
    <div class="btn-row">
      <button class="btn btn-secondary" onclick="shuffleAndReset()">↺ reiniciar</button>
      <button class="btn btn-primary" onclick="revealAll()">ver respuestas</button>
    </div>
  </div>
  <div class="result-screen" id="resultScreen">
    <div class="result-title" id="resultTitle"></div>
    <div class="result-sub" id="resultSub"></div>
    <div class="result-timeline" id="resultTimeline"></div>
    <div class="btn-row">
      <button class="btn btn-primary" onclick="shuffleAndReset()">↺ jugar de nuevo</button>
    </div>
  </div>
</div>
<script>
const HITOS = [
  { id:1, year:1969, name:"ARPANET", hint:"primera red de computadoras", emoji:"🔗", accent:"#ff9f57" },
  { id:2, year:1971, name:"Primer email", hint:"mensaje electrónico entre máquinas", emoji:"📧", accent:"#57c8ff" },
  { id:3, year:1983, name:"Protocolo TCP/IP", hint:"el lenguaje común de internet", emoji:"📡", accent:"#c857ff" },
  { id:4, year:1989, name:"World Wide Web", hint:"Tim Berners-Lee, CERN", emoji:"🌐", accent:"#ff5793" },
  { id:5, year:1993, name:"Navegador Mosaic", hint:"primera interfaz gráfica web", emoji:"🖥️", accent:"#57ffc8" },
  { id:6, year:1998, name:"Google", hint:"el buscador que lo cambió todo", emoji:"🔍", accent:"#ffdb57" },
  { id:7, year:2004, name:"Facebook", hint:"el inicio de las redes sociales masivas", emoji:"👥", accent:"#57a8ff" },
  { id:8, year:2022, name:"IA Generativa", hint:"ChatGPT y la nueva era digital", emoji:"🤖", accent:"#c8ff57" },
];

let items=[], slots=[], score=0, correct=0, draggedId=null, touchDragEl=null, touchClone=null;

function shuffle(arr){ return [...arr].sort(()=>Math.random()-0.5); }

function init(){
  items=shuffle(HITOS); slots=new Array(HITOS.length).fill(null);
  score=0; correct=0; updateStats(); renderCards(); renderSlots();
  document.getElementById('feedbackMsg').textContent='';
  document.getElementById('gameScreen').style.display='';
  document.getElementById('resultScreen').className='result-screen';
}

function updateStats(){
  document.getElementById('scoreVal').textContent=score;
  document.getElementById('correctVal').textContent=correct;
}

function renderCards(){
  const area=document.getElementById('cardsArea'); area.innerHTML='';
  items.forEach(item=>{
    const card=document.createElement('div');
    card.className='tech-card'; card.draggable=true; card.dataset.id=item.id;
    card.style.setProperty('--accent',item.accent);
    card.innerHTML=`<span class="card-emoji">${item.emoji}</span><div class="card-name">${item.name}</div><div class="card-hint">${item.hint}</div><div class="card-year">${item.year}</div>`;
    card.addEventListener('dragstart',onDragStart);
    card.addEventListener('dragend',onDragEnd);
    card.addEventListener('touchstart',onTouchStart,{passive:false});
    area.appendChild(card);
  });
}

function renderSlots(){
  const container=document.getElementById('timelineSlots'); container.innerHTML='';
  const sorted=[...HITOS].sort((a,b)=>a.year-b.year);
  sorted.forEach((orig,i)=>{
    const slot=document.createElement('div'); slot.className='slot';
    slot.innerHTML=`<div class="slot-num">#${i+1}</div>`;
    const dz=document.createElement('div'); dz.className='drop-zone';
    dz.dataset.slotIdx=i; dz.dataset.correctId=orig.id;
    if(slots[i]){
      const placed=HITOS.find(h=>h.id==slots[i]);
      const isCorrect=slots[i]==orig.id;
      dz.innerHTML=`<div class="tech-card ${isCorrect?'correct':'wrong'}" style="width:100%;--accent:${placed.accent};cursor:default;"><span class="card-emoji">${placed.emoji}</span><div class="card-name">${placed.name}</div><div class="card-hint">${placed.hint}</div>${isCorrect?`<div class="card-year">${placed.year}</div>`:''}</div>`;
    } else {
      dz.innerHTML=`<span class="drop-zone-label">soltar aquí</span>`;
    }
    dz.addEventListener('dragover',onDragOver);
    dz.addEventListener('dragleave',onDragLeave);
    dz.addEventListener('drop',onDrop);
    slot.appendChild(dz); container.appendChild(slot);
  });
}

function onDragStart(e){ draggedId=parseInt(e.currentTarget.dataset.id); setTimeout(()=>e.currentTarget.classList.add('dragging'),0); }
function onDragEnd(e){ e.currentTarget.classList.remove('dragging'); }
function onDragOver(e){ e.preventDefault(); e.currentTarget.classList.add('drag-over'); }
function onDragLeave(e){ e.currentTarget.classList.remove('drag-over'); }

function placeDrop(slotIdx){
  const sorted=[...HITOS].sort((a,b)=>a.year-b.year);
  const correctId=sorted[slotIdx].id;
  if(slots[slotIdx]!==null||!draggedId) return;
  slots[slotIdx]=draggedId;
  const isCorrect=draggedId===correctId;
  if(isCorrect){ correct++; score+=100; showFeedback('✓ ¡correcto! +100 pts',false); }
  else { score=Math.max(0,score-20); showFeedback('✗ no es ese lugar — revisá la pista',true); }
  items=items.filter(i=>i.id!==draggedId); draggedId=null;
  updateStats(); renderCards(); renderSlots();
  if(items.length===0) setTimeout(showResult,600);
}

function onDrop(e){ e.preventDefault(); e.currentTarget.classList.remove('drag-over'); placeDrop(parseInt(e.currentTarget.dataset.slotIdx)); }

function onTouchStart(e){
  e.preventDefault();
  const card=e.currentTarget;
  draggedId=parseInt(card.dataset.id);
  touchDragEl=card;
  const rect=card.getBoundingClientRect();
  touchClone=card.cloneNode(true);
  touchClone.style.cssText=`position:fixed;top:${rect.top}px;left:${rect.left}px;width:${rect.width}px;opacity:0.85;pointer-events:none;z-index:9999;`;
  document.body.appendChild(touchClone);
  card.style.opacity='0.3';
  document.addEventListener('touchmove',onTouchMove,{passive:false});
  document.addEventListener('touchend',onTouchEnd);
}
function onTouchMove(e){
  e.preventDefault();
  const t=e.touches[0];
  if(touchClone){ touchClone.style.left=(t.clientX-70)+'px'; touchClone.style.top=(t.clientY-40)+'px'; }
}
function onTouchEnd(e){
  document.removeEventListener('touchmove',onTouchMove);
  document.removeEventListener('touchend',onTouchEnd);
  if(touchClone){ document.body.removeChild(touchClone); touchClone=null; }
  if(touchDragEl){ touchDragEl.style.opacity=''; touchDragEl=null; }
  const t=e.changedTouches[0];
  const el=document.elementFromPoint(t.clientX,t.clientY);
  const dz=el ? el.closest('.drop-zone') : null;
  if(dz) placeDrop(parseInt(dz.dataset.slotIdx));
  else draggedId=null;
}

function showFeedback(msg,isError){
  const el=document.getElementById('feedbackMsg');
  el.textContent=msg; el.className='feedback-msg'+(isError?' error':'');
  setTimeout(()=>{ el.textContent=''; el.className='feedback-msg'; },2000);
}

function showResult(){
  const sorted=[...HITOS].sort((a,b)=>a.year-b.year);
  const pct=Math.round((correct/HITOS.length)*100);
  let title,sub;
  if(pct===100){title='🏆 ¡Perfecto!';sub='Dominás la historia de internet';}
  else if(pct>=75){title='🎉 ¡Muy bien!';sub=`Acertaste el ${pct}% — sabés bastante de historia digital`;}
  else if(pct>=50){title='💡 ¡Bien!';sub=`Acertaste el ${pct}% — seguí explorando`;}
  else{title='📚 Seguí aprendiendo';sub=`Acertaste el ${pct}% — ¡la historia de internet te espera!`;}
  document.getElementById('resultTitle').textContent=title;
  document.getElementById('resultSub').textContent=sub+` | ${score} puntos`;
  document.getElementById('resultTimeline').innerHTML=sorted.map(h=>`<div class="result-item"><span class="em">${h.emoji}</span><span class="yr">${h.year}</span><span class="nm">${h.name}</span></div>`).join('');
  document.getElementById('gameScreen').style.display='none';
  document.getElementById('resultScreen').className='result-screen show';
}

function shuffleAndReset(){ init(); }
function revealAll(){
  const sorted=[...HITOS].sort((a,b)=>a.year-b.year);
  sorted.forEach((h,i)=>{ slots[i]=h.id; });
  items=[]; correct=HITOS.length; score=HITOS.length*100;
  updateStats(); renderCards(); renderSlots(); setTimeout(showResult,400);
}

init();
</script>
</body>
</html>
