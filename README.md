# chill-guy-clicker-2-
chilllllllll



<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clicker Game</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      body {
        font-family: 'Arial', sans-serif;
        background-color: #d1d1d1;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        transition: background-image 0.3s ease, background-size 0.3s ease;
      }

      #game-container {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 800px;
        height: 600px;
        background-color: rgba(238, 238, 238, 0.9); /* Light semi-transparent background */
        border-radius: 10px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        overflow: hidden;
        position: relative;
      }

      #color-picker {
        position: absolute;
        left: 0;
        top: 0;
        width: 150px;
        height: 100%;
        background-color: #f5f5f5;
        border-right: 2px solid #ddd;
        padding: 20px;
        box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
      }

      .color-option {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 40px;
        margin-bottom: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        cursor: pointer;
        transition: transform 0.2s;
      }

      .color-option:hover {
        transform: scale(1.05);
      }

      #character {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 45%;
        text-align: center;
        position: relative;
        cursor: pointer;
        margin-left: 165px;
      }

      #character img {
        z-index: 2;
        width: 150px;
        height: auto;
      }

      #stats {
        font-size: 20px;
        color: #333;
        margin-top: 15px;
        z-index: 2;
      }

      #score {
        font-size: 24px;
        margin-top: 10px;
        color: #555;
        z-index: 2;
      }

      #upgrades {
        display: flex;
        flex-direction: column;
        width: 40%;
        height: 100%;
        background-color: #ffffff;
        padding: 20px;
        box-shadow: -4px 0 10px rgba(0, 0, 0, 0.1);
        overflow-y: auto;
      }

      .upgrade-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px;
        background-color: #f7f7f7;
        border-radius: 5px;
        margin-bottom: 10px;
        font-size: 16px;
        color: #555;
        cursor: pointer;
        transition: background-color 0.2s;
      }

      .upgrade-item:hover {
        background-color: #e0e0e0;
      }

      #ascension {
        position: absolute;
        bottom: 15px;
        left: 175px;
        font-size: 16px;
        background-color: #ff5722;
        color: white;
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.2s;
      }

      #ascension:hover {
        background-color: #e64a19;
      }

      #ascension-multiplier {
        position: absolute;
        bottom: 15px;
        right: 15px;
        font-size: 16px;
        background-color: #4caf50;
        color: white;
        padding: 10px 20px;
      }

      #codes-button {
        position: absolute;
        bottom: 15px;
        left: 15px;
        font-size: 16px;
        background-color: #008CBA;
        color: white;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.2s;
      }

      #codes-button:hover {
        background-color: #006f8c;
      }

      #admin-text {
        position: absolute;
        top: 20px;
        left: 50%;
        transform: translateX(-50%);
        font-size: 32px;
        color: red;
        display: none; /* Initially hidden */
      }
    </style>
  </head>
  <body>
    <div id="game-container">
      <div id="color-picker">
        <div class="color-option" style="background-color: #d1d1d1;" onclick="setBackgroundColor('#d1d1d1')">Default</div>
        <div class="color-option" style="background-color: #ffcccb;" onclick="setBackgroundColor('#ffcccb')">Red</div>
        <div class="color-option" style="background-color: #d1ffd7;" onclick="setBackgroundColor('#d1ffd7')">Green</div>
        <div class="color-option" style="background-color: #d1e8ff;" onclick="setBackgroundColor('#d1e8ff')">Blue</div>
        <div class="color-option" style="background-color: #fff2cc;" onclick="setBackgroundColor('#fff2cc')">Yellow</div>
        <div class="color-option" style="background-color: #800080;" onclick="setBackgroundColor('#800080')">Purple</div>
      </div>
      <div id="character" onclick="incrementScore()">
        <img src="https://i.kym-cdn.com/photos/images/newsfeed/002/901/902/95c.png" alt="Character">
        <p id="stats">0 Chill Guy Per Second</p>
        <p id="score">Score: 0</p>
      </div>
      <div id="upgrades">
        <div class="upgrade-item" onclick="buyUpgrade('autoClicker')"> Buy Auto Clicker <span id="autoClickerCost">50</span>
        </div>
        <div class="upgrade-item" onclick="buyUpgrade('powerBoost')"> Boost Click Power <span id="powerBoostCost">100</span>
        </div>
      </div>
      <button id="ascension" onclick="ascend()">Ascend</button>
      <div id="ascension-multiplier">Ascension Multiplier: 1.0x</div>
      <button id="codes-button" onclick="enterCode()">Codes</button>
      <div id="admin-text">ADMIN JAKE AND MILES</div>
    </div>
    <script>
      let score = 0;
      let clickPower = 1;
      let autoClicker = 0;
      let autoClickerCost = 50;
      let powerBoostCost = 100;
      let multiplier = 1.0;
      const scoreDisplay = document.getElementById('score');
      const autoClickerCostDisplay = document.getElementById('autoClickerCost');
      const powerBoostCostDisplay = document.getElementById('powerBoostCost');
      const ascensionMultiplierDisplay = document.getElementById('ascension-multiplier');
      const adminText = document.getElementById('admin-text'); // The "ADMIN JAKE AND MILES" text element

      function incrementScore() {
        score += clickPower;
        updateScore();
      }

      function buyUpgrade(upgrade) {
        if (upgrade === 'autoClicker' && score >= autoClickerCost) {
          score -= autoClickerCost;
          autoClicker++;
          autoClickerCost = Math.floor(autoClickerCost * 1.5);
          autoClickerCostDisplay.textContent = autoClickerCost;
        } else if (upgrade === 'powerBoost' && score >= powerBoostCost) {
          score -= powerBoostCost;
          clickPower++;
          powerBoostCost = Math.floor(powerBoostCost * 1.5);
          powerBoostCostDisplay.textContent = powerBoostCost;
        }
        updateScore();
      }

      function updateScore() {
        scoreDisplay.textContent = `Score: ${Math.floor(score)}`;
      }

      function autoClick() {
        score += autoClicker * multiplier;
        updateScore();
      }
      setInterval(autoClick, 1000);

      function ascend() {
        multiplier += 0.1;
        ascensionMultiplierDisplay.textContent = `Ascension Multiplier: ${multiplier.toFixed(1)}x`;
        alert('Ascended! Multiplier increased.');
      }

      function setBackgroundColor(color) {
        document.body.style.backgroundColor = color;
      }

      function enterCode() {
        const code = prompt('Enter a code:');
        if (code === 'admin') {
          clickPower = 10000;
          document.body.style.backgroundImage = "url('https://i.kym-cdn.com/photos/images/original/002/735/702/1e5.jpeg')";
          document.body.style.backgroundSize = '100% 100%';  // Make the background stretch to fit the screen
          document.body.style.backgroundPosition = 'center';  // Center the background image
          document.body.style.backgroundAttachment = 'fixed';  // Keep the background fixed
          adminText.style.display = 'block'; // Show the "ADMIN JAKE AND MILES" text
          alert('Code accepted! 10,000 clicks per click and special background.');
        } else {
          alert('Invalid code!');
        }
      }

      // Event listener for keyboard input
      document.addEventListener('keydown', function(event) {
        // Trigger click on any key press
        incrementScore();
      });
    </script>
  </body>
</html>
