


<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Appeal Portal</title>

<style>
/* GLOBAL */
* { box-sizing: border-box; }
body {
  margin: 0;
  font-family: "Inter", sans-serif;
  background: #020617;
  color: white;
  overflow: hidden;
  display: flex;
}

/* LEFT COMMAND BOX */
.command-box {
  width: 60px;
  height: 100vh;
  background: #1e3a8a; /* blue */
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding-top: 10px;
}

.command-input {
  width: 90%;
  padding: 6px;
  border: none;
  border-radius: 6px;
  background: rgba(255,255,255,0.2);
  color: white;
  outline: none;
  font-size: 12px;
}

/* MAIN WRAPPER */
.wrapper {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* CARD */
.card {
  width: 450px;
  background: rgba(15,23,42,0.7);
  border: 1px solid rgba(255,255,255,0.1);
  backdrop-filter: blur(14px);
  padding: 28px;
  border-radius: 18px;
  box-shadow: 0 0 40px rgba(0,0,0,0.5);
  animation: fadeIn .4s ease-out;
}

h2 {
  margin: 0 0 10px 0;
  text-align: center;
  font-weight: 700;
}

/* INPUTS */
input, textarea, select, button {
  width: 100%;
  margin-top: 12px;
  padding: 12px;
  border-radius: 10px;
  border: none;
  background: rgba(255,255,255,0.07);
  color: white;
  font-size: 15px;
  outline: none;
  transition: .25s;
}

input:focus, textarea:focus, select:focus {
  background: rgba(255,255,255,0.12);
  box-shadow: 0 0 0 2px #3b82f6;
}

textarea { height: 90px; resize: none; }

/* SELECT */
select {
  appearance: none;
  background-image: url("data:image/svg+xml;utf8,<svg fill='white' width='20' height='20' viewBox='0 0 20 20'><path d='M5 7l5 5 5-5'/></svg>");
  background-repeat: no-repeat;
  background-position: right 12px center;
  padding-right: 40px;
}

/* BUTTON */
button {
  background: #3b82f6;
  font-weight: 600;
  cursor: pointer;
}

button:hover {
  background: #60a5fa;
  transform: translateY(-2px);
}

/* MESSAGES */
.error, .info {
  text-align: center;
  font-size: 14px;
  margin-top: 8px;
  min-height: 18px;
}

.error { color: #f87171; }
.info { color: #38bdf8; }

/* HIDDEN */
.hidden { display: none; }

/* CONFETTI */
#confetti {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 30;
}

/* ANIMATIONS */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
</head>
<body>

<!-- LEFT COMMAND BOX -->
<div class="command-box">
  <input class="command-input" id="cmd" placeholder="cmd">
</div>

<!-- MAIN CONTENT -->
<div class="wrapper">

  <!-- STEP 1 -->
  <div class="card" id="step1">
    <h2>Enter Discord ID</h2>
    <input id="discordID" placeholder="123456789012345678">
    <div id="cooldownMsg" class="info hidden"></div>
    <div id="errorMsg" class="error hidden"></div>
    <button onclick="validateID()">Continue</button>
  </div>

  <!-- STEP 2 -->
  <div class="card hidden" id="step2">
    <h2>Appeal Form</h2>

    <select id="appealType">
      <option>Mute</option>
      <option>Kick</option>
      <option>Ban</option>
      <option>Global ban</option>
      <option>Blacklist</option>
      <option>Warning</option>
    </select>

    <textarea id="reason" placeholder="Why were you punished?"></textarea>
    <textarea id="responsibility" placeholder="Do you take responsibility?"></textarea>
    <textarea id="future" placeholder="What will you do differently?"></textarea>

    <button onclick="submitAppeal()">Submit Appeal</button>
  </div>

  <!-- LOADING -->
  <div class="card hidden" id="loading">
    <h2 id="loadingTitle">Submitting...</h2>
    <p id="loadingMsg" class="info"></p>
  </div>

</div>

<canvas id="confetti"></canvas>

<script>
/* CONFIG */
const webhook = "https://discord.com/api/webhooks/1487243550855795001/JNsSVMq38winuBfM4MjOhOLyTWzxvMSDTBNSvvUJB9MqlXn242zG6reZaSbqVC6d41oT";
const COOLDOWN_HOURS = 2;

/* ELEMENTS */
const step1 = document.getElementById("step1");
const step2 = document.getElementById("step2");
const loading = document.getElementById("loading");
const cooldownMsg = document.getElementById("cooldownMsg");
const errorMsg = document.getElementById("errorMsg");
const loadingTitle = document.getElementById("loadingTitle");
const loadingMsg = document.getElementById("loadingMsg");

/* COMMAND BOX */
document.getElementById("cmd").addEventListener("keydown", (e) => {
  if (e.key === "Enter") {
    const cmd = e.target.value.trim();
    e.target.value = "";
    runCommand(cmd);
  }
});

/* COMMAND HANDLER */
function runCommand(cmd) {
  if (cmd.startsWith("!cool ")) {
    const id = cmd.split(" ")[1];
    localStorage.removeItem("cooldown_" + id);
    alert("Cooldown removed for " + id);
  }
}

/* CHECK COOLDOWN */
function checkCooldown(id) {
  const key = "cooldown_" + id;
  const last = localStorage.getItem(key);
  if (!last) return false;

  const diff = Date.now() - Number(last);
  const hours = diff / (1000 * 60 * 60);

  if (hours < COOLDOWN_HOURS) {
    const mins = Math.ceil((COOLDOWN_HOURS - hours) * 60);
    cooldownMsg.textContent = `Cooldown active: ${mins} minutes remaining.`;
    cooldownMsg.classList.remove("hidden");
    return true;
  }

  cooldownMsg.classList.add("hidden");
  return false;
}

/* VALIDATE ID */
function validateID() {
  const id = document.getElementById("discordID").value.trim();
  errorMsg.classList.add("hidden");

  if (!/^[0-9]{17,19}$/.test(id)) {
    errorMsg.textContent = "Invalid Discord ID";
    errorMsg.classList.remove("hidden");
    return;
  }

  if (checkCooldown(id)) return;

  step1.classList.add("hidden");
  step2.classList.remove("hidden");
}

/* EMBED COLOR */
function embedColor(type) {
  return {
    "mute": 0x22c55e,
    "kick": 0xf97316,
    "ban": 0xef4444,
    "global ban": 0x7c3aed,
    "blacklist": 0x0ea5e9,
    "warning": 0xeab308
  }[type.toLowerCase()] || 0x3b82f6;
}

/* SUBMIT APPEAL */
function submitAppeal() {
  step2.classList.add("hidden");
  loading.classList.remove("hidden");

  loadingTitle.textContent = "Submitting...";
  loadingMsg.textContent = "";

  const id = document.getElementById("discordID").value;
  const type = document.getElementById("appealType").value;
  const reason = document.getElementById("reason").value;
  const responsibility = document.getElementById("responsibility").value;
  const future = document.getElementById("future").value;

  const payload = {
    username: "Appeals",
    embeds: [{
      title: "New Appeal Submitted",
      color: embedColor(type),
      description:
        `**Discord ID:** ${id}\n` +
        `**Type:** ${type}\n\n` +
        `**Reason:**\n${reason}\n\n` +
        `**Responsibility:**\n${responsibility}\n\n` +
        `**Future Actions:**\n${future}`
    }]
  };

  fetch(webhook, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(payload)
  })
  .then(() => {
    localStorage.setItem("cooldown_" + id, Date.now());
    loadingTitle.textContent = "Appeal Sent!";
    loadingMsg.textContent = "You cannot submit another appeal for 2 hours.";
    startConfetti();
  })
  .catch(() => {
    loadingTitle.textContent = "Error";
    loadingMsg.textContent = "Failed to send appeal.";
  });
}

/* CONFETTI */
const canvas = document.getElementById("confetti");
const ctx = canvas.getContext("2d");
canvas.width = innerWidth;
canvas.height = innerHeight;

let pieces = [];

function startConfetti() {
  pieces = [];
  for (let i = 0; i < 150; i++) {
    pieces.push({
      x: Math.random() * canvas.width,
      y: Math.random() * -canvas.height,
      size: Math.random() * 6 + 4,
      speed: Math.random() * 3 + 2,
      color: ["#3b82f6","#22c55e","#eab308","#ef4444","#a855f7","#0ea5e9"][Math.floor(Math.random()*6)]
    });
  }
  animateConfetti();
  setTimeout(() => pieces = [], 3000);
}

function animateConfetti() {
  ctx.clearRect(0,0,canvas.width,canvas.height);
  pieces.forEach(p => {
    p.y += p.speed;
    if (p.y > canvas.height) p.y = -10;
    ctx.fillStyle = p.color;
    ctx.fillRect(p.x, p.y, p.size, p.size);
  });
  requestAnimationFrame(animateConfetti);
}
</script>

</body>
</html>
