<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Мой Mini App</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #6e8efb, #a777e3);
            color: white;
            text-align: center;
            padding: 20px;
        }
        button {
            background: #fff;
            color: #6e8efb;
            border: none;
            padding: 10px 20px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>✨ Привет, Telegram!</h1>
    <p>Это мой первый Mini App!</p>
    <button onclick="showUserInfo()">👤 Кто я?</button>
    <p id="user-info"></p>

    <script>
        function showUserInfo() {
            const user = Telegram.WebApp.initDataUnsafe.user;
            if (user) {
                const info = `Тебя зовут ${user.first_name} ${user.last_name || ''} (@${user.username || 'без username'})`;
                document.getElementById('user-info').innerText = info;
            } else {
                document.getElementById('user-info').innerText = 'Данные пользователя недоступны';
            }
        }

        // Инициализируем приложение
        Telegram.WebApp.ready();
    </script>
</body>
</html>
