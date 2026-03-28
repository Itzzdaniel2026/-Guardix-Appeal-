



<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Appeals</title>

<style>
* {
  box-sizing: border-box;
}

/* ===== GLOBAL ===== */
body {
  font-family: "Inter", Arial, sans-serif;
  background: #020617;
  color: white;
  margin: 0;
  height: 100vh;
  overflow: hidden;
}

/* Parallax background */
.bg-layer {
  position: fixed;
  inset: 0;
  background: radial-gradient(circle at top, #1e293b, #020617 70%);
  z-index: -3;
}

.bg-blur {
  position: fixed;
  inset: -40px;
  background-image: url("https://images.pexels.com/photos/7130555/pexels-photo-7130555.jpeg");
  background-size: cover;
  background-position: center;
  filter: blur(18px) brightness(0.35);
  opacity: 0.9;
  z-index: -2;
  transform: translate3d(0,0,0);
}

/* Parallax overlay */
.bg-noise {
  position: fixed;
  inset: 0;
  background: radial-gradient(circle at top, rgba(59,130,246,0.18), transparent 55%);
  mix-blend-mode: screen;
  opacity: 0.7;
  z-index: -1;
}

/* Center wrapper */
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* ===== CARD ===== */
.card {
  width: 460px;
  background: rgba(15, 23, 42, 0.7);
  border: 1px solid rgba(148,163,184,0.35);
  backdrop-filter: blur(18px);
  padding: 28px;
  border-radius: 18px;
  box-shadow: 0 0 40px rgba(0,0,0,0.6);
  animation: fadeIn 0.5s ease-out;
  position: relative;
}

.card h2 {
  margin-top: 0;
  font-weight: 700;
  text-align: center;
  letter-spacing: 0.5px;
}

/* ===== INPUTS ===== */
input, textarea, select, button {
  width: 100%;
  margin-top: 14px;
  padding: 12px;
  border: none;
  border-radius: 10px;
  background: rgba(15,23,42,0.85);
  color: white;
  font-size: 15px;
  outline: none;
  transition: 0.25s;
}

input::placeholder,
textarea::placeholder {
  color: rgba(148,163,184,0.8);
}

input:focus, textarea:focus, select:focus {
  background: rgba(15,23,42,1);
  box-shadow: 0 0 0 2px #3b82f6;
}

/* ===== CUSTOM SELECT ===== */
select {
  appearance: none;
  background-image: url("data:image/svg+xml;utf8,<svg fill='white' height='20' width='20' viewBox='0 0 20 20'><path d='M5.5 7l4.5 4 4.5-4'/></svg>");
  background-repeat: no-repeat;
  background-position: right 12px center;
  background-size: 18px;
  padding-right: 40px;
  border: 1px solid rgba(148,163,184,0.5);
}

select:hover {
  border-color: #3b82f6;
}

/* ===== BUTTON ===== */
button {
  background: #3b82f6;
  font-weight: 600;
  cursor: pointer;
  transition: 0.25s;
}

button:hover {
  background: #60a5fa;
  transform: translateY(-2px);
}

/* ===== ERROR / INFO ===== */
.error {
  color: #f87171;
  margin-top: 8px;
  font-size: 14px;
  text-align: center;
}

.info {
  color: #38bdf8;
  margin-top: 8px;
  font-size: 14px;
  text-align: center;
}

/* ===== HIDDEN ===== */
.hidden {
  display: none;
}

/* ===== LOADING SPINNER ===== */
.spinner {
  border: 4px solid rgba(255,255,255,0.15);
  border-top: 4px solid #3b82f6;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  margin: 20px auto;
  animation: spin 0.8s linear infinite;
}

/* ===== INTRO SCREEN ===== */
.intro-card {
  text-align: center;
}

.intro-title {
  font-size: 26px;
  font-weight: 800;
  margin-bottom: 8px;
}

.intro-sub {
  font-size: 14px;
  color: #cbd5f5;
  margin-bottom: 18px;
}

.pill {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 6px 12px;
  border-radius: 999px;
  background: rgba(15,23,42,0.9);
  border: 1px solid rgba(59,130,246,0.7);
  font-size: 12px;
  color: #e5e7eb;
  margin-bottom: 16px;
}

/* ===== MODAL ===== */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(15,23,42,0.75);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 20;
}

