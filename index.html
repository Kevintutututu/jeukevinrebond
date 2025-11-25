<!doctype html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu Kevin</title>
  <style>
    body {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100vh; /* Utilisation de vh pour s'assurer que ça prend l'écran */
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
      cursor: none; /* Cache le curseur pour plus d'immersion */
    }

    .ball {
      position: absolute;
      width: 20px;
      height: 20px;
      background: #ff6b6b;
      border-radius: 50%;
      box-shadow: 0 0 15px rgba(255, 107, 107, 0.8);
      transform: translate(-50%, -50%); /* Pour centrer la balle sur ses coordonnées */
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
      cursor: default; /* Réaffiche le curseur */
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
</head>
<body>
  <div class="game-wrapper">
    <h1 class="title" id="gameTitle">Jeu Kevin</h1>
    <div class="score-container">
      <div class="score-item">
        <div class="score-label">Score</div>
        <div class="score-value" id="currentScore">0</div>
      </div>
      <div class="score-item">
        <div class="score-label">Meilleur</div>
        <div class="score-value" id="bestScore">0</div>
      </div>
    </div>
    <div class="game-canvas" id="gameCanvas">
      <div class="ball" id="ball"></div>
      <div class="paddle" id="paddle"></div>
      <div class="game-over-overlay" id="gameOverOverlay">
        <div class="game-over-text">Game Over!</div>
        <div class="final-score-text">Score: <span id="finalScore">0</span></div>
        <button class="replay-button" id="replayButton">Rejouer</button>
      </div>
    </div>
    <div class="instructions">Déplacez votre souris pour contrôler la barre</div>
  </div>

  <script>
    // Configuration de base
    const defaultConfig = {
      game_title: "Jeu Kevin",
      primary_color: "#00d4ff",
      secondary_color: "#ff6b6b",
      background_color: "#1a1a2e",
      text_color: "#ffffff",
      surface_color: "#0f0f1e"
    };

    const gameCanvas = document.getElementById('gameCanvas');
    const ball = document.getElementById('ball');
    const paddle = document.getElementById('paddle');
    const currentScoreEl = document.getElementById('currentScore');
    const bestScoreEl = document.getElementById('bestScore');
    const gameOverOverlay = document.getElementById('gameOverOverlay');
    const finalScoreEl = document.getElementById('finalScore');
    const replayButton = document.getElementById('replayButton');

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

    // Gestion de la souris
    gameCanvas.addEventListener('mousemove', (e) => {
      const rect = gameCanvas.getBoundingClientRect();
      const mouseX = e.clientX - rect.left;
      // 100 est la largeur du paddle, on centre donc avec -50
      paddleX = Math.max(0, Math.min(mouseX - 50, gameCanvas.offsetWidth - 100));
      paddle.style.left = paddleX + 'px';
    });

    function gameLoop() {
      if (!isGameRunning) return;

      ballX += ballSpeedX;
      ballY += ballSpeedY;

      // Rebond gauche/droite
      if (ballX <= 0 || ballX >= gameCanvas.offsetWidth - 20) {
        ballSpeedX = -ballSpeedX;
      }

      // Rebond haut
      if (ballY <= 0) {
        ballSpeedY = -ballSpeedY;
      }

      // Logique de collision et rebond
      const ballRadius = 10;
      const ballCenterX = ballX + ballRadius;
      const ballCenterY = ballY + ballRadius;
      
      const paddleWidth = 100;
      const paddleHeight = 15;
      const paddleTop = gameCanvas.offsetHeight - (10 + paddleHeight); // 10 est le bottom css
      
      // Collision Paddle
      if (ballCenterY + ballRadius >= paddleTop && 
          ballY < paddleTop + paddleHeight && // Empêche de traverser
          ballCenterX >= paddleX && 
          ballCenterX <= paddleX + paddleWidth &&
          ballSpeedY > 0) {
        
        // On remonte la balle pour éviter qu'elle colle
        ballY = paddleTop - 20; 
        ballSpeedY = -Math.abs(ballSpeedY);
        
        score++;
        currentScoreEl.textContent = score;
        
        // Accélération légère
        ballSpeedX *= 1.05;
        ballSpeedY *= 1.05;
        
        // Effet d'angle selon l'endroit où on tape la barre
        const hitPosition = (ballCenterX - paddleX) / paddleWidth;
        const angle = (hitPosition - 0.5) * 6; // Facteur d'angle ajusté
        ballSpeedX += angle;
      }

      // Perdu (bas de l'écran)
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
      gameCanvas.style.cursor = 'default'; // Réaffiche le curseur
    }

    function resetGame() {
      ballX = 300;
      ballY = 200;
      ballSpeedX = 3;
      ballSpeedY = 3;
      // On reset la vitesse X aléatoirement gauche ou droite pour varier
      if(Math.random() > 0.5) ballSpeedX = -3;

      paddleX = 250;
      score = 0;
      isGameRunning = true;
      
      currentScoreEl.textContent = score;
      gameOverOverlay.classList.remove('active');
      gameCanvas.style.cursor = 'none'; // Cache le curseur
      
      ball.style.left = ballX + 'px';
      ball.style.top = ballY + 'px';
      paddle.style.left = paddleX + 'px';
      
      gameLoop();
    }

    replayButton.addEventListener('click', resetGame);

    // Initialisation
    ball.style.left = ballX + 'px';
    ball.style.top = ballY + 'px';
    paddle.style.left = paddleX + 'px';
    gameLoop();

  </script>
</body>
</html>
