



<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Appeals</title>

<style>
/* ===== GLOBAL ===== */
body {
  font-family: "Inter", Arial, sans-serif;
  background: linear-gradient(135deg, #0f172a, #1e293b);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  overflow: hidden;
}

/* ===== CARD ===== */
.card {
  width: 450px;
  background: rgba(15, 23, 42, 0.55);
  border: 1px solid rgba(255,255,255,0.08);
  backdrop-filter: blur(14px);
  padding: 30px;
  border-radius: 18px;
  box-shadow: 0 0 40px rgba(0,0,0,0.45);
  animation: fadeIn 0.5s ease-out;
}

h2 {
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
  background: rgba(255,255,255,0.06);
  color: white;
  font-size: 15px;
  outline: none;
  transition: 0.25s;
}

input:focus, textarea:focus, select:focus {
  background: rgba(255,255,255,0.12);
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
  border: 1px solid rgba(255,255,255,0.1);
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

/* ===== ERROR ===== */
.error {
  color: #f87171;
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

<!-- STEP 1 -->
<div class="card" id="step1">
  <h2>Enter Your Discord ID</h2>
  <input type="text" id="discordID" placeholder="Example: 123456789012345678">
  <div id="errorMsg" class="error hidden">Invalid Discord ID</div>
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
  <h2>Submitting...</h2>
  <div class="spinner"></div>
</div>

<script>
const webhook = "YOUR_WEBHOOK_HERE";

/* ===== VALIDATE DISCORD ID ===== */
function validateID() {
  const id = document.getElementById("discordID").value.trim();
  const error = document.getElementById("errorMsg");

  const valid = /^[0-9]{17,19}$/.test(id);

  if (!valid) {
    error.classList.remove("hidden");
    return;
  }

  error.classList.add("hidden");

  if (id === "lostrbxofficial") {
    document.getElementById("step1").innerHTML =
      "<h2>Hello desired, it's funny how you're trying to appeal.</h2>";
    return;
  }

  document.getElementById("step1").classList.add("hidden");
  document.getElementById("step2").classList.remove("hidden");
}

/* ===== SEVERITY CHECK ===== */
function containsSevere(text) {
  const keywords = ["sexual", "nuke", "harassment", "racial slur", "n word"];
  text = text.toLowerCase();
  return keywords.some(k => text.includes(k));
}

/* ===== SUBMIT APPEAL ===== */
function submitAppeal() {
  document.getElementById("step2").classList.add("hidden");
  document.getElementById("loading").classList.remove("hidden");

  const id = document.getElementById("discordID").value;
  const type = document.getElementById("appealType").value;
  const reason = document.getElementById("reason").value;
  const responsibility = document.getElementById("responsibility").value;
  const future = document.getElementById("future").value;

  let severityWarning = "";
  if (containsSevere(reason + responsibility + future)) {
    severityWarning = "⚠️ **HIGH SEVERITY — STRONGLY ADVISE AGAINST REVERSAL** ⚠️\n\n";
  }

  const payload = {
    username: "Appeals",
    avatar_url: "https://media.discordapp.net/attachments/1486852883902103722/1487244767019405403/Screenshot_2026-03-27_235014-removebg-preview.png",
    content:
      severityWarning +
      `**New Appeal Submitted**\n\n` +
      `**Discord ID:** ${id}\n` +
      `**Type:** ${type}\n\n` +
      `**Reason:**\n${reason}\n\n` +
      `**Responsibility:**\n${responsibility}\n\n` +
      `**Future Actions:**\n${future}`
  };

  fetch(webhook, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(payload)
  })
  .then(() => {
    document.getElementById("loading").innerHTML = "<h2>Appeal Sent Successfully!</h2>";
  })
  .catch(() => {
    document.getElementById("loading").innerHTML = "<h2>Error sending appeal.</h2>";
  });
}
</script>

</body>
</html>
