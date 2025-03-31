> [!NOTE]
> 诸多bug慢慢修

> [!Caution]
> 认真工作哦

> 注：
> 代码copy后修改后缀即食；急转弯有bug，有概率会死掉

> 还有一件事：
> 默认方向向右，小心撞墙

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>pusipusi</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
        }
        #gameCanvas {
            border: 2px solid #333;
            background-color: #fff;
        }
        .controls {
            margin: 20px 0;
            display: flex;
            gap: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
        }
        button:hover {
            background-color: #45a049;
        }
        #difficulty {
            padding: 8px;
            font-size: 16px;
        }
        .info {
            margin: 10px 0;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>工作累了，吃个蛇蛇</h1>
    <div class="controls">
        <select id="difficulty">
            <option value="easy">Eeeasy</option>
            <option value="normal" selected>Normal</option>
            <option value="hard">Diiiificult</option>
        </select>
        <button onclick="startGame()">start game</button>
    </div>
    <div class="info">
        得分: <span id="score">0</span> | 
        难度: <span id="currentDifficulty">Normal</span>
    </div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const gridSize = 20;
        const tileCount = canvas.width / gridSize;
        
        let snake = [];
        let food = {};
        let dx = 1;  // 默认初始方向哦
        let dy = 0;  //配合dx修改四个默认初始方向
        let score = 0;
        let gameLoop;
        let speed = 150;
        
        const difficultySettings = {
            easy: { speed: 200, color: '#4CAF50' },
            normal: { speed: 150, color: '#FF9800' },
            hard: { speed: 100, color: '#F44336' }
        };

        function startGame() {
            clearInterval(gameLoop);
            snake = [{x: 10, y: 10}];
            score = 0;
            dx = 1;  // 初始方向
            dy = 0;
            document.getElementById('score').textContent = score;
            
            const difficulty = document.getElementById('difficulty').value;
            speed = difficultySettings[difficulty].speed;
            document.getElementById('currentDifficulty').textContent = 
                document.getElementById('difficulty').options[document.getElementById('difficulty').selectedIndex].text;
            
            generateFood();
            gameLoop = setInterval(update, speed);
        }

        function generateFood() {
            food = {
                x: Math.floor(Math.random() * tileCount),
                y: Math.floor(Math.random() * tileCount)
            };
            for (let segment of snake) {
                if (segment.x === food.x && segment.y === food.y) {
                    generateFood();
                }
            }
        }

        function update() {
            const head = {x: snake[0].x + dx, y: snake[0].y + dy};
            
            // 在边界吗
            if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
                gameOver();
                return;
            }
            
            // 自碰了吗
            for (let segment of snake) {
                if (head.x === segment.x && head.y === segment.y) {
                    gameOver();
                    return;
                }
            }
            
            snake.unshift(head);
            
            // 吃食物logic
            if (head.x === food.x && head.y === food.y) {
                score += 10;
                document.getElementById('score').textContent = score;
                generateFood();
            } else {
                snake.pop();
            }
            
            draw();
        }

        function draw() {
            ctx.fillStyle = '#fff';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // 蛇蛇
            const difficulty = document.getElementById('difficulty').value;
            ctx.fillStyle = difficultySettings[difficulty].color;
            for (let segment of snake) {
                ctx.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize-2, gridSize-2);
            }
            
            // 果果
            ctx.fillStyle = '#FF0000';
            ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize-2, gridSize-2);
        }

        function gameOver() {
            clearInterval(gameLoop);
            alert(`游戏结束咯，得分: ${score}`);
        }

        // 方向控制
        document.addEventListener('keydown', (e) => {
            switch(e.key) {
                case 'ArrowUp':
                    if (dy === 0) { dx = 0; dy = -1; }
                    break;
                case 'ArrowDown':
                    if (dy === 0) { dx = 0; dy = 1; }
                    break;
                case 'ArrowLeft':
                    if (dx === 0) { dx = -1; dy = 0; }
                    break;
                case 'ArrowRight':
                    if (dx === 0) { dx = 1; dy = 0; }
                    break;
            }
        });
    </script>
</body>
</html>
```