<!doctype html>
<html lang="fr">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu Kevin</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
      font-family: 'Segoe UI', Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }

    .game-wrapper {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      position: relative;
    }

    .title {
      font-size: 48px;
      font-weight: bold;
      color: #00d4ff;
      margin-bottom: 20px;
      text-shadow: 0 0 20px rgba(0, 212, 255, 0.5);
    }

    .score-container {
      display: flex;
      gap: 40px;
      margin-bottom: 20px;
    }

    .score-item {
      text-align: center;
    }

    .score-label {
      font-size: 14px;
      color: #a0a0a0;
      text-transform: uppercase;
      letter-spacing: 2px;
      margin-bottom: 5px;
    }

    .score-value {
      font-size: 32px;
      font-weight: bold;
      color: #ffffff;
    }

    .game-canvas {
      position: relative;
      width: 600px;
      height: 400px;
      background: #0f0f1e;
      border: 3px solid #00d4ff;
      border-radius: 10px;
      box-shadow: 0 0 30px rgba(0, 212, 255, 0.3);
      overflow: hidden;
      cursor: default;
    }

    .ball {
      position: absolute;
      width: 20px;
      height: 20px;
      background: #ff6b6b;
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(255, 107, 107, 0.8);
    }

    .paddle {
      position: absolute;
      bottom: 10px;
      width: 100px;
      height: 15px;
      background: #00d4ff;
      border-radius: 8px;
      box-shadow: 0 0 15px rgba(0, 212, 255, 0.8);
    }

    .game-over-overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.85);
      display: none;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap: 20px;
    }

    .game-over-overlay.active {
      display: flex;
    }

    .game-over-text {
      font-size: 48px;
      font-weight: bold;
      color: #ff6b6b;
      text-shadow: 0 0 20px rgba(255, 107, 107, 0.5);
    }

    .final-score-text {
      font-size: 24px;
      color: #ffffff;
    }

    .replay-button {
      padding: 15px 40px;
      font-size: 20px;
      font-weight: bold;
      color: #0f0f1e;
      background: #00d4ff;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      box-shadow: 0 0 20px rgba(0, 212, 255, 0.5);
      transition: all 0.3s ease;
    }

    .replay-button:hover {
      background: #00b8e6;
      transform: scale(1.05);
    }

    .instructions {
      margin-top: 20px;
      font-size: 16px;
      color: #a0a0a0;
      text-align: center;
    }

    @media (max-width: 700px) {
      .game-canvas {
        width: 90%;
        height: 60%;
      }

      .title {
        font-size: 32px;
      }
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="game-wrapper">
   <h1 class="title" id="gameTitle">Jeu Kevin</h1>
   <div class="score-container">
    <div class="score-item">
     <div class="score-label">
      Score
     </div>
     <div class="score-value" id="currentScore">
      0
     </div>
    </div>
    <div class="score-item">
     <div class="score-label">
      Meilleur
     </div>
     <div class="score-value" id="bestScore">
      0
     </div>
    </div>
   </div>
   <div class="game-canvas" id="gameCanvas">
    <div class="ball" id="ball"></div>
    <div class="paddle" id="paddle"></div>
    <div class="game-over-overlay" id="gameOverOverlay">
     <div class="game-over-text">
      Game Over!
     </div>
     <div class="final-score-text">
      Score: <span id="finalScore">0</span>
     </div><button class="replay-button" id="replayButton">Rejouer</button>
    </div>
   </div>
   <div class="instructions">
    Déplacez votre souris pour contrôler la barre
   </div>
  </div>
  <script>
    const defaultConfig = {
      game_title: "Jeu Kevin",
      play_button_text: "Rejouer",
      primary_color: "#00d4ff",
      secondary_color: "#ff6b6b",
      background_color: "#1a1a2e",
      text_color: "#ffffff",
      surface_color: "#0f0f1e",
      font_family: "Segoe UI",
      font_size: 16
    };

    let config = {};

    const gameCanvas = document.getElementById('gameCanvas');
    const ball = document.getElementById('ball');
    const paddle = document.getElementById('paddle');
    const currentScoreEl = document.getElementById('currentScore');
    const bestScoreEl = document.getElementById('bestScore');
    const gameOverOverlay = document.getElementById('gameOverOverlay');
    const finalScoreEl = document.getElementById('finalScore');
    const replayButton = document.getElementById('replayButton');
    const gameTitle = document.getElementById('gameTitle');

    let ballX = 300;
    let ballY = 200;
    let ballSpeedX = 3;
    let ballSpeedY = 3;
    let paddleX = 250;
    let score = 0;
    let bestScore = parseInt(localStorage.getItem('bestScore')) || 0;
    let isGameRunning = true;
    let animationId;

    bestScoreEl.textContent = bestScore;

    gameCanvas.addEventListener('mousemove', (e) => {
      const rect = gameCanvas.getBoundingClientRect();
      const mouseX = e.clientX - rect.left;
      paddleX = Math.max(0, Math.min(mouseX - 50, gameCanvas.offsetWidth - 100));
      paddle.style.left = paddleX + 'px';
    });

    function gameLoop() {
      if (!isGameRunning) return;

      ballX += ballSpeedX;
      ballY += ballSpeedY;

      if (ballX <= 0 || ballX >= gameCanvas.offsetWidth - 20) {
        ballSpeedX = -ballSpeedX;
      }

      if (ballY <= 0) {
        ballSpeedY = -ballSpeedY;
      }

      const ballRadius = 10;
      const ballCenterX = ballX + ballRadius;
      const ballCenterY = ballY + ballRadius;
      const paddleTop = gameCanvas.offsetHeight - 25;
      const paddleBottom = gameCanvas.offsetHeight - 10;
      const paddleLeft = paddleX;
      const paddleRight = paddleX + 100;

      if (ballCenterY + ballRadius >= paddleTop && 
          ballCenterY - ballRadius <= paddleBottom &&
          ballCenterX >= paddleLeft && 
          ballCenterX <= paddleRight &&
          ballSpeedY > 0) {
        
        ballY = paddleTop - 20;
        ballSpeedY = -Math.abs(ballSpeedY);
        
        score++;
        currentScoreEl.textContent = score;
        
        ballSpeedX *= 1.01;
        ballSpeedY *= 1.01;
        
        const hitPosition = (ballCenterX - paddleLeft) / 100;
        const angle = (hitPosition - 0.5) * 0.3;
        ballSpeedX += angle * Math.abs(ballSpeedY);
      }

      if (ballY >= gameCanvas.offsetHeight) {
        endGame();
        return;
      }

      ball.style.left = ballX + 'px';
      ball.style.top = ballY + 'px';

      animationId = requestAnimationFrame(gameLoop);
    }

    function endGame() {
      isGameRunning = false;
      finalScoreEl.textContent = score;
      
      if (score > bestScore) {
        bestScore = score;
        localStorage.setItem('bestScore', bestScore);
        bestScoreEl.textContent = bestScore;
      }
      
      gameOverOverlay.classList.add('active');
    }

    function resetGame() {
      ballX = 300;
      ballY = 200;
      ballSpeedX = 3;
      ballSpeedY = 3;
      paddleX = 250;
      score = 0;
      isGameRunning = true;
      
      currentScoreEl.textContent = score;
      gameOverOverlay.classList.remove('active');
      
      ball.style.left = ballX + 'px';
      ball.style.top = ballY + 'px';
      paddle.style.left = paddleX + 'px';
      
      gameLoop();
    }

    replayButton.addEventListener('click', resetGame);

    async function onConfigChange(newConfig) {
      const titleElement = document.getElementById('gameTitle');
      const replayBtn = document.getElementById('replayButton');
      const primaryColor = newConfig.primary_color || defaultConfig.primary_color;
      const secondaryColor = newConfig.secondary_color || defaultConfig.secondary_color;
      const backgroundColor = newConfig.background_color || defaultConfig.background_color;
      const textColor = newConfig.text_color || defaultConfig.text_color;
      const surfaceColor = newConfig.surface_color || defaultConfig.surface_color;
      const customFont = newConfig.font_family || defaultConfig.font_family;
      const baseFontStack = 'Arial, sans-serif';
      const baseSize = newConfig.font_size || defaultConfig.font_size;

      titleElement.textContent = newConfig.game_title || defaultConfig.game_title;
      replayBtn.textContent = newConfig.play_button_text || defaultConfig.play_button_text;

      document.body.style.background = `linear-gradient(135deg, ${backgroundColor} 0%, ${backgroundColor} 100%)`;
      document.body.style.fontFamily = `${customFont}, ${baseFontStack}`;

      titleElement.style.color = primaryColor;
      titleElement.style.textShadow = `0 0 20px ${primaryColor}80`;
      titleElement.style.fontFamily = `${customFont}, ${baseFontStack}`;
      titleElement.style.fontSize = `${baseSize * 3}px`;

      gameCanvas.style.background = surfaceColor;
      gameCanvas.style.borderColor = primaryColor;
      gameCanvas.style.boxShadow = `0 0 30px ${primaryColor}4D`;

      paddle.style.background = primaryColor;
      paddle.style.boxShadow = `0 0 15px ${primaryColor}CC`;

      ball.style.background = secondaryColor;
      ball.style.boxShadow = `0 0 15px ${secondaryColor}CC`;

      replayBtn.style.background = primaryColor;
      replayBtn.style.color = surfaceColor;
      replayBtn.style.boxShadow = `0 0 20px ${primaryColor}80`;
      replayBtn.style.fontFamily = `${customFont}, ${baseFontStack}`;
      replayBtn.style.fontSize = `${baseSize * 1.25}px`;

      const scoreValues = document.querySelectorAll('.score-value');
      scoreValues.forEach(el => {
        el.style.color = textColor;
        el.style.fontFamily = `${customFont}, ${baseFontStack}`;
        el.style.fontSize = `${baseSize * 2}px`;
      });

      const scoreLabels = document.querySelectorAll('.score-label');
      scoreLabels.forEach(el => {
        el.style.fontFamily = `${customFont}, ${baseFontStack}`;
        el.style.fontSize = `${baseSize * 0.875}px`;
      });

      document.querySelector('.instructions').style.fontFamily = `${customFont}, ${baseFontStack}`;
      document.querySelector('.instructions').style.fontSize = `${baseSize}px`;

      const gameOverText = document.querySelector('.game-over-text');
      gameOverText.style.color = secondaryColor;
      gameOverText.style.textShadow = `0 0 20px ${secondaryColor}80`;
      gameOverText.style.fontFamily = `${customFont}, ${baseFontStack}`;
      gameOverText.style.fontSize = `${baseSize * 3}px`;

      const finalScoreText = document.querySelector('.final-score-text');
      finalScoreText.style.color = textColor;
      finalScoreText.style.fontFamily = `${customFont}, ${baseFontStack}`;
      finalScoreText.style.fontSize = `${baseSize * 1.5}px`;
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities: (config) => ({
          recolorables: [
            {
              get: () => config.background_color || defaultConfig.background_color,
              set: (value) => {
                config.background_color = value;
                window.elementSdk.setConfig({ background_color: value });
              }
            },
            {
              get: () => config.surface_color || defaultConfig.surface_color,
              set: (value) => {
                config.surface_color = value;
                window.elementSdk.setConfig({ surface_color: value });
              }
            },
            {
              get: () => config.text_color || defaultConfig.text_color,
              set: (value) => {
                config.text_color = value;
                window.elementSdk.setConfig({ text_color: value });
              }
            },
            {
              get: () => config.primary_color || defaultConfig.primary_color,
              set: (value) => {
                config.primary_color = value;
                window.elementSdk.setConfig({ primary_color: value });
              }
            },
            {
              get: () => config.secondary_color || defaultConfig.secondary_color,
              set: (value) => {
                config.secondary_color = value;
                window.elementSdk.setConfig({ secondary_color: value });
              }
            }
          ],
          borderables: [],
          fontEditable: {
            get: () => config.font_family || defaultConfig.font_family,
            set: (value) => {
              config.font_family = value;
              window.elementSdk.setConfig({ font_family: value });
            }
          },
          fontSizeable: {
            get: () => config.font_size || defaultConfig.font_size,
            set: (value) => {
              config.font_size = value;
              window.elementSdk.setConfig({ font_size: value });
            }
          }
        }),
        mapToEditPanelValues: (config) => new Map([
          ["game_title", config.game_title || defaultConfig.game_title],
          ["play_button_text", config.play_button_text || defaultConfig.play_button_text]
        ])
      });

      config = window.elementSdk.config;
    }

    gameLoop();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a40024c86df00ce',t:'MTc2NDA2MTc4NS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