.modal {
  width: 380px;
  background: rgba(15,23,42,0.95);
  border-radius: 16px;
  padding: 20px 22px;
  border: 1px solid rgba(148,163,184,0.5);
  box-shadow: 0 0 30px rgba(0,0,0,0.7);
  animation: fadeIn 0.25s ease-out;
}

.modal-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.modal-title {
  font-weight: 700;
}

.modal-body {
  font-size: 14px;
  color: #e5e7eb;
  margin-bottom: 16px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.modal-btn-secondary {
  background: rgba(15,23,42,1);
  border: 1px solid rgba(148,163,184,0.7);
}

.modal-btn-secondary:hover {
  background: rgba(30,41,59,1);
  transform: translateY(-1px);
}

/* ===== CONFETTI CANVAS ===== */
#confettiCanvas {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 15;
}

/* ===== ANIMATIONS ===== */
@keyframes spin {
  to { transform: rotate(360deg); }
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(15px); }
  to { opacity: 1; transform: translateY(0); }
}
</style>
</head>
<body>

<div class="bg-layer"></div>
<div class="bg-blur" id="bgBlur"></div>
<div class="bg-noise"></div>

<canvas id="confettiCanvas"></canvas>

<div class="wrapper">

  <!-- INTRO SCREEN -->
  <div class="card intro-card" id="intro">
    <div class="pill">
      <span>🔒</span>
      <span>Official Appeal Portal</span>
    </div>
    <div class="intro-title">Moderation Appeal</div>
    <div class="intro-sub">Submit a detailed appeal for your punishment. Abuse of this system may result in further action.</div>
    <button onclick="goToID()">Start Appeal</button>
  </div>

  <!-- STEP 1: DISCORD ID -->
  <div class="card hidden" id="step1">
    <h2>Enter Your Discord ID</h2>
    <input type="text" id="discordID" placeholder="Example: 123456789012345678">
    <div id="cooldownMsg" class="info hidden"></div>
    <div id="errorMsg" class="error hidden">Invalid Discord ID</div>
    <button onclick="validateID()">Continue</button>
  </div>

  <!-- STEP 2: APPEAL FORM -->
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

    <button onclick="openConfirmModal()">Submit Appeal</button>
  </div>

  <!-- LOADING / STATUS -->
  <div class="card hidden" id="loading">
    <h2 id="loadingTitle">Submitting...</h2>
    <div class="spinner" id="spinner"></div>
    <div id="statusMsg" class="info"></div>
  </div>

</div>

<!-- MODAL -->
<div class="modal-backdrop hidden" id="modalBackdrop">
  <div class="modal">
    <div class="modal-header">
      <span>⚠️</span>
      <span class="modal-title">Confirm Submission</span>
    </div>
    <div class="modal-body">
      You are about to submit an appeal. Make sure all information is accurate and honest.  
      Once submitted, you will not be able to appeal again for 2 hours.
    </div>
    <div class="modal-actions">
      <button class="modal-btn-secondary" onclick="closeConfirmModal()">Cancel</button>
      <button onclick="submitAppeal()">Confirm</button>
    </div>
  </div>
</div>

<script>
const webhook = "YOUR_WEBHOOK_HERE";
const COOLDOWN_HOURS = 2;
const COOLDOWN_KEY = "appeal_last_submit";

