
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Plinko Mejorado</title>
  <style>
    body {
      background: #111;
      color: #fff;
      font-family: Arial, sans-serif;
      text-align: center;
    }
    #board {
      position: relative;
      width: 300px;
      height: 400px;
      margin: 30px auto;
      background: #333;
      border-radius: 10px;
      box-shadow: 0 0 20px #000;
    }
    .peg {
      position: absolute;
      width: 10px;
      height: 10px;
      background: #fff;
      border-radius: 50%;
    }
    .ball {
      position: absolute;
      width: 14px;
      height: 14px;
      background: red;
      border-radius: 50%;
      z-index: 2;
    }
    .slot {
      position: absolute;
      bottom: 0;
      width: 50px;
      height: 30px;
      background: #4caf50;
      border-top: 2px solid #000;
      font-weight: bold;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    #score, #history {
      font-size: 18px;
      margin-top: 10px;
    }
    button {
      margin: 5px;
      padding: 8px 15px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <h1>Plinko Mejorado</h1>
  <button onclick="dropBall()">Soltar Bola</button>
  <button onclick="resetGame()">Reiniciar Juego</button>
  <div id="score">Puntuación: 0</div>
  <div id="history">Historial: Ninguno</div>
  <div id="board"></div>

  <!-- Sonidos -->
  <audio id="pegSound" src="https://freesound.org/data/previews/66/66717_931655-lq.mp3"></audio>
  <audio id="slotSound" src="https://freesound.org/data/previews/103/103046_861714-lq.mp3"></audio>

  <script>
    const board = document.getElementById('board');
    const scoreDisplay = document.getElementById('score');
    const historyDisplay = document.getElementById('history');
    const pegSound = document.getElementById('pegSound');
    const slotSound = document.getElementById('slotSound');
    let score = 0;
    let history = [];

    // Crear clavijas
    const pegSpacingX = 50;
    const pegSpacingY = 40;

    for (let y = 0; y < 7; y++) {
      for (let x = 0; x < 6; x++) {
        const peg = document.createElement('div');
        peg.classList.add('peg');
        peg.style.top = `${y * pegSpacingY + 20}px`;
        peg.style.left = `${(x * pegSpacingX) + (y % 2 === 0 ? 25 : 0)}px`;
        board.appendChild(peg);
      }
    }

    // Crear ranuras
    const slotValues = [10, 50, 100, 50, 10, 25];
    for (let i = 0; i < 6; i++) {
      const slot = document.createElement('div');
      slot.classList.add('slot');
      slot.style.left = `${i * 50}px`;
      slot.innerText = slotValues[i];
      board.appendChild(slot);
    }

    function dropBall() {
      const ball = document.createElement('div');
      ball.classList.add('ball');
      let x = 130;
      let y = 0;
      ball.style.left = `${x}px`;
      ball.style.top = `${y}px`;
      board.appendChild(ball);

      const interval = setInterval(() => {
        y += 8;
        if (y % 40 === 0 && y < 280) {
          const direction = Math.random() < 0.5 ? -1 : 1;
          x += direction * 25;
          pegSound.currentTime = 0;
          pegSound.play();
        }
        if (y >= 360) {
          clearInterval(interval);
          const slotIndex = Math.floor(x / 50);
          const points = slotValues[slotIndex] || 0;
          score += points;
          scoreDisplay.textContent = `Puntuación: ${score}`;
          history.push(points);
          historyDisplay.textContent = `Historial: ${history.join(', ')}`;
          slotSound.currentTime = 0;
          slotSound.play();
        }
        ball.style.top = `${y}px`;
        ball.style.left = `${x}px`;
      }, 50);
    }

    function resetGame() {
      score = 0;
      history = [];
      scoreDisplay.textContent = 'Puntuación: 0';
      historyDisplay.textContent = 'Historial: Ninguno';
      const balls = document.querySelectorAll('.ball');
      balls.forEach(b => b.remove());
    }
  </script>
</body>
</html>
