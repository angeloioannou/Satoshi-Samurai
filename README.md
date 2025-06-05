<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Satoshi Samurai 🥷</title>
  <style>
    body { margin: 0; background: #111; color: #fff; font-family: sans-serif; text-align: center; }
    canvas { display: block; margin: auto; background: radial-gradient(#000, #1a1a1a); border: 5px solid gold; }
    #hud { padding: 10px; font-size: 18px; }
  </style>
</head>
<body>
  <div id="hud">🪙 BTC: 0 | 🗺️ Level: Tokyo | 🎯 Goal: Find Satoshi</div>
  <canvas id="gameCanvas" width="400" height="600"></canvas>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    let score = 0;
    const samurai = { x: 180, y: 550, w: 40, h: 20 };
    const coins = [];
    const scrolls = [];
    let level = 1;
    const levels = ["Tokyo", "New York", "London", "Zug", "Cypher Domain"];

    function drawSamurai() {
      ctx.fillStyle = "gold";
      ctx.fillRect(samurai.x, samurai.y, samurai.w, samurai.h);
    }

    function drawCoin(c) {
      ctx.beginPath();
      ctx.arc(c.x, c.y, 10, 0, Math.PI * 2);
      ctx.fillStyle = "#0f0";
      ctx.fill();
      ctx.closePath();
    }

    function spawnCoin() {
      coins.push({ x: Math.random() * 380 + 10, y: 0, speed: 2 + Math.random() * 3 });
    }

    function updateCoins() {
      coins.forEach(c => c.y += c.speed);
    }

    function detectCatches() {
      coins.forEach((c, i) => {
        if (c.y >= samurai.y && c.x >= samurai.x && c.x <= samurai.x + samurai.w) {
          score += 1;
          coins.splice(i, 1);
          if (score % 20 === 0) unlockLevel();
          updateHUD();
        }
      });
    }

    function unlockLevel() {
      level++;
      if (level < levels.length) {
        alert("🗝️ Scroll Unlocked: Level " + levels[level]);
      } else {
        alert("🏆 You found Satoshi’s Vault with 1M BTC!");
      }
    }

    function updateHUD() {
      document.getElementById("hud").innerText = `🪙 BTC: ${score} | 🗺️ Level: ${levels[level] || 'Satoshi Vault'} | 🎯 Goal: Find Satoshi`;
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawSamurai();
      coins.forEach(drawCoin);
      updateCoins();
      detectCatches();
      requestAnimationFrame(gameLoop);
    }

    document.addEventListener("keydown", e => {
      if (e.key === "ArrowLeft") samurai.x -= 20;
      if (e.key === "ArrowRight") samurai.x += 20;
      samurai.x = Math.max(0, Math.min(samurai.x, canvas.width - samurai.w));
    });

    setInterval(spawnCoin, 800);
    gameLoop();
  </script>
</body>
</html>
