<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cambio Titocambios</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
}

body{
background:linear-gradient(180deg,#000,#111,#000);
font-family:Arial,Helvetica,sans-serif;
color:white;
text-align:center;
overflow:hidden;
touch-action:none;
}

#panel{
padding:15px;
background:rgba(0,0,0,.7);
border-bottom:2px solid gold;
}

h1{
color:gold;
font-size:28px;
margin-bottom:10px;
text-shadow:0 0 10px gold;
}

#score{
font-size:24px;
margin:10px;
}

button{
padding:12px 20px;
border:none;
border-radius:10px;
background:gold;
color:#000;
font-weight:bold;
font-size:16px;
cursor:pointer;
margin:5px;
box-shadow:0 0 10px gold;
}

button:hover{
transform:scale(1.05);
}

#rates{
margin-top:10px;
font-size:18px;
color:#0f0;
}

canvas{
display:block;
margin:auto;
background:#000;
border:2px solid gold;
box-shadow:0 0 25px gold;
}

#popup{
position:fixed;
top:0;
left:0;
right:0;
bottom:0;
background:rgba(0,0,0,.85);
display:none;
justify-content:center;
align-items:center;
z-index:999;
}

#popupContent{
background:#111;
padding:30px;
border:2px solid gold;
border-radius:20px;
box-shadow:0 0 20px gold;
width:90%;
max-width:400px;
}

input{
padding:12px;
width:90%;
margin:10px;
border:none;
border-radius:10px;
font-size:18px;
text-align:center;
}

.small{
font-size:14px;
color:#aaa;
margin-top:10px;
}
</style>
</head>

<body>

<div id="panel">
<h1>💰 CAMBIO TITO GAME PRO 💰</h1>

<div id="score">
<span id="playerScore">0</span> -
<span id="cpuScore">0</span>
</div>

<button onclick="openCalc()">Calcular Cambio</button>
<button onclick="contact()">WhatsApp</button>

<div id="rates">
USD: $4.000 COP | EUR: $4.350 COP
</div>
</div>

<canvas id="game" width="800" height="500"></canvas>

<div id="popup">
<div id="popupContent">
<h2 style="color:gold;">Calculadora Cambio Tito</h2>

<input id="usd" type="number" placeholder="USD a cambiar">

<button onclick="calc()">Calcular</button>

<h3 id="result"></h3>

<button onclick="sendWhats()">Enviar por WhatsApp</button>

<div class="small">Atención inmediata ⚡</div>

<button onclick="closeCalc()">Cerrar</button>
</div>
</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const playerScoreText = document.getElementById("playerScore");
const cpuScoreText = document.getElementById("cpuScore");

const paddleW = 12;
const paddleH = 100;
const ballR = 8;

let playerY = 200;
let cpuY = 200;

let ballX = 400;
let ballY = 250;

let speedX = 5;
let speedY = 5;

let playerScore = 0;
let cpuScore = 0;

let usdRate = 4000;

canvas.addEventListener("touchmove",(e)=>{
const rect = canvas.getBoundingClientRect();
playerY = e.touches[0].clientY - rect.top - paddleH/2;
});

canvas.addEventListener("mousemove",(e)=>{
const rect = canvas.getBoundingClientRect();
playerY = e.clientY - rect.top - paddleH/2;
});

function drawRect(x,y,w,h,color="#fff"){
ctx.fillStyle=color;
ctx.fillRect(x,y,w,h);
}

function drawBall(){
ctx.beginPath();
ctx.arc(ballX,ballY,ballR,0,Math.PI*2);
ctx.fillStyle="cyan";
ctx.shadowBlur=20;
ctx.shadowColor="cyan";
ctx.fill();
ctx.shadowBlur=0;
}

function resetBall(){
ballX = canvas.width/2;
ballY = canvas.height/2;
speedX *= -1;
speedY = (Math.random()*6)-3;
}

function update(){

cpuY += (ballY - (cpuY+paddleH/2))*0.08;

ballX += speedX;
ballY += speedY;

if(ballY-ballR<0 || ballY+ballR>canvas.height){
speedY *= -1;
}

if(
ballX-ballR < paddleW &&
ballY > playerY &&
ballY < playerY+paddleH
){
speedX *= -1.08;
}

if(
ballX+ballR > canvas.width-paddleW &&
ballY > cpuY &&
ballY < cpuY+paddleH
){
speedX *= -1.08;
}

if(ballX<0){
cpuScore++;
cpuScoreText.textContent = cpuScore;
resetBall();
}

if(ballX>canvas.width){
playerScore++;
playerScoreText.textContent = playerScore;
resetBall();

if(playerScore==3){
setTimeout(()=>{
openCalc();
},500);
}
}
}

function render(){
ctx.fillStyle="#000";
ctx.fillRect(0,0,canvas.width,canvas.height);

drawRect(0,playerY,paddleW,paddleH,"gold");
drawRect(canvas.width-paddleW,cpuY,paddleW,paddleH,"gold");

drawBall();
}

function gameLoop(){
update();
render();
requestAnimationFrame(gameLoop);
}

gameLoop();

function openCalc(){
document.getElementById("popup").style.display="flex";
}

function closeCalc(){
document.getElementById("popup").style.display="none";
}

function calc(){
let usd = Number(document.getElementById("usd").value);
let total = usd * usdRate;

document.getElementById("result").innerHTML =
usd + " USD = $" + total.toLocaleString("es-CO") + " COP";
}

function sendWhats(){
let usd = document.getElementById("usd").value;
let msg = "Hola Cambio Tito, deseo cambiar " + usd + " USD.";
window.location.href =
"https://wa.me/573144162593?text="+encodeURIComponent(msg);
}

function contact(){
window.location.href="https://wa.me/573144162593";
}
</script>

</body>
</html>
