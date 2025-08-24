<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Snake Game</title>
  <style>
    body { 
      text-align: center; 
      margin: 0; 
      background: #111; 
      color: white; 
      font-family: Arial, sans-serif;
    }
    h1 {
      margin: 20px;
    }
    canvas { 
      background: #222; 
      display: block; 
      margin: auto; 
      border: 2px solid #555; 
    }
  </style>
</head>
<body>
  <h1>🐍 Snake Game</h1>
  <canvas id="gameCanvas" width="400" height="400"></canvas>
  <p>Use Arrow Keys ⬅️⬆️➡️⬇️ to move</p>
  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    let box = 20;
    let snake = [{x: 9*box, y:10*box}];
    let food = {
      x: Math.floor(Math.random()*19)*box, 
      y: Math.floor(Math.random()*19)*box
    };
    let d;

    document.addEventListener("keydown", direction);
    function direction(event){
      if(event.keyCode == 37 && d != "RIGHT") d = "LEFT";
      else if(event.keyCode == 38 && d != "DOWN") d = "UP";
      else if(event.keyCode == 39 && d != "LEFT") d = "RIGHT";
      else if(event.keyCode == 40 && d != "UP") d = "DOWN";
    }

    function draw(){
      ctx.clearRect(0, 0, 400, 400);
      for(let i=0;i<snake.length;i++){
        ctx.fillStyle = (i==0)? "lime":"green";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
      }

      ctx.fillStyle = "red";
      ctx.fillRect(food.x, food.y, box, box);

      let snakeX = snake[0].x;
      let snakeY = snake[0].y;

      if(d == "LEFT") snakeX -= box;
      if(d == "UP") snakeY -= box;
      if(d == "RIGHT") snakeX += box;
      if(d == "DOWN") snakeY += box;

      if(snakeX == food.x && snakeY == food.y){
        food = {
          x: Math.floor(Math.random()*19)*box, 
          y: Math.floor(Math.random()*19)*box
        };
      } else {
        snake.pop();
      }

      let newHead = {x: snakeX, y: snakeY};

      // Game Over condition
      if(
        snakeX < 0 || snakeX >= 400 || 
        snakeY < 0 || snakeY >= 400 || 
        collision(newHead, snake)
      ){
        clearInterval(game);
        alert("Game Over 😢 Reload to play again!");
      }

      snake.unshift(newHead);
    }

    function collision(head, array){
      for(let i=0;i<array.length;i++){
        if(head.x == array[i].x && head.y == array[i].y){
          return true;
        }
      }
      return false;
    }

    let game = setInterval(draw, 100);
  </script>
</body>
</html>
