<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
  <title>Колесо Удачи</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      height: 100vh;
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #121212;
      color: #ffffff;
      overflow: hidden;
    }

    h1 {
      color: #ffffff;
      margin: 15px 0 8px;
      text-align: center;
      font-size: 1.5rem;
    }

    main {
      flex-grow: 1;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 5px;
      overflow: hidden;
    }

    button {
      background-color: #333;
      color: #fff;
      border: none;
      padding: 8px 16px;
      margin-top: 5px;
      cursor: pointer;
      border-radius: 5px;
      font-size: 14px;
      transition: background-color 0.3s;
      user-select: none;
    }

    button:hover {
      background-color: #555;
    }

    /* Здесь уменьшен margin-top с 5px на 0 */
    .wheel-container {
      position: relative;
      margin-top: 0;
      text-align: center;
      width: 280px;
      height: 280px;
    }

    canvas {
      border: 2px solid #ffd700;
      border-radius: 50%;
      background-color: #222;
      display: block;
      margin: 0 auto;
      width: 280px;
      height: 280px;
    }

    .arrow {
      position: absolute;
      top: -15px;
      left: 50%;
      transform: translateX(-50%);
      width: 0;
      height: 0;
      border-left: 12px solid transparent;
      border-right: 12px solid transparent;
      border-bottom: 25px solid red;
      z-index: 2;
    }

    #result {
      margin-top: 15px;
      font-size: 1.1em;
      color: #ffffff;
      min-height: 1.2em;
      word-wrap: break-word;
      max-width: 280px;
    }

    nav.tabs {
      display: flex;
      justify-content: space-around;
      background-color: #222;
      border-top: 1px solid #444;
      padding: 8px 0;
      position: fixed;
      bottom: 0;
      width: 100%;
      max-width: 480px;
      margin: 0 auto;
      left: 0;
      right: 0;
      z-index: 10;
    }

    nav.tabs button {
      flex-grow: 1;
      background: none;
      border: none;
      color: #aaa;
      font-weight: bold;
      font-size: 13px;
      cursor: pointer;
      padding: 8px 0;
      transition: color 0.3s, border-top 0.3s;
      border-top: 3px solid transparent;
      user-select: none;
    }

    nav.tabs button.active {
      color: #ffd700;
      border-top: 3px solid #ffd700;
    }

    @media (max-width: 320px) {
      .wheel-container {
        width: 240px;
        height: 240px;
      }
      canvas {
        width: 240px;
        height: 240px;
      }
      #result {
        max-width: 240px;
      }
      button {
        font-size: 13px;
        padding: 6px 12px;
      }
    }
  </style>
