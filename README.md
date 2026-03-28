

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Appeals</title>
<style>
body { font-family: Arial, sans-serif; background:#0f172a; color:white; display:flex; justify-content:center; align-items:center; height:100vh; margin:0; }
.container { width:400px; background:#1e293b; padding:20px; border-radius:10px; box-shadow:0 0 20px rgba(0,0,0,0.5); }
input, textarea, select, button { width:100%; margin-top:10px; padding:10px; border:none; border-radius:5px; }
button { background:#3b82f6; color:white; cursor:pointer; }
.hidden { display:none; }
.fade { animation: fade 0.5s ease-in-out; }
@keyframes fade { from {opacity:0;} to {opacity:1;} }
</style>
</head>
<body>

<div class="container fade" id="step1">
<h2>Enter Discord Username</h2>
<input type="text" id="username" placeholder="Username#0000">
<button onclick="nextStep()">Continue</button>
</div>

<div class="container hidden" id="step2">
<h2>Appeal Form</h2>
<select id="appealType">
<option>Mute</option>
<option>Kick</option>
<option>Ban</option>
<option>Global ban</option>
<option>Blacklist</option>
<option>Warning</option>
</select>
<textarea id="reason" placeholder="Why were you given this?"></textarea>
<textarea id="responsibility" placeholder="Do you take responsibility?"></textarea>
<textarea id="future" placeholder="What will you do differently?"></textarea>
<button onclick="submitAppeal()">Send</button>
</div>

<div class="container hidden" id="loading">
<h2>Loading...</h2>
</div>

<script>
const webhook = "https://discord.com/api/webhooks/1487243550855795001/JNsSVMq38winuBfM4MjOhOLyTWzxvMSDTBNSvvUJB9MqlXn242zG6reZaSbqVC6d41oT";

function nextStep(){
  const usernameInput = document.getElementById('username').value.trim().toLowerCase();

  if(usernameInput === "lostrbxofficial"){
    document.getElementById('step1').innerHTML = "<h2>Hello desired it's funny how you trying to appeal.</h2>";
    return;
  }

  document.getElementById('step1').classList.add('hidden');
  const step2 = document.getElementById('step2');
  step2.classList.remove('hidden');
  step2.classList.add('fade');
}

function containsSevere(text){
  const keywords = ["sexual", "nuke", "harassment", "racial slur", "n word"];
  text = text.toLowerCase();
  return keywords.some(k => text.includes(k));
}

function submitAppeal(){
  document.getElementById('step2').classList.add('hidden');
  document.getElementById('loading').classList.remove('hidden');

  const username = document.getElementById('username').value;
  const type = document.getElementById('appealType').value;
  const reason = document.getElementById('reason').value;
  const responsibility = document.getElementById('responsibility').value;
  const future = document.getElementById('future').value;

  let severityWarning = "";
  if(containsSevere(reason + responsibility + future)){
    severityWarning = "⚠️ STRONGLY ADVISE NOT REVOKE - HIGH SEVERITY ⚠️\n\n";
  }

  const payload = {
    username: "Appeals",
    avatar_url: "https://media.discordapp.net/attachments/1486852883902103722/1487244767019405403/Screenshot_2026-03-27_235014-removebg-preview.png",
    content: severityWarning +
      `**New Appeal**\n\n` +
      `User: ${username}\n` +
      `Type: ${type}\n\n` +
      `Reason: ${reason}\n\n` +
      `Responsibility: ${responsibility}\n\n` +
      `Future Actions: ${future}`
  };

  fetch(webhook, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(payload)
  })
  .then(() => {
    document.getElementById('loading').innerHTML = "<h2>Appeal Sent!</h2>";
  })
  .catch(() => {
    document.getElementById('loading').innerHTML = "<h2>Error sending appeal.</h2>";
  });
}
</script>

</body>
</html>
