<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: black;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        canvas {
            border: 1px solid white;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        // Game variables
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const gridSize = 20;
        let snake = [{x: 10, y: 10}];
        let food = {x: 15, y: 15};
        let dx = 1;
        let dy = 0;
        let score = 0;

        // Main game loop
        function gameLoop() {
            requestAnimationFrame(gameLoop);
            update();
            draw();
        }

        // Update game state
        function update() {
            const head = {x: snake[0].x + dx, y: snake[0].y + dy};

            // Check for collision with food
            if (head.x === food.x && head.y === food.y) {
                score++;
                food = generateFood();
            } else {
                snake.pop(); // Remove the tail segment
            }

            // Check for collision with walls or itself
            if (head.x < 0 || head.x >= canvas.width / gridSize || head.y < 0 || head.y >= canvas.height / gridSize || snake.some(segment => segment.x === head.x && segment.y === head.y)) {
                // Game over
                alert("Game Over! Your score: " + score);
                resetGame();
                return;
            }

            // Move the snake
            snake.unshift(head);
        }

        // Generate food at random position
        function generateFood() {
            return {
                x:
