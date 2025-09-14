<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Колесо Удачи — улучшенное</title>
<style>
:root {
  --bg-1: #0f1724;
  --bg-2: #071028;
  --panel: rgba(255, 255, 255, 0.03);
  --text: #EDF2F7;
  --muted: #9AA4B2;
  --accent: #60A5FA;
  --nav-height: 64px;
  --wheel-size: 460px;
}
html, body {
  height: 100%;
  margin: 0;
  font-family: Inter, "Segoe UI", Roboto, Arial, sans-serif;
  background: linear-gradient(180deg, var(--bg-1), var(--bg-2) 60%);
  color: var(--text);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
main {
  max-width: 860px;
  margin: 18px auto 88px;
  padding: 14px;
  box-sizing: border-box;
}
h1 {
  margin: 6px 0 14px;
  font-size: 1.35rem;
  text-align: center;
  color: var(--text);
  letter-spacing: 0.4px;
  text-shadow: 0 2px 8px rgba(0,0,0,0.6);
}
.wheel-wrap {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  gap: 18px;
  width: 100%;
}
.wheel-container {
  width: var(--wheel-size);
  height: var(--wheel-size);
  position: relative;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
}
canvas#wheel {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  display: block;
  box-shadow: 0 8px 30px rgba(2,6,23,0.6), inset 0 6px 18px rgba(255,255,255,0.03);
  background: transparent;
  image-rendering: auto;
}
/* стрелка */
.arrow {
  position: absolute;
  top: -8px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 8;
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
  border-top: 32px solid #FF6B6B;
  filter: drop-shadow(0 4px 8px rgba(0,0,0,0.45));
  border-top-left-radius: 2px;
  border-top-right-radius: 2px;
}
.arrow::after {
  content: '';
  position: absolute;
  left: 50%;
  transform: translateX(-50%) translateY(-3px);
  top: 0;
  width: 4px;
  height: 8px;
  background: rgba(0, 0, 0, 0.06);
  border-radius: 2px;
}
.controls {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  margin-top: 14px;
}
.control-row {
  display: flex;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
  justify-content: center;
}
#spin {
  appearance: none;
  -webkit-appearance: none;
  border: 0;
  background: linear-gradient(180deg, #2b3a4a, #11161b);
  color: var(--text);
  padding: 14px 20px;
  border-radius: 12px;
  font-weight: 800;
  cursor: pointer;
  font-size: 16px;
  user-select: none;
  box-shadow: 0 6px 18px rgba(2,6,23,0.6), inset 0 -6px 18px rgba(255,255,255,0.02);
  transition: transform .12s ease, opacity .12s ease, box-shadow .12s;
  width: 260px;
  max-width: 92%;
  text-align: center;
  letter-spacing: 0.6px;
}
#spin:active {
  transform: translateY(2px) scale(.996);
}
#spin[disabled] {
  opacity: .6;
  cursor: not-allowed;
  transform: none;
}
#spin.spin-active {
  box-shadow: 0 10px 34px rgba(2,6,23,0.7);
}
#result {
  color: var(--muted);
  min-height: 1.2em;
  text-align: center;
  padding: 6px 8px;
  max-width: 92%;
  word-break: break-word;
  font-weight: 700;
  font-size: 14px;
}
nav.tabs {
  display: flex;
  justify-content: space-around;
  background: var(--panel);
  border-top: 1px solid rgba(255,255,255,0.03);
  padding: 8px 0;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: var(--nav-height);
  max-width: 860px;
  margin: 0 auto;
  z-index: 20;
  box-sizing: border-box;
}
nav.tabs button {
  flex: 1;
  background: none;
  border: 0;
  color: var(--muted);
  font-weight: 700;
  cursor: pointer;
  font-size: 13px;
  padding: 6px;
  transition: color .18s;
}
nav.tabs button.active {
  color: var(--accent);
}
section.tab-content {
  display: none;
}
section.tab-content.active {
  display: block;
}

