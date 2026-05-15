[dice.html](https://github.com/user-attachments/files/27805273/dice.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Roll Color Dice</title>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background:
    linear-gradient(rgba(0,0,0,0.35), rgba(0,0,0,0.35)),
    url('https://images.unsplash.com/photo-1500375592092-40eb2168fd21?q=80&w=1800&auto=format&fit=crop');
  background-size: cover;
  background-position: center;
  overflow: hidden;
  color: white;
}

.background-icons {
  position: fixed;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
  z-index: 0;
}

.float-img {
  position: absolute;
  width: 90px;
  height: 90px;
  opacity: 0.35;
  border-radius: 12px;
  animation: floatIcon linear infinite;
  object-fit: cover;
  z-index: 0;
}

@keyframes floatIcon {
  from { transform: translateY(110vh) rotate(0deg); }
  to { transform: translateY(-120vh) rotate(360deg); }
}

body::before {
  content: "";
  position: fixed;
  inset: 0;
  opacity: 0.08;
  background-image:
    linear-gradient(45deg, white 2px, transparent 2px),
    linear-gradient(-45deg, white 2px, transparent 2px);
  background-size: 40px 40px;
  z-index: 0;
}

.topbar {
  width: 98%;
  margin: 16px auto;
  background: rgba(255,255,255,0.2);
  padding: 14px;
  border-radius: 8px;
  text-align: center;
  font-size: 1.2rem;
  position: relative;
  z-index: 2;
}