const intro = document.getElementById("intro");
const step1 = document.getElementById("step1");
const step2 = document.getElementById("step2");
const loading = document.getElementById("loading");
const errorMsg = document.getElementById("errorMsg");
const cooldownMsg = document.getElementById("cooldownMsg");
const modalBackdrop = document.getElementById("modalBackdrop");
const loadingTitle = document.getElementById("loadingTitle");
const statusMsg = document.getElementById("statusMsg");
const spinner = document.getElementById("spinner");
const confettiCanvas = document.getElementById("confettiCanvas");
const ctx = confettiCanvas.getContext("2d");

/* ===== PARALLAX ===== */
document.addEventListener("mousemove", (e) => {
  const x = (e.clientX / window.innerWidth - 0.5) * 20;
  const y = (e.clientY / window.innerHeight - 0.5) * 20;
  document.getElementById("bgBlur").style.transform = `translate3d(${x}px, ${y}px, 0) scale(1.05)`;
});

/* ===== INTRO NAVIGATION ===== */
function goToID() {
  intro.classList.add("hidden");
  step1.classList.remove("hidden");
  checkCooldownMessage();
}

/* ===== COOLDOWN CHECK ===== */
function checkCooldownMessage() {
  const last = localStorage.getItem(COOLDOWN_KEY);
  if (!last) return;

  const lastTime = parseInt(last, 10);
  const now = Date.now();
  const diffMs = now - lastTime;
  const diffHours = diffMs / (1000 * 60 * 60);

  if (diffHours < COOLDOWN_HOURS) {
    const remainingMs = COOLDOWN_HOURS * 60 * 60 * 1000 - diffMs;
    const remainingMinutes = Math.ceil(remainingMs / (1000 * 60));
    cooldownMsg.textContent = `Cooldown active: you can submit another appeal in about ${remainingMinutes} minute(s).`;
    cooldownMsg.classList.remove("hidden");
  } else {
    cooldownMsg.classList.add("hidden");
  }
}

/* ===== VALIDATE DISCORD ID ===== */
function validateID() {
  const id = document.getElementById("discordID").value.trim();
  const valid = /^[0-9]{17,19}$/.test(id);

  // Cooldown hard block
  const last = localStorage.getItem(COOLDOWN_KEY);
  if (last) {
    const lastTime = parseInt(last, 10);
    const now = Date.now();
    const diffHours = (now - lastTime) / (1000 * 60 * 60);
    if (diffHours < COOLDOWN_HOURS) {
      checkCooldownMessage();
      errorMsg.textContent = "You are currently on cooldown and cannot submit another appeal yet.";
      errorMsg.classList.remove("hidden");
      return;
    }
  }

  if (!valid) {
    errorMsg.textContent = "Invalid Discord ID";
    errorMsg.classList.remove("hidden");
    return;
  }

  errorMsg.classList.add("hidden");

  if (id === "lostrbxofficial") {
    step1.innerHTML = "<h2>Hello desired, it's funny how you're trying to appeal.</h2>";
    return;
  }

  step1.classList.add("hidden");
  step2.classList.remove("hidden");
}

/* ===== MODAL CONTROL ===== */
function openConfirmModal() {
  modalBackdrop.classList.remove("hidden");
}

function closeConfirmModal() {
  modalBackdrop.classList.add("hidden");
}

/* ===== SEVERITY CHECK ===== */
function containsSevere(text) {
  const keywords = ["sexual", "nuke", "harassment", "racial slur", "n word"];
  text = text.toLowerCase();
  return keywords.some(k => text.includes(k));
}

/* ===== EMBED COLOR BY TYPE ===== */
function getEmbedColor(type) {
  switch (type.toLowerCase()) {
    case "mute": return 0x22c55e;      // green
    case "kick": return 0xf97316;      // orange
    case "ban": return 0xef4444;       // red
    case "global ban": return 0x7c3aed;// purple
    case "blacklist": return 0x0ea5e9; // cyan
    case "warning": return 0xeab308;   // yellow
    default: return 0x3b82f6;          // blue
  }
}

