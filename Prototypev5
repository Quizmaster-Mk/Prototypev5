<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Avengers vs Villains V3 - Préparation</title>
<style>
body { font-family: Arial, sans-serif; background:#111; color:#eee; text-align:center; }
h1 { color:#f39c12; }
.choice { margin:20px; }
button { padding:10px 20px; margin:5px; cursor:pointer; font-size:16px; }
#summary { display:none; border:2px solid #f39c12; padding:20px; margin:20px; background:#222; }
.bar { height:20px; background:#333; border-radius:10px; margin:10px auto; width:300px; position:relative; overflow:hidden; }
.fill { height:100%; border-radius:10px; transition: width 0.5s ease; }
#log { border:1px solid #555; padding:10px; height:200px; overflow-y:auto; background:#222; text-align:left; margin-top:20px; }
.avatar { font-size:50px; display:inline-block; position:relative; transition: transform 0.3s ease; }
.floating-dmg { position:absolute; animation: floatUp 1s ease-out forwards; font-weight:bold; }
@keyframes floatUp { from { opacity:1; transform:translateY(0); } to { opacity:0; transform:translateY(-50px); } }
.crit { font-size:24px; color:yellow; font-weight:bold; }
.shield { box-shadow:0 0 20px 5px #3498db; border-radius:50%; }
.hit { animation: hitAnim 0.3s; }
@keyframes hitAnim { 0% { transform: scale(1); } 50% { transform: scale(1.3); } 100% { transform: scale(1); } }
#narrative { margin:20px; font-style:italic; color:#f1c40f; }
</style>
</head>
<body>
<h1>🦸 Avengers vs Villains V3 - Préparation</h1>

<!-- Écran de sélection -->
<div id="setup">
  <div class="choice">
    <h2>Choisis ton héros 1</h2>
    <button onclick="selectHero(0,'Iron Man','🤖')">🤖 Iron Man</button>
    <button onclick="selectHero(0,'Thor','⚡')">⚡ Thor</button>
  </div>
  <div class="choice">
    <h2>Choisis ton héros 2</h2>
    <button onclick="selectHero(1,'Captain','🛡️')">🛡️ Captain</button>
    <button onclick="selectHero(1,'Hulk','🟢')">🟢 Hulk</button>
  </div>
  <div class="choice">
    <h2>Choisis la mission</h2>
    <button onclick="setMission('Thanos')">🟣 Empêcher Thanos</button>
    <button onclick="setMission('Chitauri')">👽 Sauver New York</button>
  </div>
  <div class="choice">
    <h2>Choisis la difficulté</h2>
    <button onclick="setDifficulty('Facile')">Facile</button>
    <button onclick="setDifficulty('Normal')">Normal</button>
    <button onclick="setDifficulty('Difficile')">Difficile</button>
    <button onclick="setDifficulty('Cauchemar')">Cauchemar</button>
  </div>
  <div class="choice">
    <button onclick="showSummary()">📜 Voir résumé</button>
  </div>
</div>

<!-- Écran résumé -->
<div id="summary">
  <h2>Résumé de l’équipe</h2>
  <div id="summaryContent"></div>
  <button onclick="startMission()">🚀 Commencer la mission</button>
</div>

<!-- Écran combat -->
<div id="battle" style="display:none;">
  <div id="narrative"></div>
  <div id="heroes"></div>
  <hr>
  <div id="enemies"></div>
  <hr>
  <div id="actions"></div>
  <button id="continueBtn" style="display:none;" onclick="continueBattle()">Continuer</button>
  <div id="log"></div>
</div>

<script>
let heroes=[], enemies=[], currentHero=0, currentEnemy=0;
let difficulty="Normal", mission="Thanos", comboUsed=false;

function selectHero(index,name,emoji){
  heroes[index]={name:name, emoji:emoji, pv:30, max:30, usedPower:false, shieldAvailable:true, shieldActive:false};
}

function setMission(m){ mission=m; }
function setDifficulty(level){ difficulty=level; }

function showSummary(){
  if(heroes.length<2 || !mission){ alert("⚠️ Choisis d'abord 2 héros et une mission !"); return; }
  document.getElementById("setup").style.display="none";
  document.getElementById("summary").style.display="block";

  let html="";
  heroes.forEach(h=>{
    html+=`
      <p>${h.emoji} <b>${h.name}</b> – PV : ${h.max}<br>
      ⚡ Rapide : 3-5 dégâts<br>
      💥 Lourde : 5-9 dégâts<br>
      🌟 Pouvoir : 8-12 dégâts (usage unique)<br>
      🛡️ Bouclier : disponible<br>
      </p>
    `;
  });
  html+=`<hr><p><b>Mission :</b> ${mission}<br><b>Difficulté :</b> ${difficulty}</p>`;
  document.getElementById("summaryContent").innerHTML=html;
}

function applyDifficulty(enemy){
  if(difficulty==="Facile"){ enemy.pv=Math.floor(enemy.pv*0.8); enemy.dmg=[Math.max(1,enemy.dmg[0]-1), Math.max(2,enemy.dmg[1]-2)]; }
  if(difficulty==="Difficile"){ enemy.pv=Math.floor(enemy.pv*1.2); enemy.dmg=[enemy.dmg[0]+1, enemy.dmg[1]+2]; }
  if(difficulty==="Cauchemar"){ enemy.pv=Math.floor(enemy.pv*1.5); enemy.dmg=[enemy.dmg[0]+3, enemy.dmg[1]+5]; }
}

function startMission(){
  document.getElementById("summary").style.display="none";
  document.getElementById("battle").style.display="block";

  if(mission==="Thanos"){ enemies=[{name:"Thanos", emoji:"🟣", pv:50, max:50, dmg:[4,8]}, {name:"Thanos 2", emoji:"🟣", pv:40, max:40, dmg:[3,7]}]; }
  if(mission==="Chitauri"){ enemies=[{name:"Chitauri 1", emoji:"👽", pv:40, max:40, dmg:[3,6]}, {name:"Chitauri 2", emoji:"👽", pv:35, max:35, dmg:[2,5]}]; }

  enemies.forEach(applyDifficulty);
  narrativePrompt();
}

function rnd(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }

function render(){
  const heroesDiv=document.getElementById("heroes"); heroesDiv.innerHTML="";
  heroes.forEach((h,i)=>{ heroesDiv.innerHTML+=`<div class="avatar" id="hero${i}">${h.emoji}</div> ${h.name} PV:${h.pv}/${h.max}<br>`; });
  const enemiesDiv=document.getElementById("enemies"); enemiesDiv.innerHTML="";
  enemies.forEach((e,i)=>{ enemiesDiv.innerHTML+=`<div class="avatar" id="enemy${i}">${e.emoji}</div> ${e.name} PV:${e.pv}/${e.max}<br>`; });
}

function pushLog(msg){ const log=document.getElementById("log"); log.innerHTML+="<div>"+msg+"</div>"; log.scrollTop=log.scrollHeight; }

function showFloatingDmg(target,dmg,color,crit=false){
  const bar=document.getElementById(target);
  const span=document.createElement("span");
  span.className="floating-dmg";
  span.textContent=(dmg>0? "-"+dmg:dmg);
  if(crit) span.className="floating-dmg crit";
  span.style.color=color;
  span.style.left=(bar.offsetLeft+50)+"px";
  span.style.top=(bar.offsetTop-20)+"px";
  document.body.appendChild(span);
  setTimeout(()=>span.remove(),1000);
  bar.classList.add("hit");
  setTimeout(()=>{bar.classList.remove("hit");},300);
}

function narrativePrompt(){
  const options=[
    "Foncez frontalement !",
    "Protégez les civils d'abord !",
    "Attaquez par surprise depuis le flanc !"
  ];
  document.getElementById("narrative").innerHTML="Choisissez votre stratégie :<br>"+options.map((o,i)=>`<button onclick="startRound(${i})">${o}</button>`).join("");
}

function startRound(choice){
  document.getElementById("narrative").innerHTML="";
  pushLog("Choix stratégique : "+(choice===0?"Foncez frontalement":choice===1?"Protégez civils":"Attaque flanc"));
  if(choice===1){ heroes.forEach(h=>{h.pv=Math.min(h.max,h.pv+5)}); pushLog("Les héros regagnent 5 PV grâce à la protection des civils."); render(); }
  currentHero=0;
  currentEnemy=0;
  comboUsed=false;
  playerTurn();
}

function playerTurn(){
  if(currentHero>=heroes.length){ currentHero=0; return enemyTurn(); }
  const h=heroes[currentHero];
  if(h.pv<=0){ currentHero++; return playerTurn(); }
  document.getElementById("actions").innerHTML=`
    <button onclick="playerAttack('rapide')">⚡ Rapide (3-5)</button>
    <button onclick="playerAttack('lourde')">💥 Lourde (5-9)</button>
    <button onclick="playerAttack('pouvoir')" ${h.usedPower?"disabled":""}>🌟 Pouvoir (8-12, unique)</button>
    <button onclick="playerShield()" ${!h.shieldAvailable?"disabled":""}>🛡️ Bouclier</button>
  `;
}

function playerAttack(type){
  const h=heroes[currentHero];
  const e=enemies[currentEnemy];
  let dmg=0, crit=false;
  if(type==="rapide") dmg=rnd(3,5);
  if(type==="lourde") dmg=rnd(5,9);
  if(type==="pouvoir"){ dmg=rnd(8,12); h.usedPower=true; }
  if(Math.random()<0.2){ crit=true; dmg*=2; }

  if(!comboUsed && currentHero>0 && heroes[currentHero-1].pv>0 && currentEnemy===0){ dmg*=2; pushLog("💥 Combo entre héros activé !"); comboUsed=true; }

  animateAttack("hero"+currentHero,"enemy"+currentEnemy);

  e.pv=Math.max(0,e.pv-dmg);
  showFloatingDmg("enemy"+currentEnemy,dmg,crit?"yellow":"red",crit);
  pushLog(h.name+" attaque "+e.name+" pour "+dmg+" PV"+(crit?" (crit)":""));
  render();

  if(e.pv<=0){ 
    pushLog(e.name+" est vaincu !"); 
    currentEnemy++; 
    if(currentEnemy>=enemies.length){ pushLog("🎉 Mission terminée !"); endGame(); return; } 
  }

  document.getElementById("actions").innerHTML="";
  document.getElementById("continueBtn").style.display="block";
}

function playerShield(){
  const h=heroes[currentHero];
  if(!h.shieldAvailable){ return; }
  h.shieldActive=true; h.shieldAvailable=false;
  pushLog(h.name+" active son bouclier !");
  document.getElementById("actions").innerHTML="";
  document.getElementById("continueBtn").style.display="block";
}

function continueBattle(){ document.getElementById("continueBtn").style.display="none"; currentHero++; playerTurn(); }

function enemyTurn(){
  if(currentEnemy>=enemies.length){ currentEnemy=0; return playerTurn(); }
  const e=enemies[currentEnemy];
  const h=heroes[currentHero];
  if(h.pv<=0){ pushLog(h.name+" est déjà KO !"); currentHero++; if(currentHero>=heroes.length){ currentHero=0; } return playerTurn(); }
  let dmg=rnd(e.dmg[0],e.dmg[1]);
  if(h.shieldActive){ pushLog(h.name+" bloque l'attaque !"); h.shieldActive=false; dmg=0; }
  animateAttack("enemy"+currentEnemy,"hero"+currentHero);
  h.pv=Math.max(0,h.pv-dmg);
  showFloatingDmg("hero"+currentHero,dmg,"red");
  pushLog(e.name+" attaque "+h.name+" pour "+dmg+" PV");
  render();
  if(h.pv<=0){ pushLog(h.name+" est vaincu !"); }
  currentHero++; 
  if(currentHero>=heroes.length){ currentHero=0; }
  playerTurn();
}

function animateAttack(attackerId,targetId){
  const attacker=document.getElementById(attackerId);
  const target=document.getElementById(targetId);
  if(!attacker || !target) return;
  const startPos=attacker.getBoundingClientRect().left;
  const endPos=target.getBoundingClientRect().left;
  attacker.style.transform=`translateX(${endPos-startPos}px)`;
  setTimeout(()=>{attacker.style.transform=`translateX(0)`;},300);
}

function endGame(){ document.getElementById("actions").innerHTML=""; document.getElementById("continueBtn").style.display="none"; pushLog("🏁 Fin du combat."); }
</script>
</body>
</html>