.container {
  position: relative;
  z-index: 3;
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1 {
  font-size: 4rem;
  margin-bottom: 30px;
  text-shadow: 3px 3px rgba(0,0,0,0.2);
}

.controls {
  display: flex;
  gap: 14px;
  margin-bottom: 40px;
}

select {
  padding: 18px;
  width: 190px;
  border-radius: 4px;
  border: 4px solid #2c7fff;
  font-size: 1.2rem;
  font-weight: bold;
}

.dice-wrapper {
  display: flex;
  gap: 18px;
  transition: transform 0.6s ease;
  position: relative;
  z-index: 5;
}

.dice-row {
  display: flex;
  gap: 18px;
}

.dice {
  width: 100px;
  height: 100px;
  border-radius: 18px;
  border: 10px solid white;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: 0.2s;
  z-index: 6;
}

.dice:hover { transform: scale(1.05); }

.dot {
  width: 22px;
  height: 22px;
  background: white;
  border-radius: 50%;
}

.red { background: red; }
.orange { background: orange; }
.yellow { background: #ffcc00; }
.green { background: limegreen; }
.blue { background: blue; }
.purple { background: purple; }

.result {
  margin-top: 30px;
  font-size: 2rem;
  font-weight: bold;
}

.rig-panel {
  position: fixed;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255,255,255,0.18);
  padding: 18px;
  border-radius: 16px;
  width: 170px;
  z-index: 3;
}

.rig-title {
  text-align: center;
  font-size: 1.7rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.rig-option {
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 10px;
  text-align: center;
  font-weight: bold;
  cursor: pointer;
  border: 3px solid white;
  transition: 0.2s;
}

button {
  margin-top: 25px;
  padding: 16px 40px;
  border: none;
  border-radius: 12px;
  background: white;
  color: #0070ff;
  font-size: 1.2rem;
  font-weight: bold;
  cursor: pointer;
}

button:hover { transform: scale(1.05); }

.spark {
  position: fixed;
  width: 8px;
  height: 8px;
  background: yellow;
  border-radius: 50%;
  pointer-events: none;
  animation: sparkFly 1s linear forwards;
  z-index: 999;
}

@keyframes sparkFly {
  0% { transform: translate(0,0) scale(1); opacity: 1; }
  100% { transform: translate(var(--x), var(--y)) scale(0); opacity: 0; }
}
</style>
</head>
<body>

<div class="background-icons" id="backgroundIcons"></div>

<div class="topbar">
Possible colors are: Red, Orange, Yellow, Green, Blue and Purple.
</div>

<div class="container">
  <h1>ROLL COLOR DICE</h1>

  <div class="controls">
    <select><option>4 DICE</option></select>
    <select><option>COLOR DICE</option></select>
    <select><option>NO BETS</option></select>
  </div>

  <div class="dice-wrapper" id="diceWrapper">
    <div class="dice-row" id="diceRow">
      <div class="dice purple"><div class="dot"></div></div>
      <div class="dice red"><div class="dot"></div></div>
      <div class="dice blue"><div class="dot"></div></div>
      <div class="dice orange"><div class="dot"></div></div>
    </div>
  </div>

  <div class="result" id="result">Press roll to generate colors!</div>

  <button id="rollButton">ROLL</button>
</div>

<div class="rig-panel">
  <div class="rig-title">BLOCK COLORS</div>
  <div class="rig-option red" onclick="setBlock('red')">RED</div>
  <div class="rig-option orange" onclick="setBlock('orange')">ORANGE</div>
  <div class="rig-option yellow" onclick="setBlock('yellow')">YELLOW</div>
  <div class="rig-option green" onclick="setBlock('green')">GREEN</div>
  <div class="rig-option blue" onclick="setBlock('blue')">BLUE</div>
  <div class="rig-option purple" onclick="setBlock('purple')">PURPLE</div>
</div>

<script>
const colors = ['purple','red','blue','orange','yellow','green'];
const colorNames = {
  purple:'Purple', red:'Red', blue:'Blue', orange:'Orange', yellow:'Yellow', green:'Green'
};

let blockedColors = [];

function createDice(color){
  return `<div class=\"dice ${color}\"><div class=\"dot\"></div></div>`;
}

function setBlock(color){
  const btn = document.querySelector('.rig-option.' + color);

  if(blockedColors.includes(color)){
    blockedColors = blockedColors.filter(c => c !== color);
    btn.style.opacity='1';
    btn.style.filter='brightness(1)';
    btn.style.border='3px solid white';
  } else {
    blockedColors.push(color);
    btn.style.opacity='0.35';
    btn.style.filter='brightness(0.3)';
    btn.style.border='3px solid black';
  }
}

function createSpark(x,y){
  const s=document.createElement('div');
  s.className='spark';
  s.style.left=x+'px';
  s.style.top=y+'px';
  const angle=Math.random()*Math.PI*2;
  const dist=50+Math.random()*120;
  s.style.setProperty('--x', Math.cos(angle)*dist+'px');
  s.style.setProperty('--y', Math.sin(angle)*dist+'px');
  document.body.appendChild(s);
  setTimeout(()=>s.remove(),1000);
}

function burstSparks(){
  const rect=document.getElementById('diceRow').getBoundingClientRect();
  for(let i=0;i<30;i++) createSpark(rect.left+rect.width/2, rect.top+rect.height/2);
}

function rollDice(){
  const diceWrapper=document.getElementById('diceWrapper');
  const diceRow=document.getElementById('diceRow');
  const result=document.getElementById('result');

  diceRow.innerHTML='';

  const available=colors.filter(c=>!blockedColors.includes(c));

  if(!available.length){
    result.textContent='You blocked all colors!';
    return;
  }

  // instant roll (no delay)
  diceWrapper.style.transform='rotate(360deg)';
  burstSparks();

  let rolled=[];

  for(let i=0;i<4;i++){
    const c=available[Math.floor(Math.random()*available.length)];
    rolled.push(colorNames[c]);
    diceRow.innerHTML+=createDice(c);
  }

  result.textContent='Rolled: '+rolled.join(', ');
}

document.getElementById('rollButton').addEventListener('click', rollDice);

const bg=document.getElementById('backgroundIcons');
const images=[
  'https://images.unsplash.com/photo-1500375592092-40eb2168fd21?q=80&w=1800&auto=format&fit=crop',
  'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?q=80&w=1800&auto=format&fit=crop',
  'https://images.unsplash.com/photo-1501004318641-b39e6451bec6?q=80&w=1800&auto=format&fit=crop'
];

for(let i=0;i<35;i++){
  const img=document.createElement('img');
  img.className='float-img';
  img.src=images[Math.floor(Math.random()*images.length)];

  img.style.left=Math.random()*100+'%';
  img.style.animationDuration=10+Math.random()*20+'s';
  img.style.animationDelay=Math.random()*5+'s';
  img.style.width=50+Math.random()*60+'px';
  img.style.height=50+Math.random()*60+'px';

  bg.appendChild(img);
}

rollDice();
</script>

</body>
</html>