/* ===== SUBMIT APPEAL ===== */
function submitAppeal() {
  closeConfirmModal();
  step2.classList.add("hidden");
  loading.classList.remove("hidden");
  loadingTitle.textContent = "Submitting...";
  spinner.classList.remove("hidden");
  statusMsg.textContent = "";

  const id = document.getElementById("discordID").value;
  const type = document.getElementById("appealType").value;
  const reason = document.getElementById("reason").value;
  const responsibility = document.getElementById("responsibility").value;
  const future = document.getElementById("future").value;

  let severityWarning = "";
  if (containsSevere(reason + responsibility + future)) {
    severityWarning = "⚠️ **HIGH SEVERITY — STRONGLY ADVISE AGAINST REVERSAL** ⚠️\n\n";
  }

  const embedColor = getEmbedColor(type);

  const payload = {
    username: "Appeals",
    avatar_url: "https://media.discordapp.net/attachments/1486852883902103722/1487244767019405403/Screenshot_2026-03-27_235014-removebg-preview.png",
    embeds: [
      {
        title: "New Appeal Submitted",
        description:
          severityWarning +
          `**Discord ID:** ${id}\n` +
          `**Type:** ${type}\n\n` +
          `**Reason:**\n${reason}\n\n` +
          `**Responsibility:**\n${responsibility}\n\n` +
          `**Future Actions:**\n${future}`,
        color: embedColor
      }
    ]
  };

  fetch(webhook, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(payload)
  })
  .then(() => {
    spinner.classList.add("hidden");
    loadingTitle.textContent = "Appeal Sent!";
    statusMsg.textContent = "Your appeal has been submitted. You will not be able to submit another appeal for 2 hours.";
    localStorage.setItem(COOLDOWN_KEY, Date.now().toString());
    startConfetti();
  })
  .catch(() => {
    spinner.classList.add("hidden");
    loadingTitle.textContent = "Error";
    statusMsg.textContent = "There was an error sending your appeal. Please try again later.";
  });
}

/* ===== CONFETTI ===== */
let confettiPieces = [];
let confettiAnimationId;

function initConfetti() {
  confettiCanvas.width = window.innerWidth;
  confettiCanvas.height = window.innerHeight;
  confettiPieces = [];
  for (let i = 0; i < 180; i++) {
    confettiPieces.push({
      x: Math.random() * confettiCanvas.width,
      y: Math.random() * confettiCanvas.height - confettiCanvas.height,
      size: Math.random() * 6 + 4,
      speed: Math.random() * 3 + 2,
      color: randomConfettiColor(),
      rotation: Math.random() * 360,
      rotationSpeed: Math.random() * 10 - 5
    });
  }
}

function randomConfettiColor() {
  const colors = ["#3b82f6", "#22c55e", "#eab308", "#ef4444", "#a855f7", "#0ea5e9"];
  return colors[Math.floor(Math.random() * colors.length)];
}

function renderConfetti() {
  ctx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
  confettiPieces.forEach(p => {
    p.y += p.speed;
    p.rotation += p.rotationSpeed;
    if (p.y > confettiCanvas.height) {
      p.y = -10;
      p.x = Math.random() * confettiCanvas.width;
    }
    ctx.save();
    ctx.translate(p.x, p.y);
    ctx.rotate(p.rotation * Math.PI / 180);
    ctx.fillStyle = p.color;
    ctx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size);
    ctx.restore();
  });
  confettiAnimationId = requestAnimationFrame(renderConfetti);
}

function startConfetti() {
  initConfetti();
  renderConfetti();
  setTimeout(() => {
    cancelAnimationFrame(confettiAnimationId);
    ctx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
  }, 3500);
}

window.addEventListener("resize", () => {
  confettiCanvas.width = window.innerWidth;
  confettiCanvas.height = window.innerHeight;
});
</script>

</body>
</html>