/* Мобильная адаптация */
@media (max-width: 768px) {
  :root {
    --wheel-size: 320px;
    --nav-height: 56px;
  }
  main {
    margin: 12px auto 70px;
    padding: 10px;
  }
  h1 {
    font-size: 1.1rem;
    margin: 4px 0 10px;
  }
  .wheel-wrap {
    flex-direction: column;
    gap: 12px;
  }
  .controls {
    margin-top: 10px;
  }
  #spin {
    width: 220px;
    padding: 12px 16px;
    font-size: 14px;
  }
  #result {
    font-size: 12px;
  }
  nav.tabs {
    height: var(--nav-height);
    padding: 6px 0;
  }
  nav.tabs button {
    font-size: 12px;
    padding: 4px;
  }
}
</style>
</head>
<body>
<h1>Колесо Удачи</h1>
<main>
<div class="wheel-wrap">
  <section id="tab-roulette" class="tab-content active" style="width:100%;">
    <div class="wheel-container" id="wheelContainer" aria-label="Колесо удачи" role="img">
      <div class="arrow" aria-hidden="true"></div>
      <canvas id="wheel"></canvas>
    </div>
    <div class="controls" aria-live="polite" aria-atomic="true">
      <div class="control-row">
        <button id="spin" aria-label="Крутить колесо">Крутить колесо!</button>
      </div>
      <div class="control-row" style="margin-top:6px;">
        <div id="result"></div>
      </div>
    </div>
  </section>
  <section id="tab-inventory" class="tab-content" aria-label="Инвентарь">
    <p style="text-align:center; color: var(--muted); margin-top: 40px;">Здесь будет ваш инвентарь.</p>
  </section>
  <section id="tab-profile" class="tab-content" aria-label="Профиль">
    <p style="text-align:center; color: var(--muted); margin-top: 40px;">Здесь будет ваш профиль.</p>
  </section>
</div>
<nav class="tabs" role="tablist" aria-label="Навигация">
  <button class="tab-button active" data-tab="roulette" aria-selected="true" role="tab" id="tab-roulette-btn">Рулетка</button>
  <button class="tab-button" data-tab="inventory" aria-selected="false" role="tab" id="tab-inventory-btn">Инвентарь</button>
  <button class="tab-button" data-tab="profile" aria-selected="false" role="tab" id="tab-profile-btn">Профиль</button>
