<canvas id="matrix"></canvas>

<div id="passwordScreen">
  <h1>Enter Password:</h1>
  <div id="passwordInput"></div>
</div>

<div id="terminal"></div>

<div id="secondScreen">
  <h1>WELCOME COMMANDER</h1>
</div>
body {
  margin: 0;
  overflow: hidden;
  background: black;
  font-family: monospace;
}

canvas {
  position: fixed;
  top: 0;
  left: 0;
}

#passwordScreen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #0a0a0a;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #00ffcc;
  font-size: 40px;
  z-index: 3;
}

#passwordInput {
  margin-top: 20px;
  font-size: 36px;
  text-shadow: 0 0 15px #00ffcc;
}

#terminal {
  position: relative;
  color: #00ff00;
  padding: 20px;
  font-size: 16px;
  z-index: 2;
  white-space: pre-line;
  display: none;
}

#secondScreen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #0a0a0a;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  transition: opacity 1.5s ease;
  z-index: 5;
}

#secondScreen h1 {
  color: #00ffcc;
  font-size: 60px;
  letter-spacing: 5px;
  text-align: center;
  text-shadow: 0 0 15px #00ffcc;
}
// MATRIX BACKGROUND
const canvas = document.getElementById("matrix");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
const fontSize = 14;
const columns = canvas.width / fontSize;
const drops = [];

for (let x = 0; x < columns; x++) {
  drops[x] = 1;
}

function drawMatrix() {
  ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.fillStyle = "#00ff00";
  ctx.font = fontSize + "px monospace";

  for (let i = 0; i < drops.length; i++) {
    const text = letters.charAt(Math.floor(Math.random() * letters.length));
    ctx.fillText(text, i * fontSize, drops[i] * fontSize);

    if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
      drops[i] = 0;
    }

    drops[i]++;
  }
}

let matrixInterval = setInterval(drawMatrix, 33);

// PASSWORD SCREEN
const passwordScreen = document.getElementById("passwordScreen");
const passwordInput = document.getElementById("passwordInput");
const fakePassword = "UltraSecure123";
let pwIndex = 0;

function typePassword() {
  if (pwIndex < fakePassword.length) {
    passwordInput.innerText += "*";
    pwIndex++;
    setTimeout(typePassword, 200);
  } else {
    setTimeout(startTerminal, 500);
  }
}

typePassword();

// TERMINAL EFFECT
const terminal = document.getElementById("terminal");
const messages = [
  "Initializing system...",
  "Accessing secure database...",
  "Bypassing firewall...",
  "Decrypting encrypted files...",
  "Access granted."
];

let i = 0;

function startTerminal() {
  passwordScreen.style.display = "none";
  terminal.style.display = "block";
  typeLine();
}

function typeLine() {
  if (i < messages.length) {
    let line = messages[i];
    let j = 0;

    let interval = setInterval(() => {
      terminal.innerHTML += line[j];
      j++;
      if (j === line.length) {
        clearInterval(interval);
        terminal.innerHTML += "\n";
        i++;
        setTimeout(typeLine, 500);
      }
    }, 50);
  } else {
    setTimeout(showSecondScreen, 1000);
  }
}

// SECOND SCREEN
function showSecondScreen() {
  clearInterval(matrixInterval);
  canvas.style.display = "none";
  terminal.style.display = "none";

  const secondScreen = document.getElementById("secondScreen");
  secondScreen.style.opacity = "1";
}
