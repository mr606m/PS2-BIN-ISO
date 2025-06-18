<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ZombieChaos - Web Version</title>
  <style>
    body {
      margin: 0;
      background: black;
      overflow: hidden;
    }
    canvas {
      display: block;
    }
    #startBtn {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      padding: 20px 40px;
      font-size: 20px;
      background: red;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <button id="startBtn">ابدأ القتال</button>
  <canvas id="gameCanvas"></canvas>

  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const player = {
      x: canvas.width / 2,
      y: canvas.height - 100,
      width: 40,
      height: 40,
      color: 'green',
      speed: 5
    };

    const zombies = [];
    const keys = {};

    document.addEventListener('keydown', e => keys[e.key] = true);
    document.addEventListener('keyup', e => keys[e.key] = false);

    function drawPlayer() {
      ctx.fillStyle = player.color;
      ctx.fillRect(player.x, player.y, player.width, player.height);
    }

    function drawZombies() {
      ctx.fillStyle = 'purple';
      zombies.forEach(z => ctx.fillRect(z.x, z.y, z.width, z.height));
    }

    function movePlayer() {
      if (keys['a'] || keys['ArrowLeft']) player.x -= player.speed;
      if (keys['d'] || keys['ArrowRight']) player.x += player.speed;
    }

    function spawnZombie() {
      const z = {
        x: Math.random() * canvas.width,
        y: -50,
        width: 30,
        height: 30,
        speed: 2 + Math.random() * 2
      };
      zombies.push(z);
    }

    function updateZombies() {
      zombies.forEach(z => z.y += z.speed);
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawPlayer();
      drawZombies();
      movePlayer();
      updateZombies();
      requestAnimationFrame(gameLoop);
    }

    setInterval(spawnZombie, 1000
