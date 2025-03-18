<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TeleSendBot - Отправка в Telegram от лица бота</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: radial-gradient(circle at top, #1a1a2e, #0f3460);
            color: #fff;
        }
        .container {
            background: rgba(255, 255, 255, 0.1);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            text-align: center;
            width: 400px;
        }
        h2 {
            color: #fff;
            margin-bottom: 15px;
            text-transform: uppercase;
        }
        input, textarea {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: none;
            border-radius: 8px;
            background: rgba(255, 255, 255, 0.2);
            color: #fff;
            outline: none;
            transition: 0.3s;
        }
        input::placeholder, textarea::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }
        input:focus, textarea:focus {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.05);
        }
        .buttons {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        button {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            background: linear-gradient(45deg, #ff416c, #ff4b2b);
            color: white;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.4s;
        }
        .github-btn {
            margin-top: 20px;
            display: block;
            text-align: center;
            background: #333;
            padding: 10px;
            border-radius: 8px;
            text-decoration: none;
            color: #fff;
            font-size: 14px;
            font-weight: bold;
            transition: 0.3s;
        }
        .github-btn:hover {
            background: #444;
        }
        .info-box {
            background: rgba(0, 0, 0, 0.2);
            padding: 15px;
            border-radius: 12px;
            text-align: left;
            font-size: 14px;
            margin-top: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        .info-box h3 {
            color: #ff4b2b;
            margin-bottom: 10px;
        }
        .info-box p {
            margin-bottom: 8px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Отправка в Telegram</h2>
        <input type="text" id="token" placeholder="Введите токен бота">
        <input type="text" id="chat_id" placeholder="Введите Chat ID">
        <input type="text" id="reply_id" placeholder="ID сообщения для ответа (необязательно)">
        <textarea id="message" placeholder="Введите сообщение" rows="4"></textarea>
        <input type="file" id="photo" accept="image/*">

        <div class="buttons">
            <button onclick="sendMessageOrPhoto()">📩 Отправить</button>
            <button onclick="clearForm()">🧹 Очистить</button>
        </div>

        <a href="https://github.com/твоя-ссылка" class="github-btn">⭐ GitHub</a>

        <div class="info-box">
            <h3>📌 Как узнать токен бота?</h3>
            <p>1️⃣ Открой Telegram и найди бота <a href="https://t.me/BotFather"><b>@BotFather</b></a>.</p>
            <p>2️⃣ Напиши <b>/newbot</b> и следуй инструкциям.</p>
            <p>3️⃣ После создания бота, BotFather пришлёт тебе токен.</p>

            <h3>📌 Как узнать Chat ID?</h3>
            <p>1️⃣ Напиши <a href="https://t.me/userinfobot"><b>@userinfobot</b></a> в Telegram.</p>
            <p>2️⃣ Отправь команду <b>/start</b>.</p>
            <p>3️⃣ Бот покажет твой Chat ID.</p>
        </div>
    </div>

    <script>
        function sendMessageOrPhoto() {
            let token = document.getElementById("token").value.trim();
            let chat_id = document.getElementById("chat_id").value.trim();
            let reply_id = document.getElementById("reply_id").value.trim();
            let message = document.getElementById("message").value.trim();
            let photoInput = document.getElementById("photo").files[0];

            if (!token || !chat_id) {
                alert("Введите токен и Chat ID!");
                return;
            }

            let formData = new FormData();
            formData.append("chat_id", chat_id);
            if (reply_id) {
                formData.append("reply_to_message_id", reply_id);
            }

            let url = `https://api.telegram.org/bot${token}/`;
            
            if (photoInput) {
                formData.append("photo", photoInput);
                url += "sendPhoto";
            } else {
                formData.append("text", message);
                formData.append("parse_mode", "HTML");
                url += "sendMessage";
            }

            fetch(url, {
                method: "POST",
                body: formData
            })
            .then(response => response.json())
            .then(data => {
                if (data.ok) {
                    alert(photoInput ? "✅ Фото отправлено!" : "✅ Сообщение отправлено!");
                } else {
                    alert("❌ Ошибка: " + data.description);
                }
            })
            .catch(error => alert("Ошибка отправки!"));
        }

        function clearForm() {
            document.getElementById("token").value = "";
            document.getElementById("chat_id").value = "";
            document.getElementById("reply_id").value = "";
            document.getElementById("message").value = "";
            document.getElementById("photo").value = "";
        }
    </script>
</body>
</html>
