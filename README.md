<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Колесо Удачи</title>
<style>
    body {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        height: 100vh;
        margin: 0;
        font-family: Arial, sans-serif;
        background-color: #121212; /* тёмный фон */
        color: #ffffff; /* светлый текст */
    }

    h1 {
        color: #ffffff;
    }

    button {
        background-color: #333;
        color: #fff;
        border: none;
        padding: 10px 20px;
        margin-top: 20px;
        cursor: pointer;
        border-radius: 5px;
        font-size: 16px;
        transition: background-color 0.3s;
    }

    button:hover {
        background-color: #555;
    }

    .wheel-container {
        position: relative;
        margin-top: 20px;
        text-align: center;
    }

    canvas {
        border: 2px solid #ffd700; /* яркий бордюр для выделения */
        border-radius: 50%;
        background-color: #222; /* чуть светлее фона, чтобы колесо было заметнее */
        display: block;
    }

    /* Стрелка */
    .arrow {
        position: absolute;
        top: -20px;
        left: 50%;
        transform: translateX(-50%);
        width: 0; 
        height: 0; 
        border-left: 15px solid transparent;
        border-right: 15px solid transparent;
        border-bottom: 30px solid red; /* красная стрелка */
        z-index: 2;
    }

    #result {
        margin-top: 20px;
        font-size: 1.2em;
        color: #ffffff;
        min-height: 1.2em;
    }
</style>
</head>
<body>
<h1>Колесо Удачи</h1>
<div class="wheel-container">
    <div class="arrow"></div>
    <canvas id="wheel" width="400" height="400"></canvas>
    <button id="spin">Крутить колесо!</button>
    <div id="result"></div>
</div>
<script>
const canvas = document.getElementById('wheel');
const ctx = canvas.getContext('2d');
const resultDiv = document.getElementById('result');

const prizes = [
    { name: 'Ничего', percent: 90, color: '#FF6347', text: 'Ничего' },
    { name: 'Lol поп', percent: 5, color: '#7CFC00', text: 'Lol Pop' },
    { name: '1.000 Stars', percent: 5, color: '#1E90FF', text: '1.000 Stars' },
];

let prizeTexts = [];

function getPrizeAngles() {
    const totalPercent = prizes.reduce((sum, p) => sum + p.percent, 0);
    let angle = 0;
    prizeTexts = [];
    for (let prize of prizes) {
        const sliceAngle = (prize.percent / totalPercent) * 2 * Math.PI;
        ctx.beginPath();
        ctx.moveTo(200, 200);
        ctx.arc(200, 200, 200, angle, angle + sliceAngle);
        ctx.fillStyle = prize.color;
        ctx.shadowColor = 'rgba(0,0,0,0.3)';
        ctx.shadowBlur = 10;
        ctx.fill();
        ctx.closePath();
        // Запоминаем сегмент
        prize.startAngle = angle;
        prize.endAngle = angle + sliceAngle;

        // Добавляем texte для вставки
        prizeTexts.push({
            startAngle: angle,
            endAngle: angle + sliceAngle,
            text: prize.text
        });

        angle += sliceAngle;
    }

    // Добавляем текст на сегментах
    drawTextOnSegments();
}

function drawTextOnSegments() {
    // Основное колесо
    ctx.save();
    ctx.translate(200, 200);
    ctx.font = 'bold 16px Arial';
    ctx.textAlign = 'center';
    ctx.fillStyle = 'white';

    prizeTexts.forEach(prize => {
        // Центр сектора
        const middleAngle = (prize.startAngle + prize.endAngle) / 2;
        const radius = 100; // радиус для текста
        const x = Math.cos(middleAngle) * radius;
        const y = Math.sin(middleAngle) * radius;

        ctx.save();
        ctx.translate(x, y);
        ctx.rotate(middleAngle);
        ctx.fillText(prize.text, 0, 0);
        ctx.restore();
    });
    ctx.restore();
}

let isSpinning = false;

document.getElementById('spin').addEventListener('click', () => {
    if (isSpinning) return;
    resultDiv.textContent = '';
    isSpinning = true;

    const spinDegree = Math.random() * 360 + 1440; // 4 круга + случайный угол
    const duration = 4000; // более длинное вращение
    const startTime = performance.now();

    function animate() {
        const elapsed = performance.now() - startTime;
        const progress = Math.min(elapsed / duration, 1);
        const easeProgress = 1 - Math.pow(1 - progress, 3);

        const animatedAngle = spinDegree * easeProgress;

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.save();
        ctx.translate(200, 200);
        ctx.rotate((animatedAngle * Math.PI) / 180);
        ctx.translate(-200, -200);
        getPrizeAngles(); // рисуем сегменты и текст
        ctx.restore();

        if (progress < 1) {
            requestAnimationFrame(animate);
        } else {
            // Определяем сектор
            let finalAngle = (animatedAngle % 360);
            if (finalAngle < 0) finalAngle += 360;
            let pointerAngle = (360 - finalAngle + 90) % 360;

            let prizeWon = null;
            for (let prize of prizes) {
                let startDeg = (prize.startAngle * 180) / Math.PI;
                let endDeg = (prize.endAngle * 180) / Math.PI;
                if (startDeg < 0) startDeg += 360;
                if (endDeg < 0) endDeg += 360;

                if (startDeg > endDeg) {
                    if (pointerAngle >= startDeg || pointerAngle <= endDeg) {
                        prizeWon = prize;
                        break;
                    }
                } else {
                    if (pointerAngle >= startDeg && pointerAngle <= endDeg) {
                        prizeWon = prize;
                        break;
                    }
                }
            }

            if (prizeWon) {
                resultDiv.textContent = `Вам выпало: ${prizeWon.name}!`;
            } else {
                resultDiv.textContent = `Что-то пошло не так.`;
            }
            isSpinning = false;
        }
    }

    animate();
});

// Перерисовать всё при загрузке
getPrizeAngles();

</script>
</body>
</html>
