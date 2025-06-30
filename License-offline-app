<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>License Admin (Offline HTML App)</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body { font-family: sans-serif; max-width: 600px; margin: auto; padding: 20px; background: #f4f4f4; }
    form, .box { background: #fff; padding: 20px; margin-top: 20px; border-radius: 8px; box-shadow: 0 0 10px #ccc; }
    input, button { width: 100%; padding: 10px; margin-top: 10px; }
    button[type="button"] { background: #eee; cursor: pointer; }
    .result { padding: 15px; border-radius: 5px; margin-top: 10px; }
    .valid { background: #d4edda; color: #155724; }
    .invalid { background: #f8d7da; color: #721c24; }
    .hidden { display: none; }
  </style>
</head>
<body>

<h1>License Admin (Offline HTML App)</h1>

<form id="loginForm">
  <h2>Admin Login</h2>
  <input type="text" id="username" placeholder="Username" required>
  <input type="password" id="password" placeholder="Password" required>
  <button type="submit">Login</button>
</form>

<form id="licenseForm" class="hidden">
  <h2>Create License</h2>
  <input type="text" id="key" placeholder="License Key" required>
  <button type="button" onclick="generateKey()">🔄 Generate Key</button>
  <input type="date" id="expiry">
  <label><input type="checkbox" id="active" checked> Active</label>
  <button type="submit">Add License</button>
</form>

<form id="validateForm">
  <h2>Validate License</h2>
  <input type="text" id="validateKey" placeholder="Enter License Key" required>
  <button type="submit">Validate</button>
</form>

<div id="result" class="result hidden"></div>

<script>
  if (!localStorage.getItem("users")) {
    localStorage.setItem("users", JSON.stringify([{ username: "admin", password: "admin123" }]));
    localStorage.setItem("licenses", JSON.stringify([]));
  }

  const loginForm = document.getElementById("loginForm");
  const licenseForm = document.getElementById("licenseForm");
  const validateForm = document.getElementById("validateForm");
  const resultBox = document.getElementById("result");

  loginForm.onsubmit = function(e) {
    e.preventDefault();
    const u = document.getElementById("username").value;
    const p = document.getElementById("password").value;
    const users = JSON.parse(localStorage.getItem("users"));
    if (users.some(uo => uo.username === u && uo.password === p)) {
      alert("✅ Logged in!");
      loginForm.classList.add("hidden");
      licenseForm.classList.remove("hidden");
    } else alert("❌ Login failed");
  };

  licenseForm.onsubmit = function(e) {
    e.preventDefault();
    const key = document.getElementById("key").value.trim();
    const expiry = document.getElementById("expiry").value;
    const active = document.getElementById("active").checked;
    const licenses = JSON.parse(localStorage.getItem("licenses"));
    if (licenses.find(l => l.key === key)) return alert("License exists");
    licenses.push({ key, active, expiresAt: expiry, usageCount: 0, lastUsed: null });
    localStorage.setItem("licenses", JSON.stringify(licenses));
    alert("✅ License added");
    licenseForm.reset();
  };

  validateForm.onsubmit = function(e) {
    e.preventDefault();
    const k = document.getElementById("validateKey").value.trim();
    const licenses = JSON.parse(localStorage.getItem("licenses"));
    const l = licenses.find(x => x.key === k);
    const now = new Date();
    if (!l) return showResult("❌ Not found", false);
    if (!l.active) return showResult("❌ Inactive", false);
    if (l.expiresAt && new Date(l.expiresAt) < now) return showResult("❌ Expired", false);
    l.usageCount++;
    l.lastUsed = now.toISOString();
    localStorage.setItem("licenses", JSON.stringify(licenses));
    showResult(`✅ Valid. Uses: ${l.usageCount}, Expires: ${l.expiresAt || "Never"}, Last: ${l.lastUsed}`, true);
  };

  function showResult(msg, ok) {
    resultBox.innerHTML = msg;
    resultBox.className = "result " + (ok ? "valid" : "invalid");
    resultBox.classList.remove("hidden");
  }

  function generateKey(len = 16) {
    const chars = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789";
    let k = "";
    for (let i = 0; i < len; i++) k += chars.charAt(Math.floor(Math.random() * chars.length));
    document.getElementById("key").value = k.match(/.{1,4}/g).join("-");
  }
</script>

</body>
</html>