</head>
<body>
  <h1>Колесо Удачи</h1>

  <main>
    <section id="tab-roulette">
      <div class="wheel-container">
        <div class="arrow"></div>
        <canvas id="wheel" width="280" height="280"></canvas>
        <button id="spin">Крутить колесо!</button>
        <div id="result"></div>
      </div>
    </section>

    <section id="tab-inventory" style="display:none; text-align: center; color: #ccc;">
      <p>Инвентарь пока пуст.</p>
    </section>

    <section id="tab-profile" style="display:none; text-align: center; color: #ccc;">
      <p>Профиль пока пуст.</p>
    </section>
  </main>

  <nav class="tabs">
    <button class="tab-button active" data-tab="roulette">Рулетка</button>
    <button class="tab-button" data-tab="inventory">Инвентарь</button>
    <button class="tab-button" data-tab="profile">Профиль</button>
  </nav>

  <script>
    // Переключение вкладок
    const tabs = {
      roulette: document.getElementById('tab-roulette'),
      inventory: document.getElementById('tab-inventory'),
      profile: document.getElementById('tab-profile'),
    };
    const tabButtons = document.querySelectorAll('.tab-button');

    function switchTab(tabName) {
      Object.entries(tabs).forEach(([name, elem]) => {
        elem.style.display = (name === tabName) ? 'block' : 'none';
      });
      tabButtons.forEach(btn => {
        btn.classList.toggle('active', btn.dataset.tab === tabName);
      });
    }

    tabButtons.forEach(button => {
      button.addEventListener('click', () => {
        switchTab(button.dataset.tab);
      });
    });

    switchTab('roulette');

    // Колесо удачи
    const canvas = document.getElementById('wheel');
    const ctx = canvas.getContext('2d');
    const resultDiv = document.getElementById('result');
    const prizes = [
      { name: 'Ничего', percent: 90, color: '#FF6347', resultText: 'Вы выиграли: Ничего' },
      { name: 'Lol поп', percent: 5, color: '#7CFC00', resultText: 'Вы выиграли: Lol Pop' },
      { name: '1.000 Stars', percent: 5, color: '#1E90FF', resultText: 'Вы выиграли: 1.000 Stars' },
    ];

    function drawWheel(rotation = 0) {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      const centerX = canvas.width / 2;
      const centerY = canvas.height / 2;
      const radius = canvas.width / 2;
      let startAngle = rotation;

      const totalPercent = prizes.reduce((sum, p) => sum + p.percent, 0);

      for (let prize of prizes) {
        const sliceAngle = (prize.percent / totalPercent) * 2 * Math.PI;

        // Рисуем сектор
        ctx.beginPath();
        ctx.moveTo(centerX, centerY);
        ctx.arc(centerX, centerY, radius, startAngle, startAngle + sliceAngle);
        ctx.closePath();
        ctx.fillStyle = prize.color;
        ctx.shadowColor = "rgba(0, 0, 0, 0.3)";
        ctx.shadowBlur = 10;
        ctx.fill();

        // Запоминаем углы для каждого приза (для определения победителя)
        prize.startAngle = startAngle;
        prize.endAngle = startAngle + sliceAngle;

        // Добавляем текст приза
        ctx.save();
        ctx.translate(centerX, centerY);
        ctx.rotate(startAngle + sliceAngle / 2);
        ctx.textAlign = "right";
        ctx.fillStyle = "#fff";
        ctx.font = "bold 14px Arial";
        ctx.fillText(prize.name, radius - 10, 0);
        ctx.restore();

        startAngle += sliceAngle;
      }
    }

    let isSpinning = false;

    document.getElementById("spin").addEventListener("click", () => {
      if (isSpinning) return;
      resultDiv.textContent = "";
      isSpinning = true;

      const spinDegree = Math.random() * 360 + 1440;
      const duration = 4000;
      const startTime = performance.now();

      function animate() {
        const elapsed = performance.now() - startTime;
        const progress = Math.min(elapsed / duration, 1);
        const easeProgress = 1 - Math.pow(1 - progress, 3);
        const animatedAngle = (spinDegree * easeProgress * Math.PI) / 180;

        drawWheel(animatedAngle);

        if (progress < 1) {
          requestAnimationFrame(animate);
        } else {
          // Определяем приз по позиции стрелки
          const normalizedAngle = (animatedAngle % (2 * Math.PI));
          let prizeWon = null;
          for (let prize of prizes) {
            let start = prize.startAngle < 0 ? prize.startAngle + 2 * Math.PI : prize.startAngle;
            let end = prize.endAngle < 0 ? prize.endAngle + 2 * Math.PI : prize.endAngle;
            if (normalizedAngle >= start && normalizedAngle < end) {
              prizeWon = prize;
              break;
            }
          }
          if (!prizeWon) {
            // Можно взять первую если не сработало
            prizeWon = prizes[0];
          }

          resultDiv.textContent = prizeWon.resultText;
          isSpinning = false;
        }
      }

      requestAnimationFrame(animate);
    });

    // Нарисовать начальное колесо
    drawWheel(0);
  </script>
</body>
</html>


