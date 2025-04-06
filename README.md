<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="README.md">
    <title>Telefonni uzoqroqqa qo'yish</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            text-align: center;
        }
        #container {
            position: relative;
            padding: 30px;
            background-color: #fff;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            width: 300px;
        }
        #message {
            margin-top: 20px;
            font-size: 18px;
            color: #333;
        }
        .button {
            padding: 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        .button:hover {
            background-color: #45a049;
        }
        #stopButton {
            background-color: #f44336;
            display: none; /* Initially hidden */
        }
        #stopButton:hover {
            background-color: #e53935;
        }
        #audioPlayer {
            display: none;
        }
    </style>
</head>
<body>

<div id="container">
    <h1>Telefonni uzoqroqqa qo'ying</h1>
    <p>Chapak cherting yoki tugmani bosing!</p>
    <button class="button" id="actionButton">Tugmani bosing!</button>
    <button class="button" id="stopButton">Qo'shiqni to'xtatish</button>
    <p id="message"></p>

    <!-- Audio o‘ynatuvchi element -->
    <audio id="audioPlayer" src="c:\Users\12345\Music\Neoni - DARKSIDE.mp3"></audio>
</div>
<script>
    const actionButton = document.getElementById('actionButton');
    const stopButton = document.getElementById('stopButton');
    const message = document.getElementById('message');
    const audioPlayer = document.getElementById('audioPlayer');

    // Tugmani bosish funksiyasi
    actionButton.addEventListener('click', function() {
        actionButton.style.display = "none";  // "Tugmani bosing" tugmasini yashirish
        stopButton.style.display = "inline-block";  // "Qo'shiqni to'xtatish" tugmasini ko'rsatish

        message.textContent = "Qoshiq aytishni boshlayapman! 🎤🎶";
        audioPlayer.play();
        setTimeout(() => {
            message.textContent = "🎵 La-la-la, bu mening qo'shig'im! 🎶";
        }, 2000); 
    });

    // Qo'shiqni to'xtatish funksiyasi
    stopButton.addEventListener('click', function() {
        audioPlayer.pause();  // Qo'shiqni to'xtatish
        audioPlayer.currentTime = 0;  // Qo'shiqni boshidan qaytaring
        message.textContent = "Qo'shiq to'xtatildi. 🎶";
        stopButton.style.display = "none";  // To'xtatish tugmasini yashirish
        actionButton.style.display = "inline-block";  // "Tugmani bosing" tugmasini yana ko'rsatish
    });

    // Chapakni aniqlash uchun DeviceMotion Event
    let lastAcceleration = { x: 0, y: 0, z: 0 };
    let threshold = 15;  // Chapakni aniqlash uchun thresholdni pastga tushirdik (25 -> 15)

    function detectFlap(event) {
        const ax = event.acceleration.x;
        const ay = event.acceleration.y;
        const az = event.acceleration.z;

        // Tezlikni o‘lchash
        const deltaX = Math.abs(lastAcceleration.x - ax);
        const deltaY = Math.abs(lastAcceleration.y - ay);
        const deltaZ = Math.abs(lastAcceleration.z - az);

        // Agar xabar threshold'dan oshsa, chapakni chertgan deb hisoblaymiz
        if (deltaX > threshold || deltaY > threshold || deltaZ > threshold) {
            message.textContent = "Qoshiq aytishni boshlayapman! 🎤🎶";
            audioPlayer.play();
            stopButton.style.display = "inline-block"; // Chapak chertilganida ham to'xtatish tugmasini ko'rsatish
            actionButton.style.display = "none"; // "Tugmani bosing" tugmasi ko'rinmasin
            setTimeout(() => {
                message.textContent = "🎵 La-la-la, bu mening qo'shig'im! 🎶";
            }, 2000);
        }

        // So‘nggi tezlikni saqlash
        lastAcceleration = { x: ax, y: ay, z: az };
    }

    // device motion eventga hodisani qo‘shish
    if (window.DeviceMotionEvent) {
        window.addEventListener('devicemotion', detectFlap, false);
    } else {
        console.log('DeviceMotionEvent ishlamayapti');
    }
</script>

</body>
</html>




