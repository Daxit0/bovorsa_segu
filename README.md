# bovorsa_segu
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego de la Viborita</title>
    <style>
        canvas {
            display: block;
            margin: auto;
            background-color: #f2f2f2;
        }
        .buttons {
            text-align: center;
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            margin: 0 10px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <div class="buttons">
        <button id="upButton">&#8593;</button>
        <button id="leftButton">&#8592;</button>
        <button id="rightButton">&#8594;</button>
        <button id="downButton">&#8595;</button>
        <button id="startButton">Empezar Juego</button>
    </div>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const box = 20;
        let snake = [];
        snake[0] = { x: 10 * box, y: 10 * box };
        let food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
        let d;

        document.getElementById("startButton").addEventListener("click", startGame);
        document.getElementById("leftButton").addEventListener("click", () => (d !== "RIGHT") && (d = "LEFT"));
        document.getElementById("upButton").addEventListener("click", () => (d !== "DOWN") && (d = "UP"));
        document.getElementById("rightButton").addEventListener("click", () => (d !== "LEFT") && (d = "RIGHT"));
        document.getElementById("downButton").addEventListener("click", () => (d !== "UP") && (d = "DOWN"));

        function startGame() {
            snake = [{ x: 10 * box, y: 10 * box }];
            food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
            d = undefined;
            document.addEventListener("keydown", direction);
            clearInterval(game);
            game = setInterval(draw, 50); // Respuesta más rápida al presionar las flechas
        }

        function direction(event) {
            if (event.keyCode == 37 && d != "RIGHT") {
                d = "LEFT";
            } else if (event.keyCode == 38 && d != "DOWN") {
                d = "UP";
            } else if (event.keyCode == 39 && d != "LEFT") {
                d = "RIGHT";
            } else if (event.keyCode == 40 && d != "UP") {
                d = "DOWN";
            }
        }

        function collision(head, array) {
            for (let i = 0; i < array.length; i++) {
                if (head.x == array[i].x && head.y == array[i].y) {
                    return true;
                }
            }
            return false;
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < snake.length; i++) {
                ctx.fillStyle = (i == 0) ? "green" : "white";
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
                ctx.strokeStyle = "black";
                ctx.strokeRect(snake[i].x, snake[i].y, box, box);
            }
            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);

            let snakeX = snake[0].x;
            let snakeY = snake[0].y;

            if (d == "LEFT") snakeX -= box;
            if (d == "UP") snakeY -= box;
            if (d == "RIGHT") snakeX += box;
            if (d == "DOWN") snakeY += box;

            if (snakeX == food.x && snakeY == food.y) {
                food = { x: Math.floor(Math.random() * 20) * box, y: Math.floor(Math.random() * 20) * box };
            } else {
                snake.pop();
            }

            let newHead = { x: snakeX, y: snakeY };

            if (snakeX < 0 || snakeX >= canvas.width || snakeY < 0 || snakeY >= canvas.height || collision(newHead, snake)) {
                clearInterval(game);
            }

            snake.unshift(newHead);
        }

        let game;
    </script>
</body>
</html>
