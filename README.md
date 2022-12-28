# simple-Snake-game
Here's an example of how you can create a simple Snake game using HTML, CSS, and JavaScript
<html>
  <head>
    <title>My Snake Game</title>
    <style>
      canvas {
        border: 1px solid black;
      }
    </style>
  </head>
  <body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
      // Get the canvas element
      const canvas = document.getElementById("gameCanvas");
      // Get the canvas context
      const ctx = canvas.getContext("2d");

      // Define the snake's position and size
      let snake = [{x: 150, y: 150}, {x: 140, y: 150}, {x: 130, y: 150}];
      const snakeSize = 10;

      // Define the direction of the snake's movement
      let dx = 10;
      let dy = 0;

      // Define the food's position and size
      let foodX;
      let foodY;
      const foodSize = 10;

      // Define the game's score
      let score = 0;

      function draw() {
        // Clear the canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // Draw the snake
        ctx.fillStyle = "green";
        snake.forEach(segment => {
          ctx.fillRect(segment.x, segment.y, snakeSize, snakeSize);
        });

        // Draw the food
        ctx.fillStyle = "red";
        ctx.fillRect(foodX, foodY, foodSize, foodSize);

        // Update the snake's position
        const head = {x: snake[0].x + dx, y: snake[0].y + dy};
        snake.unshift(head);
        const didEatFood = snake[0].x === foodX && snake[0].y === foodY;
        if (didEatFood) {
          score++;
          generateFood();
        } else {
          snake.pop();
        }

        // Check for game over
        checkGameOver();
      }

      // Generate a random position for the food
      function generateFood() {
        foodX = Math.floor(Math.random() * (canvas.width - foodSize));
        foodY = Math.floor(Math.random() * (canvas.height - foodSize));
      }

      // Check if the game is over
      function checkGameOver() {
        // Check if the snake has collided with a wall
        if (snake[0].x < 0 || snake[0].x > canvas.width - snakeSize || snake[0].y < 0 || snake[0].y > canvas.height - snakeSize) {
          alert("Game over! Your score is " + score);
          resetGame();
        }

        // Check if the snake has collided with itself
        for (let i = 1; i < snake.length; i++) {
          if (snake[0].x === snake[i].x && snake[0].y === snake[i].y) {
            alert("Game over! Your score is " + score);
            resetGame();
          }
        }
