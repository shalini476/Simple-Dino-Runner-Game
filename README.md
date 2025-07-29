# Simple-Dino-Runner-Game
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple Dino Runner</title>
<style>
  body {
    background: #f7f7f7;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    font-family: Arial, sans-serif;
  }
  #game {
    position: relative;
    width: 600px;
    height: 150px;
    border: 2px solid #333;
    background: #fff;
    overflow: hidden;
  }
  #dino {
    position: absolute;
    bottom: 0;
    left: 50px;
    width: 40px;
    height: 40px;
    background: #555;
    border-radius: 5px;
  }
  #obstacle {
    position: absolute;
    bottom: 0;
    right: 0;
    width: 20px;
    height: 40px;
    background: #b22222;
    border-radius: 3px;
  }
  #score {
    position: absolute;
    top: 5px;
    right: 10px;
    font-weight: bold;
    font-size: 18px;
  }
</style>
</head>
<body>
<div id="game">
  <div id="dino"></div>
  <div id="obstacle"></div>
  <div id="score">Score: 0</div>
</div>

<script>
  const dino = document.getElementById('dino');
  const obstacle = document.getElementById('obstacle');
  const scoreDisplay = document.getElementById('score');

  let isJumping = false;
  let jumpHeight = 0;
  let gravity = 0.8;
  let position = 0;

  let obstacleSpeed = 5;
  let score = 0;

  function jump() {
    if (isJumping) return;
    isJumping = true;

    let upInterval = setInterval(() => {
      if (position >= 100) {
        clearInterval(upInterval);
        let downInterval = setInterval(() => {
          if (position <= 0) {
            clearInterval(downInterval);
            isJumping = false;
          } else {
            position -= 5;
            dino.style.bottom = position + 'px';
          }
        }, 20);
      } else {
        position += 5;
        dino.style.bottom = position + 'px';
      }
    }, 20);
  }

  document.addEventListener('keydown', (e) => {
    if (e.code === 'Space') {
      jump();
    }
  });

  function moveObstacle() {
    let obstacleLeft = parseInt(window.getComputedStyle(obstacle).getPropertyValue('right'));
    if (obstacleLeft > 600) {
      obstacle.style.right = '-20px';
      // Increase speed and score
      obstacleSpeed += 0.1;
      score++;
      scoreDisplay.textContent = 'Score: ' + score;
    } else {
      obstacle.style.right = (obstacleLeft + obstacleSpeed) + 'px';
    }

    // Collision detection
    let dinoBottom = position;
    let dinoLeft = 50;
    let dinoWidth = 40;
    let dinoHeight = 40;

    let obstaclePos = 600 - obstacleLeft;
    let obstacleWidth = 20;
    let obstacleHeight = 40;

    if (
      obstaclePos < dinoLeft + dinoWidth &&
      obstaclePos + obstacleWidth > dinoLeft &&
      dinoBottom < obstacleHeight
    ) {
      alert('Game Over! Your score: ' + score);
      // Reset game
      score = 0;
      obstacleSpeed = 5;
      scoreDisplay.textContent = 'Score: ' + score;
      obstacle.style.right = '0px';
      position = 0;
      dino.style.bottom = '0px';
    }
  }

  setInterval(moveObstacle, 20);
</script>
</body>
</html>
