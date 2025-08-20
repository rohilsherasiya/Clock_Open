
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>LED Clock with Settings</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background: #000;
      color: #0f0;
      font-family: "Courier New", monospace;
      flex-direction: column;
    }

    .clock {
      font-size: 12vw;
      text-shadow: 0 0 20px currentColor, 0 0 40px currentColor;
      letter-spacing: 5px;
    }

    .date {
      font-size: 5vw;
      margin-top: 10px;
      text-shadow: 0 0 10px currentColor;
    }

    .settings-btn {
      position: absolute;
      top: 20px;
      right: 20px;
      font-size: 24px;
      cursor: pointer;
      background: none;
      border: none;
      color: #0f0;
      text-shadow: 0 0 10px currentColor;
    }

    /* Settings Modal */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.8);
      justify-content: center;
      align-items: center;
    }

    .modal-content {
      background: #111;
      padding: 20px;
      border-radius: 12px;
      color: white;
      width: 90%;
      max-width: 400px;
      box-shadow: 0 0 20px #0f0;
    }

    .modal-content h2 {
      margin-top: 0;
      text-align: center;
      color: #0f0;
    }

    .modal-content label {
      display: block;
      margin: 10px 0 5px;
    }

    .modal-content select,
    .modal-content input[type="checkbox"],
    .modal-content input[type="range"] {
      width: 100%;
      margin-bottom: 10px;
    }

    .close-btn {
      display: block;
      margin: 10px auto 0;
      padding: 8px 16px;
      background: #0f0;
      border: none;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <button class="settings-btn">⚙️</button>
  <div class="clock" id="clock"></div>
  <div class="date" id="date"></div>

  <!-- Settings Modal -->
  <div class="modal" id="settingsModal">
    <div class="modal-content">
      <h2>⚙ Clock Settings</h2>

      <label for="timeFormat">Time Format:</label>
      <select id="timeFormat">
        <option value="24">24 Hour</option>
        <option value="12">12 Hour</option>
      </select>

      <label for="colorSelect">LED Color:</label>
      <select id="colorSelect">
        <option value="#0f0">Green</option>
        <option value="#f00">Red</option>
        <option value="#ffbf00">Amber</option>
        <option value="#00f">Blue</option>
      </select>

      <label><input type="checkbox" id="toggleDate" checked> Show Date</label>

      <label for="brightness">Brightness:</label>
      <input type="range" id="brightness" min="10" max="100" value="40">

      <button class="close-btn" id="closeBtn">Close</button>
    </div>
  </div>

  <script>
    const clockEl = document.getElementById('clock');
    const dateEl = document.getElementById('date');
    const modal = document.getElementById('settingsModal');
    const settingsBtn = document.querySelector('.settings-btn');
    const closeBtn = document.getElementById('closeBtn');

    // Settings
    const timeFormat = document.getElementById('timeFormat');
    const colorSelect = document.getElementById('colorSelect');
    const toggleDate = document.getElementById('toggleDate');
    const brightness = document.getElementById('brightness');

    let format = 24;
    let ledColor = "#0f0";
    let showDate = true;
    let glow = 40;

    function updateClock() {
      const now = new Date();
      let hours = now.getHours();
      let minutes = now.getMinutes();
      let seconds = now.getSeconds();

      if (format === 12) {
        hours = hours % 12 || 12;
      }

      const timeString = 
        String(hours).padStart(2, '0') + ":" +
        String(minutes).padStart(2, '0') + ":" +
        String(seconds).padStart(2, '0');

      clockEl.textContent = timeString;
      clockEl.style.color = ledColor;
      clockEl.style.textShadow = `0 0 ${glow/2}px ${ledColor}, 0 0 ${glow}px ${ledColor}`;

      if (showDate) {
        const dateString = now.toDateString();
        dateEl.textContent = dateString;
        dateEl.style.color = ledColor;
        dateEl.style.textShadow = `0 0 ${glow/2}px ${ledColor}`;
      } else {
        dateEl.textContent = "";
      }
    }

    setInterval(updateClock, 1000);
    updateClock();

    // Settings controls
    settingsBtn.onclick = () => modal.style.display = "flex";
    closeBtn.onclick = () => modal.style.display = "none";

    timeFormat.onchange = () => format = parseInt(timeFormat.value);
    colorSelect.onchange = () => ledColor = colorSelect.value;
    toggleDate.onchange = () => showDate = toggleDate.checked;
    brightness.oninput = () => glow = brightness.value;
  </script>
</body>
