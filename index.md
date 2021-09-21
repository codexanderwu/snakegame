<html>
<head>
  <title>Snake Game</title>
</head>
<body>
  <canvas  height="550" width="1200" id="ctx" style="border:2px solid #000000"/>
  <script>

    var ctx = document.getElementById('ctx').getContext('2d');
    var width = 1200;
    var height = 550;
    var snakeList, foodList, direction, eaten, intervalVar, score, running=false;
    ctx.font = "20px Calibri";
    ctx.fillText('Click me to start the game',500,250);

    var snakeBody = {
      width:20,
      height:20,
      color:'violet'
    };

    var food = {
      width:20,
      height:20,
      color:'orange'
    };

    document.getElementById('ctx').onmousedown = function() {
      if (running){
         clearInterval(intervalVar);
         running = false;
       }
      startGame();
    }

    document.onkeydown = function(event) {
      //0 - Left
      //1 - Up
      //2 - Right
      //3 - Down
      if (event.keyCode == 37 && direction != 2) {
        direction = 0;
      }
      else if (event.keyCode == 38 && direction != 3) {
        direction = 1;
      }
      else if (event.keyCode == 39 && direction != 0) {
        direction = 2;
      }
      else if (event.keyCode == 40 && direction != 1) {
        direction = 3;
      }
    }

    testCollision = function(rect1,rect2) {
      return ((rect1.x <= rect2.x + food.width) &&
              (rect2.x <= rect1.x + snakeBody.width) &&
              (rect1.y <= rect2.y + food.height) &&
              (rect2.y <= rect1.y +snakeBody.height));
    }

    testCollisionSnake = function(snake1,snake2) {
      return((Math.abs(snake1.x-snake2.x)<5) &&
            (Math.abs(snake1.y-snake2.y)<5));
    }

    drawSnake = function(sb,i) {
      ctx.save();
      if (i==0)
      ctx.fillStyle = 'blue';
      else
        ctx.fillStyle = snakeBody.color;
      ctx.fillRect(sb.x,sb.y,snakeBody.width,snakeBody.height);
      ctx.restore();
    }

    drawFood = function(f,i) {
      ctx.save();
      ctx.fillStyle = food.color;
      ctx.fillRect(f.x,f.y,food.width,food.height);
      ctx.restore();
    }

    updateSnakeList = function() {
      for (var i=snakeList.length-1;i>=0;i--) {
        if (direction == 0) {
          if (i==0) {
            snakeList[i].x = snakeList[i].x - 5;
              }
              else {
          snakeList[i].x = snakeList[i-1].x;
          snakeList[i].y = snakeList[i-1].y;
        }
      }
      else if (direction == 1) {
        if (i==0) {
          snakeList[i].y = snakeList[i].y - 5;
            }
            else {
        snakeList[i].x = snakeList[i-1].x;
        snakeList[i].y = snakeList[i-1].y;
      }
      }
      if (direction == 2) {
        if (i==0) {
          snakeList[i].x = snakeList[i].x + 5;
            }
            else {
        snakeList[i].x = snakeList[i-1].x;
        snakeList[i].y = snakeList[i-1].y;
      }
      }
      if (direction == 3) {
        if (i==0) {
          snakeList[i].y = snakeList[i].y + 5;
            }
            else {
        snakeList[i].x = snakeList[i-1].x;
        snakeList[i].y = snakeList[i-1].y;
      }
        }
      }
    }

    checkSnakePosition = function() {
      if (snakeList[0].x > 1200) {
        snakeList[0].x = 0;
      }
      if (snakeList[0].x < 0) {
        snakeList[0].x = 1200;
      }
      if (snakeList[0].y > 550) {
        snakeList[0].y = 0;
      }
      if (snakeList[0].y < 0) {
        snakeList[0].y = 550;
      }
    }

    isGameOver = function() {
      for (i in snakeList) {
        if (i==0)
         continue;
        if (testCollisionSnake(snakeList[0], snakeList[i])) {
          clearInterval(intervalVar) ;
          ctx.fillText('Game Over! Click to restart',500,250);
          return;
        }
      }
    }

    updateSnakePosition = function() {
      ctx.clearRect(0,0,width,height);
      while(eaten) {
        var pos_x = Math.random()*1200+5;
        var pos_y = Math.random()*545+5;
        foodList[0] = {x:pos_x,y:pos_y};
        eaten = false;
      }
      foodList.forEach(drawFood);
      snakeList.forEach(drawSnake);

      if (testCollision(snakeList[0],foodList[0])) {
        foodList = [];
        eaten = true;
        score += 1;
        var new_X,new_Y;
        if (direction == 0) {
          new_X = snakeList[0].x - 10;
          new_Y = snakeList[0].y;
        }
        else if (direction == 1) {
          new_X = snakeList[0].x;
          new_Y = snakeList[0].y - 10;
        }
        else if (direction == 2) {
          new_X = snakeList[0].x + 10;
          new_Y = snakeList[0].y;
        }
        else if (direction == 3) {
          new_X = snakeList[0].x;
          new_Y = snakeList[0].y + 10;
        }
        snakeList.unshift({x:new_X,y:new_Y});
      }
      ctx.fillText('Score: '+score,1100,30);
      ctx.fillText('Dont crush into yourself',1000,50);
      ctx.fillText('Press to start or restart',1000,70);

      isGameOver();
      checkSnakePosition();
      updateSnakeList();
    }

    startGame = function() {
      snakeList = [
      {x:220,y:200},
      {x:210,y:200},
      {x:200,y:200}
      ];
      foodList = [];
      direction = 99;
      eaten = true;
      score = 0;
      running = true;
      intervalVar = setInterval(updateSnakePosition,18);  // 10 fps
    }

  </script>
</body>
</html>