</nav>
<script>
(() => {
  const canvas = document.getElementById('wheel');
  const ctx = canvas.getContext('2d');
  const spinButton = document.getElementById('spin');
  const resultDiv = document.getElementById('result');

  // Для четкости на HiDPI
  function resizeCanvas() {
    const rect = canvas.getBoundingClientRect();
    const dpr = window.devicePixelRatio || 1;
    canvas.width = rect.width * dpr;
    canvas.height = rect.height * dpr;
    ctx.setTransform(1, 0, 0, 1, 0, 0); // сброс трансформации
    ctx.scale(dpr, dpr);
  }
  resizeCanvas();
  window.addEventListener('resize', resizeCanvas);

  // Призы с цветами и процентами (теперь 6 секторов для красоты)
  const prizes = [
    { name: 'Ничего', percent: 50, colorFrom: '#FF6347', colorTo: '#D9473D', resultText: 'Вы выиграли: Ничего' },
    { name: 'Lol Pop', percent: 15, colorFrom: '#1E90FF', colorTo: '#0F5CFF', resultText: 'Вы выиграли: Lol Pop' },
    { name: '25⭐', percent: 15, colorFrom: '#7CFC00', colorTo: '#55C700', resultText: 'Вы выиграли: 25 Telegram Stars ⭐' },
    { name: '100⭐', percent: 10, colorFrom: '#FFD700', colorTo: '#FFA500', resultText: 'Вы выиграли: 100 Telegram Stars ⭐' },
    { name: '150⭐', percent: 5, colorFrom: '#FF1493', colorTo: '#C71585', resultText: 'Вы выиграли: 150 Telegram Stars ⭐' },
    { name: '300⭐', percent: 5, colorFrom: '#32CD32', colorTo: '#228B22', resultText: 'Вы выиграли: 300 Telegram Stars ⭐' }
  ];

  let currentAngle = 0; // в радианах
  let isSpinning = false;
  let highlightedIndex = null;

  // Звук вращения (Web Audio API)
  let audioCtx;
  let spinOscillator;
  let spinGain;

  function startSpinSound() {
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    spinOscillator = audioCtx.createOscillator();
    spinGain = audioCtx.createGain();
    spinOscillator.type = 'square';
    spinOscillator.frequency.setValueAtTime(150, audioCtx.currentTime);
    spinGain.gain.setValueAtTime(0.02, audioCtx.currentTime);
    spinOscillator.connect(spinGain);
    spinGain.connect(audioCtx.destination);
    spinOscillator.start();
  }

  function stopSpinSound() {
    if (spinOscillator) {
      spinGain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 0.3);
      spinOscillator.stop(audioCtx.currentTime + 0.3);
      spinOscillator.disconnect();
      spinGain.disconnect();
      spinOscillator = null;
      spinGain = null;
    }
  }

  // Звук выигрыша (короткий "чирп")
  function playWinSound() {
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = audioCtx.createOscillator();
    const gainNode = audioCtx.createGain();
    osc.type = 'triangle';
    osc.frequency.setValueAtTime(880, audioCtx.currentTime);
    gainNode.gain.setValueAtTime(0, audioCtx.currentTime);
    gainNode.gain.linearRampToValueAtTime(0.15, audioCtx.currentTime + 0.01);
    gainNode.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + 0.4);
    osc.connect(gainNode);
    gainNode.connect(audioCtx.destination);
    osc.start();
    osc.stop(audioCtx.currentTime + 0.4);
  }

  // Рисуем колесо с улучшенными градиентами, внутренним кругом и подсветкой
  function drawWheel() {
    const rect = canvas.getBoundingClientRect();
    const centerX = rect.width / 2;
    const centerY = rect.height / 2;
    const radius = Math.min(centerX, centerY) - 10;
    const innerRadius = radius * 0.3; // внутренний круг

    ctx.clearRect(0, 0, rect.width, rect.height);

    let startAngle = -Math.PI / 2; // начало сверху
    prizes.forEach((prize, index) => {
      const angle = (prize.percent / 100) * 2 * Math.PI;
      const endAngle = startAngle + angle;

      // Градиент для сектора (радиальный)
      const grad = ctx.createRadialGradient(centerX, centerY, innerRadius, centerX, centerY, radius);
      grad.addColorStop(0, prize.colorFrom);
      grad.addColorStop(0.5, prize.colorFrom);
      grad.addColorStop(1, prize.colorTo);

      ctx.beginPath();
      ctx.moveTo(centerX, centerY);
      ctx.arc(centerX, centerY, radius, startAngle, endAngle);
      ctx.closePath();

      ctx.fillStyle = grad;
      ctx.fill();

      // Подсветка выигравшего сектора (ярче)
      if (highlightedIndex === index) {
        ctx.shadowColor = 'yellow';
        ctx.shadowBlur = 30;
        ctx.lineWidth = 8;
        ctx.strokeStyle = 'yellow';
        ctx.stroke();
        ctx.shadowBlur = 0;
      } else {
        ctx.lineWidth = 2;
        ctx.strokeStyle = '#fff';
        ctx.stroke();
      }

      // Текст по центру сектора с контуром
      const textAngle = startAngle + angle / 2;
      const textRadius = radius * 0.7;
      ctx.save();
      ctx.translate(centerX + textRadius * Math.cos(textAngle), centerY + textRadius * Math.sin(textAngle));
      ctx.rotate(textAngle + Math.PI / 2);
      ctx.fillStyle = '#fff';
      ctx.strokeStyle = '#000';
      ctx.lineWidth = 2;
      ctx.font = 'bold 16px Arial';
      ctx.textAlign = 'center';
      ctx.textBaseline = 'middle';
      ctx.strokeText(prize.name, 0, 0);
      ctx.fillText(prize.name, 0, 0);
      ctx.restore();

      // Сохраняем углы для вычисления результата
      prize._startAngle = startAngle;
      prize._endAngle = endAngle;

      startAngle = endAngle;
    });

    // Внутренний круг с текстурой
    const innerGrad = ctx.createRadialGradient(centerX, centerY, 0, centerX, centerY, innerRadius);
    innerGrad.addColorStop(0, '#fff');
    innerGrad.addColorStop(1, '#ddd');
    ctx.beginPath();
    ctx.arc(centerX, centerY, innerRadius, 0, 2 * Math.PI);
    ctx.fillStyle = innerGrad;
    ctx.fill();
    ctx.strokeStyle = '#aaa';
    ctx.lineWidth = 2;
    ctx.stroke();
  }

  // Вращаем колесо
  function spinWheel() {
    if (isSpinning) return;
    isSpinning = true;
    spinButton.disabled = true;
    spinButton.classList.add('spin-active');
    resultDiv.textContent = '';
    highlightedIndex = null;
    drawWheel();

    // Запускаем звук вращения
    startSpinSound();

    // Генерируем случайную конечную позицию (5-10 оборотов + случайный угол)
    const spins = Math.floor(Math.random() * 6) + 5; // 5-10 оборотов
    const randomAngle = Math.random() * 2 * Math.PI;
    const finalAngle = spins * 2 * Math.PI + randomAngle;

    const duration = 4000; // 4 секунды
    const startTime = performance.now();
    const startAngle = currentAngle;

    function animate(time) {
      const elapsed = time - startTime;
      const progress = Math.min(elapsed / duration, 1);
      // ease out cubic
      const easeOut = 1 - Math.pow(1 - progress, 3);
      currentAngle = startAngle + (finalAngle - startAngle) * easeOut;

      // Рисуем колесо с текущим поворотом
      const rect = canvas.getBoundingClientRect();
      ctx.clearRect(0, 0, rect.width, rect.height);
      ctx.save();
      ctx.translate(rect.width / 2, rect.height / 2);
      ctx.rotate(currentAngle);
      ctx.translate(-rect.width / 2, -rect.height / 2);
      drawWheel();
      ctx.restore();

      if (progress < 1) {
        requestAnimationFrame(animate);
      } else {
        isSpinning = false;
        spinButton.disabled = false;
        spinButton.classList.remove('spin-active');
        stopSpinSound();

        // Определяем выигрышный сектор по стрелке (стрелка вверху, угол π/2)
        const arrowAngle = Math.PI / 2; // стрелка сверху
        let normalizedAngle = currentAngle % (2 * Math.PI);
        if (normalizedAngle < 0) normalizedAngle += 2 * Math.PI;
        // Угол, на который указывает стрелка: (2π - normalizedAngle + arrowAngle) % 2π
        const winAngle = (2 * Math.PI - normalizedAngle + arrowAngle) % (2 * Math.PI);

        let winIndex = -1;
        for (let i = 0; i < prizes.length; i++) {
          if (winAngle >= prizes[i]._startAngle && winAngle < prizes[i]._endAngle) {
            winIndex = i;
            break;
          }
        }
        if (winIndex >= 0) {
          highlightedIndex = winIndex;
          resultDiv.textContent = prizes[winIndex].resultText;
          // Звуковой эффект выигрыша
          playWinSound();
          // Перерисовка с подсветкой
          const rect = canvas.getBoundingClientRect();
          ctx.clearRect(0, 0, rect.width, rect.height);
          ctx.save();
          ctx.translate(rect.width / 2, rect.height / 2);
          ctx.rotate(currentAngle);
          ctx.translate(-rect.width / 2, -rect.height / 2);
          drawWheel();
          ctx.restore();
        }
      }
    }
    requestAnimationFrame(animate);
  }

  // Обработчики вкладок
  const tabButtons = document.querySelectorAll('.tab-button');
  const tabContents = document.querySelectorAll('.tab-content');

  tabButtons.forEach(button => {
    button.addEventListener('click', () => {
      const tab = button.dataset.tab;
      // Убираем активные классы
      tabButtons.forEach(btn => {
        btn.classList.remove('active');
        btn.setAttribute('aria-selected', 'false');
      });
      tabContents.forEach(content => content.classList.remove('active'));
      // Добавляем активный класс
      button.classList.add('active');
      button.setAttribute('aria-selected', 'true');
      document.getElementById(`tab-${tab}`).classList.add('active');
    });
  });

  // Инициализация
  drawWheel();
  spinButton.addEventListener('click', spinWheel);
})();
</script>
</body>
</html>


