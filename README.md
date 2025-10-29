 JACK-website-
Public 
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>JACK — Login</title>
  <style>
    :root{--blue:#000080;--bg:#f2f2f2}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,Segoe UI,Arial,sans-serif;background:var(--bg)}
    .header{background:var(--blue);color:#fff;padding:14px 16px;text-align:center;font-weight:800;letter-spacing:2px;font-size:20px;position:sticky;top:0;z-index:20}
    .container{max-width:420px;margin:40px auto;padding:12px}
    .card{background:#fff;border-radius:12px;padding:22px;box-shadow:0 8px 30px rgba(2,6,23,0.08);text-align:center}
    .title{font-size:28px;margin:6px 0 10px}
    .subtitle{color:#555;margin-bottom:12px}
    input[type="text"],input[type="password"],input[type="number"]{
      width:100%;padding:12px;border-radius:8px;border:1px solid #d0d0d0;
      margin:8px 0;font-size:15px
    }
    .btn{display:inline-block;width:100%;padding:12px;border-radius:8px;background:var(--blue);
      color:#fff;border:0;font-weight:700;cursor:pointer;margin-top:12px;font-size:16px}
    .btn:hover{background:#0010cc}
    .hidden{display:none}
    .welcome-name{font-size:28px;margin:12px 0}
    .name-style{
      font-size:32px;font-weight:900;
      background:linear-gradient(90deg,#ff0000,#ff9900,#33cc33,#3399ff,#9933ff);
      -webkit-background-clip:text;-webkit-text-fill-color:transparent;
      text-shadow:2px 2px 4px rgba(0,0,0,0.3);letter-spacing:2px;
    }
    .qr{margin-top:18px}
    .qr img{width:200px;height:200px;border-radius:10px;border:6px solid var(--blue);background:#fff;padding:6px}
    .logout,.scanner-btn{display:inline-block;margin-top:18px;padding:10px 18px;background:#444;color:#fff;border-radius:8px;text-decoration:none}
    video{width:100%;margin-top:12px;border-radius:8px}
  </style>
</head>
<body>

  <!-- Top header -->
  <div class="header">🌟 <span class="name-style">WELCOME JACK</span> 🌟</div>

  <div class="container">
    <!-- LOGIN -->
    <div id="loginCard" class="card">
      <div class="title"><span class="name-style">JACK</span></div>
      <div class="subtitle">Enter your mobile number and PIN</div>
      <input id="mobileField" type="text" placeholder="Enter your mobile number" />
      <input id="pinField" type="password" placeholder="Enter your PIN" />
      <button class="btn" onclick="sendCode()">Send Code</button>

      <!-- Confirm Code Box -->
      <div id="codeBox" class="hidden">
        <input id="codeField" type="number" placeholder="Enter confirmation code" />
        <button class="btn" onclick="handleLogin()">Confirm & Login</button>
      </div>
    </div>

    <!-- WELCOME -->
    <div id="welcomeCard" class="card hidden">
      <div class="welcome-name" id="welcomeName">
        <span class="name-style">WELCOME JACK</span> 🔞
      </div>
      <div class="subtitle">You have successfully logged in.</div>

      <div class="qr">
        <h3>Your QR Code</h3>
        <img id="qrImage" src="" alt="QR Code">
      </div>

      <!-- Scanner Button -->
      <a href="#" class="scanner-btn" onclick="toggleScanner();return false;">Open Scanner</a>
      <div id="scannerBox" class="hidden">
        <video id="preview"></video>
      </div>

      <!-- YouTube Link -->
      <div style="margin-top:15px;">
        <a href="https://youtube.com/@aiai-g5b?si=kBMRFoeYFpFhhT8f" 
           target="_blank" 
           style="display:inline-block;padding:10px 18px;background:#cc0000;color:#fff;
                  border-radius:8px;text-decoration:none;font-weight:600;">
           ▶ Visit My YouTube
        </a>
      </div>

      <!-- Logout -->
      <a href="#" class="logout" onclick="doLogout();return false;">Log Out</a>
    </div>
  </div>

<!-- QR Scanner Library -->
<script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>

<script>
  let generatedCode=null;

  function sendCode(){
    const mobile=document.getElementById("mobileField").value.trim();
    const pin=document.getElementById("pinField").value.trim();

    if(mobile==="" || pin===""){
      alert("❌ Please enter mobile number and PIN first.");
      return;
    }

    // Random 6-digit code generate
    generatedCode=Math.floor(100000+Math.random()*900000);
    alert("✅ Your confirmation code is: "+generatedCode);

    // Show confirm code input box
    document.getElementById("codeBox").classList.remove("hidden");
  }

  function handleLogin(){
    const code=document.getElementById("codeField").value.trim();

    if(code==="" || code!=generatedCode){
      alert("❌ Invalid confirmation code.");
      return;
    }

    // Success → show welcome
    document.getElementById("loginCard").classList.add("hidden");
    document.getElementById("welcomeCard").classList.remove("hidden");

    const mobile=document.getElementById("mobileField").value.trim();
    const pin=document.getElementById("pinField").value.trim();

    const data=`${mobile} | PIN: ${pin} | Logged in at ${new Date().toLocaleString()}`;
    document.getElementById("qrImage").src=
      "https://api.qrserver.com/v1/create-qr-code/?size=300x300&data="+encodeURIComponent(data);

    // Reset
    document.getElementById("mobileField").value="";
    document.getElementById("pinField").value="";
    document.getElementById("codeField").value="";
    document.getElementById("codeBox").classList.add("hidden");
    generatedCode=null;
  }

  function doLogout(){
    document.getElementById("welcomeCard").classList.add("hidden");
    document.getElementById("loginCard").classList.remove("hidden");
    stopScanner();
  }

  // QR Scanner
  let scanner=null;
  function toggleScanner(){
    const box=document.getElementById("scannerBox");
    if(box.classList.contains("hidden")){
      box.classList.remove("hidden");
      startScanner();
    }else{
      box.classList.add("hidden");
      stopScanner();
    }
  }

  function startScanner(){
    scanner=new Html5Qrcode("preview");
    scanner.start({facingMode:"environment"},{fps:10,qrbox:250},
      qrCodeMessage=>{
        alert("Scanned: "+qrCodeMessage);
        stopScanner();
        document.getElementById("scannerBox").classList.add("hidden");
      });
  }

  function stopScanner(){
    if(scanner){scanner.stop().catch(()=>{});scanner=null;}
  }
</script>

</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>PDF Upload Form (Locked)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      user-select: none;
    }
    #videoButton {
      background-color: black;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
    #videoButton:hover { background-color: #333; }

    .whatsapp-btn {
      width: 180px;
      height: 45px;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      margin: 5px;
      transition: 0.3s;
    }

    #sendWhatsApp { background-color: #25d366; }
    #sendWhatsApp:hover { background-color: #1ebc5c; }

    #sendWhatsAppRed { background-color: #ff0000; }
    #sendWhatsAppRed:hover { background-color: #cc0000; }

    .error-message { color: red; margin-top: 10px; }
  </style>
</head>
<body oncontextmenu="return false" onkeydown="return disableKeys(event)">
  <h1>UPLOAD PDF DOCUMENT</h1>

  <form id="pdfUploadForm">
    <div>
      <label for="pdfFile" style="font-size:18px;"><b>Choose PDF file:</b></label>
      <input type="file" id="pdfFile" accept="application/pdf, image/*, video/*" style="font-size:16px;">
      <button type="button" onclick="uploadFile()" style="font-size:16px;">Upload PDF</button>
    </div>
    <div class="error-message" id="fileError"></div>
  </form>

  <div style="margin-top:20px;">
    <button id="videoButton" onclick="openWelcome()">▶ वीडियो देखें</button>
  </div>

  <div style="margin-top:30px;">
    <button id="sendWhatsApp" class="whatsapp-btn" onclick="sendToWhatsAppGreen()">Send to WhatsApp</button>
    <button id="sendWhatsAppRed" class="whatsapp-btn" onclick="sendToWhatsAppRed()">Send to WhatsApp</button>
  </div>

  <script>
    let uploadedFiles = [];
    let lastPage = '';

    // 🔒 Secure history functions
    function getSavedFiles() {
      const files = localStorage.getItem('savedFiles');
      const backup = localStorage.getItem('backupFiles');
      if (files) return JSON.parse(files);
      if (backup) {
        localStorage.setItem('savedFiles', backup);
        return JSON.parse(backup);
      }
      return [];
    }

    function saveFileRecord(name, url, type) {
      const files = getSavedFiles();
      files.push({ name, url, type, date: new Date().toLocaleString() });
      localStorage.setItem('savedFiles', JSON.stringify(files));
      localStorage.setItem('backupFiles', JSON.stringify(files)); // 🧠 backup copy permanent
    }

    // 🧩 Upload function
    function uploadFile() {
      const fileInput = document.getElementById('pdfFile');
      const file = fileInput.files[0];
      const error = document.getElementById('fileError');
      error.textContent = "";

      if (!file) {
        error.textContent = "⚠ कृपया एक फ़ाइल चुनें।";
        return;
      }

      uploadedFiles.push(file);
      const fileURL = URL.createObjectURL(file);
      saveFileRecord(file.name, fileURL, file.type);
      alert("✅ " + file.name + " upload complete! (History me add ho gaya)");
    }

    // 📽 Show uploaded file
    function openWelcome() {
      if (uploadedFiles.length === 0) {
        alert("⚠ पहले कोई फ़ाइल अपलोड करें!");
        return;
      }

      const file = uploadedFiles[uploadedFiles.length - 1];
      const url = URL.createObjectURL(file);

      let fileDisplay = "";
      if (file.type.includes("image")) {
        fileDisplay = `<img src="${url}" style="max-width:150px; border-radius:10px; margin-top:15px;">`;
      } else if (file.type.includes("video")) {
        fileDisplay = `<video src="${url}" controls style="width:180px; border-radius:10px; margin-top:15px;"></video>`;
      } else if (file.type.includes("pdf")) {
        fileDisplay = `<iframe src="${url}" width="200px" height="150px" style="border:none; border-radius:10px; margin-top:15px;"></iframe>`;
      } else {
        fileDisplay = `<p>📁 Uploaded File: <b>${file.name}</b></p>`;
      }

      lastPage = document.body.innerHTML;

      document.body.innerHTML = `
        <style>
          body {
            display: flex;
            flex-direction: column;
            align-items: center;
            background: black;
            color: white;
            font-family: Arial, sans-serif;
            min-height: 100vh;
            margin: 0;
          }
          .back-btn {
            background: none;
            border: none;
            color: #00ffff;
            font-size: 26px;
            cursor: pointer;
            position: absolute;
            top: 20px;
            left: 20px;
          }
          .back-btn:hover { color: white; }
          .red-btn {
            background: none;
            border: none;
            color: #ff4444;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
            position: absolute;
            top: 20px;
            right: 20px;
          }
          h1 {
            font-size: 50px;
            text-align: center;
            text-shadow: 0 0 15px #00ffff;
            margin-top: 80px;
          }
          .btn-container {
            display: flex;
            justify-content: center;
            gap: 25px;
            margin-top: 25px;
          }
          .action-btn {
            background: none;
            border: none;
            color: #00ffff;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
          }
        </style>

        <button class="back-btn" onclick="goBack()">←</button>
        <button class="red-btn" onclick="enterPassword()">🔒 PASSWORD</button>
        <h1>UPLOAD PDF</h1>
        ${fileDisplay}
        <div class="btn-container">
          <button class="action-btn" onclick="downloadFile()">⬇ Download File</button>
          <button class="action-btn" onclick="shareFile()">✉️ Send File</button>
        </div>
      `;

      window.downloadFile = function () {
        const blob = new Blob([file], { type: file.type });
        const link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = file.name;
        link.click();
      }

      window.shareFile = function () {
        const number = "7007576493";
        const message = "नमस्ते! मैंने एक फ़ाइल शेयर की है।";
        window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
      }

      window.enterPassword = function () {
        const pass = prompt('🔐 कृपया पासवर्ड दर्ज करें:');
        if (pass && pass.toLowerCase() === 'jack7005') {
          openHistoryPage();
        } else if (pass) {
          alert('❌ गलत पासवर्ड!');
        }
      }
    }

    // 🧾 HISTORY PAGE (Permanent + Locked)
    function openHistoryPage() {
      const files = getSavedFiles();
      let fileListHTML = files.map(f => {
        if (f.type.includes("pdf")) {
          return `<iframe src="${f.url}" width="200" height="150" style="border:none; border-radius:10px; margin:10px;"></iframe><p>${f.name}<br><small>${f.date}</small></p>`;
        } else if (f.type.includes("image")) {
          return `<img src="${f.url}" style="max-width:150px; border-radius:10px; margin:10px;"><p>${f.name}<br><small>${f.date}</small></p>`;
        } else if (f.type.includes("video")) {
          return `<video src="${f.url}" controls style="width:180px; border-radius:10px; margin:10px;"></video><p>${f.name}<br><small>${f.date}</small></p>`;
        } else {
          return `<p>📁 ${f.name}<br><small>${f.date}</small></p>`;
        }
      }).join('');

      document.body.innerHTML = `
        <body oncontextmenu="return false" onkeydown="return disableKeys(event)">
        <style>
          body {
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
          }
          h1 {
            margin-top: 40px;
            font-size: 48px;
            text-shadow: 0 0 15px #00ffff;
          }
          .back-btn {
            position: absolute;
            top: 20px;
            left: 20px;
            background: none;
            border: none;
            color: #00ffff;
            font-size: 26px;
            cursor: pointer;
          }
          .file-container {
            margin-top: 30px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
          }
          .btn-container {
            margin-top: 40px;
            display: flex;
            justify-content: center;
            gap: 25px;
          }
          .action-btn {
            background: none;
            border: 1px solid #00ffff;
            color: #00ffff;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
            border-radius: 8px;
            padding: 8px 20px;
          }
        </style>
        <button class="back-btn" onclick="goBack()">←</button>
        <h1>HISTORY</h1>
        <div class="file-container">${fileListHTML}</div>
        <div class="btn-container">
          <button class="action-btn" onclick="shareHistory()">✉️ Send</button>
          <button class="action-btn" onclick="downloadHistory()">⬇ Download</button>
        </div>
        </body>
      `;

      window.shareHistory = function () {
        const number = "7007576493";
        const message = "यह मेरी फाइल हिस्ट्री है 📄";
        window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
      }

      window.downloadHistory = function () {
        const blob = new Blob([JSON.stringify(files, null, 2)], { type: "application/json" });
        const link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = "history.json";
        link.click();
      }
    }

    window.goBack = function() {
      document.body.innerHTML = lastPage;
    }

    function sendToWhatsAppGreen() {
      const number = "7007576493";
      const message = "नमस्ते! मैंने एक फ़ाइल अपलोड की है।";
      window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
    }

    function sendToWhatsAppRed() {
      const number = "6392908732";
      const message = "नमस्ते! मैंने एक फ़ाइल अपलोड की है।";
      window.open(`https://wa.me/${number}?text=${encodeURIComponent(message)}`, "_blank");
    }

    function disableKeys(e) {
      if (
        e.keyCode === 123 || 
        (e.ctrlKey && e.shiftKey && e.keyCode === 73) || 
        (e.ctrlKey && e.shiftKey && e.keyCode === 74) || 
        (e.ctrlKey && e.keyCode === 85) || 
        (e.ctrlKey && e.keyCode === 83)
      ) {
        alert("🔒 Page is locked!");
        e.preventDefault();
        return false;
      }
    }
  </script>
</body>
</html>
