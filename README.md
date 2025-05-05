<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Jeu Snake</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            background-color: #111;
            color: #fff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
        }

        h1 {
            margin: 20px 0;
            color: #00ff99;
        }

        #gameContainer {
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 20px auto;
        }

        canvas {
            background-color: #000;
            border: 2px solid #00ff99;
        }

        #score {
            font-size: 20px;
            margin: 10px;
        }

        #restartBtn {
            background-color: #00ff99;
            color: #000;
            border: none;
            padding: 10px 20px;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 15px;
            display: none;
        }

        #restartBtn:hover {
            background-color: #00cc77;
        }

        footer {
            margin-top: 30px;
            color: #555;
            font-size: 14px;
        }
    </style>
</head>
<body>

    <h1>🐍 Snake Game</h1>
    <div id="score">Score : 0</div>
    <div id="gameContainer">
        <canvas id="gameCanvas" width="400" height="400"></canvas>
    </div>
    <button id="restartBtn" onclick="restartGame()">Rejouer</button>

    <footer>&copy; 2025 - Jeu Snake HTML/JS</footer>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const box = 20;
        const canvasSize = 400;
        const rows = canvasSize / box;
        const cols = canvasSize / box;

        let snake = [];
        snake[0] = {
            x: 9 * box,
            y: 10 * box
        };

        let direction = "";
        let score = 0;

        let food = {
            x: Math.floor(Math.random() * cols) * box,
            y: Math.floor(Math.random() * rows) * box
        };

        document.addEventListener("keydown", setDirection);

        function setDirection(event) {
            if (event.key === "ArrowLeft" && direction !== "RIGHT") {
                direction = "LEFT";
            } else if (event.key === "ArrowUp" && direction !== "DOWN") {
                direction = "UP";
            } else if (event.key === "ArrowRight" && direction !== "LEFT") {
                direction = "RIGHT";
            } else if (event.key === "ArrowDown" && direction !== "UP") {
                direction = "DOWN";
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            for (let i = 0; i < snake.length; i++) {
                ctx.fillStyle = i === 0 ? "#00ff99" : "#ffffff";
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
                ctx.strokeStyle = "#000";
                ctx.strokeRect(snake[i].x, snake[i].y, box, box);
            }

            ctx.fillStyle = "#ff0033";
            ctx.fillRect(food.x, food.y, box, box);

            // Position actuelle de la tête
            let snakeX = snake[0].x;
            let snakeY = snake[0].y;

            // Déplacement
            if (direction === "LEFT") snakeX -= box;
            if (direction === "UP") snakeY -= box;
            if (direction === "RIGHT") snakeX += box;
            if (direction === "DOWN") snakeY += box;

            // Mange la pomme
            if (snakeX === food.x && snakeY === food.y) {
                score++;
                document.getElementById("score").innerText = "Score : " + score;
                food = {
                    x: Math.floor(Math.random() * cols) * box,
                    y: Math.floor(Math.random() * rows) * box
                };
            } else {
                // Retire la queue
                snake.pop();
            }

            const newHead = {
                x: snakeX,
                y: snakeY
            };

            // Game Over Conditions
            if (
                snakeX < 0 || snakeX >= canvas.width ||
                snakeY < 0 || snakeY >= canvas.height ||
                collision(newHead, snake)
            ) {
                clearInterval(game);
                document.getElementById("restartBtn").style.display = "inline-block";
                return;
            }

            snake.unshift(newHead);
        }

        function collision(head, array) {
            for (let i = 0; i < array.length; i++) {
                if (head.x === array[i].x && head.y === array[i].y) {
                    return true;
                }
            }
            return false;
        }

        function restartGame() {
            snake = [];
            snake[0] = {
                x: 9 * box,
                y: 10 * box
            };
            direction = "";
            score = 0;
            food = {
                x: Math.floor(Math.random() * cols) * box,
                y: Math.floor(Math.random() * rows) * box
            };
            document.getElementById("score").innerText = "Score : 0";
            document.getElementById("restartBtn").style.display = "none";
            clearInterval(game);
            game = setInterval(draw, 120);
        }

        // Démarrage du jeu
        let game = setInterval(draw, 120);
    </script>
</body>
</html>

