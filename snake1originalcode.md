<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>贪食蛇小游戏</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: #fff;
            padding: 20px;
        }
        
        .header {
            text-align: center;
            margin-bottom: 20px;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 8px;
            background: linear-gradient(90deg, #ff9a00, #ff0080);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        
        .subtitle {
            font-size: 1.1rem;
            color: #a0a0c0;
            margin-bottom: 25px;
        }
        
        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
            max-width: 800px;
            width: 100%;
        }
        
        .game-info {
            display: flex;
            justify-content: space-between;
            width: 100%;
            margin-bottom: 20px;
            background-color: rgba(255, 255, 255, 0.08);
            border-radius: 10px;
            padding: 15px;
        }
        
        .score-container, .high-score-container, .level-container {
            text-align: center;
            flex: 1;
        }
        
        .info-label {
            font-size: 1rem;
            color: #a0a0c0;
            margin-bottom: 5px;
        }
        
        .info-value {
            font-size: 2.2rem;
            font-weight: bold;
            color: #4dffea;
        }
        
        .game-area {
            position: relative;
            margin-bottom: 20px;
        }
        
        #game-canvas {
            border-radius: 8px;
            background-color: #0d1b2a;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        }
        
        .controls {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
            margin-bottom: 25px;
        }
        
        .btn {
            padding: 12px 24px;
            font-size: 1.1rem;
            font-weight: 600;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 140px;
        }
        
        #start-btn {
            background: linear-gradient(90deg, #00b09b, #96c93d);
            color: white;
        }
        
        #pause-btn {
            background: linear-gradient(90deg, #ff9a00, #ff0080);
            color: white;
        }
        
        #restart-btn {
            background: linear-gradient(90deg, #8a2be2, #4a00e0);
            color: white;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .btn:active {
            transform: translateY(1px);
        }
        
        .instructions {
            background-color: rgba(255, 255, 255, 0.08);
            border-radius: 10px;
            padding: 20px;
            width: 100%;
            margin-top: 15px;
        }
        
        .instructions h3 {
            color: #ff9a00;
            margin-bottom: 12px;
            font-size: 1.3rem;
        }
        
        .instructions p {
            margin-bottom: 8px;
            line-height: 1.5;
            color: #d0d0e0;
        }
        
        .key {
            display: inline-block;
            background-color: rgba(255, 255, 255, 0.15);
            border-radius: 4px;
            padding: 2px 8px;
            font-family: monospace;
            font-weight: bold;
            margin: 0 3px;
        }
        
        .mobile-controls {
            display: none;
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(2, 1fr);
            gap: 10px;
            margin-top: 20px;
            width: 100%;
            max-width: 300px;
        }
        
        .mobile-btn {
            height: 70px;
            background-color: rgba(255, 255, 255, 0.15);
            border: none;
            border-radius: 10px;
            color: white;
            font-size: 1.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            user-select: none;
        }
        
        .up-btn {
            grid-column: 2;
            grid-row: 1;
        }
        
        .left-btn {
            grid-column: 1;
            grid-row: 2;
        }
        
        .right-btn {
            grid-column: 3;
            grid-row: 2;
        }
        
        .down-btn {
            grid-column: 2;
            grid-row: 2;
        }
        
        .center-btn {
            grid-column: 2;
            grid-row: 2;
            background-color: transparent;
        }
        
        .game-over {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.85);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            z-index: 10;
            display: none;
        }
        
        .game-over h2 {
            font-size: 3rem;
            color: #ff4757;
            margin-bottom: 20px;
        }
        
        .game-over p {
            font-size: 1.5rem;
            margin-bottom: 30px;
            color: #ff9a00;
        }
        
        @media (max-width: 768px) {
            .game-container {
                padding: 15px;
            }
            
            .controls {
                flex-direction: column;
                align-items: center;
            }
            
            .btn {
                width: 100%;
                max-width: 300px;
            }
            
            .mobile-controls {
                display: grid;
            }
            
            .instructions p {
                font-size: 0.95rem;
            }
        }
        
        @media (max-width: 480px) {
            h1 {
                font-size: 2.2rem;
            }
            
            .info-value {
                font-size: 1.8rem;
            }
            
            #game-canvas {
                width: 300px;
                height: 300px;
            }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>贪食蛇小游戏</h1>
        <p class="subtitle">控制小蛇吃掉食物，避免撞到墙壁或自己的身体！</p>
    </div>
    
    <div class="game-container">
        <div class="game-info">
            <div class="score-container">
                <div class="info-label">当前得分</div>
                <div class="info-value" id="score">0</div>
            </div>
            <div class="high-score-container">
                <div class="info-label">最高分</div>
                <div class="info-value" id="high-score">0</div>
            </div>
            <div class="level-container">
                <div class="info-label">等级</div>
                <div class="info-value" id="level">1</div>
            </div>
        </div>
        
        <div class="game-area">
            <canvas id="game-canvas" width="600" height="450"></canvas>
            
            <div class="game-over" id="game-over">
                <h2>游戏结束!</h2>
                <p>得分: <span id="final-score">0</span></p>
                <button class="btn" id="restart-btn-2">重新开始</button>
            </div>
        </div>
        
        <div class="controls">
            <button class="btn" id="start-btn">开始游戏</button>
            <button class="btn" id="pause-btn">暂停游戏</button>
            <button class="btn" id="restart-btn">重新开始</button>
        </div>
        
        <div class="mobile-controls">
            <button class="mobile-btn up-btn" id="up-btn">↑</button>
            <button class="mobile-btn left-btn" id="left-btn">←</button>
            <div class="center-btn"></div>
            <button class="mobile-btn right-btn" id="right-btn">→</button>
            <button class="mobile-btn down-btn" id="down-btn">↓</button>
        </div>
        
        <div class="instructions">
            <h3>游戏说明</h3>
            <p>使用 <span class="key">方向键</span> 或 <span class="key">WASD</span> 控制蛇的移动方向</p>
            <p>每吃到一个食物 <span style="color:#ff9a00">●</span> 获得10分，每得100分升一级</p>
            <p>等级越高，蛇移动速度越快</p>
            <p>避免撞到墙壁或自己的身体</p>
            <p>点击"开始游戏"按钮开始，游戏过程中可以暂停</p>
        </div>
    </div>

    <script>
        // 获取Canvas和Context
        const canvas = document.getElementById('game-canvas');
        const ctx = canvas.getContext('2d');

        // 游戏变量
        let snake = [];
        let food = {};
        let direction = 'right';
        let nextDirection = 'right';
        let gameSpeed = 120; // 初始速度（毫秒）
        let gameInterval;
        let score = 0;
        let highScore = localStorage.getItem('snakeHighScore') || 0;
        let level = 1;
        let gameRunning = false;
        let gamePaused = false;

        // 游戏元素尺寸
        const gridSize = 15;
        const gridWidth = canvas.width / gridSize;
        const gridHeight = canvas.height / gridSize;

        // DOM元素
        const scoreElement = document.getElementById('score');
        const highScoreElement = document.getElementById('high-score');
        const levelElement = document.getElementById('level');
        const startBtn = document.getElementById('start-btn');
        const pauseBtn = document.getElementById('pause-btn');
        const restartBtn = document.getElementById('restart-btn');
        const restartBtn2 = document.getElementById('restart-btn-2');
        const gameOverScreen = document.getElementById('game-over');
        const finalScoreElement = document.getElementById('final-score');

        // 移动控制按钮
        const upBtn = document.getElementById('up-btn');
        const downBtn = document.getElementById('down-btn');
        const leftBtn = document.getElementById('left-btn');
        const rightBtn = document.getElementById('right-btn');

        // 初始化游戏
        function initGame() {
            // 初始化蛇
            snake = [
                {x: 5, y: 10},
                {x: 4, y: 10},
                {x: 3, y: 10}
            ];
            
            // 初始化方向
            direction = 'right';
            nextDirection = 'right';
            
            // 初始分数和等级
            score = 0;
            level = 1;
            gameSpeed = 120;
            
            // 更新显示
            scoreElement.textContent = score;
            levelElement.textContent = level;
            highScoreElement.textContent = highScore;
            
            // 生成第一个食物
            generateFood();
            
            // 隐藏游戏结束画面
            gameOverScreen.style.display = 'none';
        }

        // 生成食物
        function generateFood() {
            // 随机生成食物位置
            food = {
                x: Math.floor(Math.random() * gridWidth),
                y: Math.floor(Math.random() * gridHeight)
            };
            
            // 确保食物不会生成在蛇身上
            for (let segment of snake) {
                if (segment.x === food.x && segment.y === food.y) {
                    return generateFood();
                }
            }
        }

        // 绘制游戏元素
        function drawGame() {
            // 清除画布
            ctx.fillStyle = '#0d1b2a';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // 绘制网格线
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.05)';
            ctx.lineWidth = 1;
            
            // 垂直线
            for (let x = 0; x <= canvas.width; x += gridSize) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }
            
            // 水平线
            for (let y = 0; y <= canvas.height; y += gridSize) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width, y);
                ctx.stroke();
            }
            
            // 绘制蛇
            snake.forEach((segment, index) => {
                // 蛇头用不同颜色
                if (index === 0) {
                    ctx.fillStyle = '#4dffea'; // 蛇头颜色
                } else {
                    // 蛇身渐变颜色
                    const colorValue = 150 + Math.floor(105 * (index / snake.length));
                    ctx.fillStyle = `rgb(50, ${colorValue}, 100)`;
                }
                
                // 绘制圆角矩形作为蛇身
                drawRoundedRect(
                    segment.x * gridSize, 
                    segment.y * gridSize, 
                    gridSize - 1, 
                    gridSize - 1, 
                    3
                );
                
                // 绘制蛇眼睛（只在头部）
                if (index === 0) {
                    ctx.fillStyle = '#000';
                    
                    // 根据方向绘制眼睛位置
                    let eyeOffsetX = 0;
                    let eyeOffsetY = 0;
                    
                    if (direction === 'right') {
                        eyeOffsetX = 10;
                        eyeOffsetY = 4;
                    } else if (direction === 'left') {
                        eyeOffsetX = 4;
                        eyeOffsetY = 4;
                    } else if (direction === 'up') {
                        eyeOffsetX = 4;
                        eyeOffsetY = 4;
                    } else if (direction === 'down') {
                        eyeOffsetX = 4;
                        eyeOffsetY = 10;
                    }
                    
                    // 绘制两只眼睛
                    ctx.beginPath();
                    ctx.arc(
                        segment.x * gridSize + eyeOffsetX,
                        segment.y * gridSize + eyeOffsetY,
                        2, 0, Math.PI * 2
                    );
                    ctx.fill();
                    
                    ctx.beginPath();
                    ctx.arc(
                        segment.x * gridSize + eyeOffsetX,
                        segment.y * gridSize + gridSize - eyeOffsetY,
                        2, 0, Math.PI * 2
                    );
                    ctx.fill();
                }
            });
            
            // 绘制食物
            ctx.fillStyle = '#ff9a00';
            ctx.beginPath();
            ctx.arc(
                food.x * gridSize + gridSize/2,
                food.y * gridSize + gridSize/2,
                gridSize/2 - 2, 0, Math.PI * 2
            );
            ctx.fill();
            
            // 添加食物光晕效果
            ctx.shadowColor = '#ff9a00';
            ctx.shadowBlur = 10;
            ctx.fill();
            ctx.shadowBlur = 0;
        }

        // 绘制圆角矩形函数
        function drawRoundedRect(x, y, width, height, radius) {
            ctx.beginPath();
            ctx.moveTo(x + radius, y);
            ctx.lineTo(x + width - radius, y);
            ctx.quadraticCurveTo(x + width, y, x + width, y + radius);
            ctx.lineTo(x + width, y + height - radius);
            ctx.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
            ctx.lineTo(x + radius, y + height);
            ctx.quadraticCurveTo(x, y + height, x, y + height - radius);
            ctx.lineTo(x, y + radius);
            ctx.quadraticCurveTo(x, y, x + radius, y);
            ctx.closePath();
            ctx.fill();
        }

        // 更新游戏状态
        function updateGame() {
            // 更新蛇的方向
            direction = nextDirection;
            
            // 根据方向计算新的蛇头位置
            const head = {x: snake[0].x, y: snake[0].y};
            
            switch(direction) {
                case 'up':
                    head.y -= 1;
                    break;
                case 'down':
                    head.y += 1;
                    break;
                case 'left':
                    head.x -= 1;
                    break;
                case 'right':
                    head.x += 1;
                    break;
            }
            
            // 检查是否撞墙
            if (head.x < 0 || head.x >= gridWidth || head.y < 0 || head.y >= gridHeight) {
                gameOver();
                return;
            }
            
            // 检查是否撞到自己
            for (let segment of snake) {
                if (head.x === segment.x && head.y === segment.y) {
                    gameOver();
                    return;
                }
            }
            
            // 将新的头部添加到蛇身
            snake.unshift(head);
            
            // 检查是否吃到食物
            if (head.x === food.x && head.y === food.y) {
                // 增加分数
                score += 10;
                scoreElement.textContent = score;
                
                // 更新最高分
                if (score > highScore) {
                    highScore = score;
                    highScoreElement.textContent = highScore;
                    localStorage.setItem('snakeHighScore', highScore);
                }
                
                // 检查是否升级
                const newLevel = Math.floor(score / 100) + 1;
                if (newLevel > level) {
                    level = newLevel;
                    levelElement.textContent = level;
                    
                    // 增加游戏速度
                    gameSpeed = Math.max(50, 120 - (level - 1) * 10);
                    
                    // 重新设置游戏间隔
                    if (gameRunning && !gamePaused) {
                        clearInterval(gameInterval);
                        gameInterval = setInterval(updateGame, gameSpeed);
                    }
                }
                
                // 生成新食物
                generateFood();
            } else {
                // 如果没有吃到食物，移除蛇尾
                snake.pop();
            }
            
            // 重绘画布
            drawGame();
        }

        // 游戏结束
        function gameOver() {
            gameRunning = false;
            clearInterval(gameInterval);
            
            // 显示游戏结束画面
            finalScoreElement.textContent = score;
            gameOverScreen.style.display = 'flex';
            
            // 更新按钮文本
            startBtn.textContent = '开始游戏';
        }

        // 开始游戏
        function startGame() {
            if (!gameRunning) {
                gameRunning = true;
                gamePaused = false;
                startBtn.textContent = '游戏中...';
                
                // 如果蛇已经死亡，重新初始化游戏
                if (snake.length === 0) {
                    initGame();
                }
                
                // 开始游戏循环
                gameInterval = setInterval(updateGame, gameSpeed);
                drawGame();
            }
        }

        // 暂停/继续游戏
        function togglePause() {
            if (!gameRunning) return;
            
            if (gamePaused) {
                // 继续游戏
                gamePaused = false;
                pauseBtn.textContent = '暂停游戏';
                gameInterval = setInterval(updateGame, gameSpeed);
            } else {
                // 暂停游戏
                gamePaused = true;
                pauseBtn.textContent = '继续游戏';
                clearInterval(gameInterval);
            }
        }

        // 重新开始游戏
        function restartGame() {
            clearInterval(gameInterval);
            gameRunning = false;
            gamePaused = false;
            startBtn.textContent = '开始游戏';
            pauseBtn.textContent = '暂停游戏';
            initGame();
            drawGame();
        }

        // 键盘控制
        function handleKeyDown(e) {
            // 防止方向键滚动页面
            if ([37, 38, 39, 40, 65, 87, 68, 83].includes(e.keyCode)) {
                e.preventDefault();
            }
            
            // 根据按键设置方向
            switch(e.keyCode) {
                case 38: // 上箭头
                case 87: // W键
                    if (direction !== 'down') nextDirection = 'up';
                    break;
                case 40: // 下箭头
                case 83: // S键
                    if (direction !== 'up') nextDirection = 'down';
                    break;
                case 37: // 左箭头
                case 65: // A键
                    if (direction !== 'right') nextDirection = 'left';
                    break;
                case 39: // 右箭头
                case 68: // D键
                    if (direction !== 'left') nextDirection = 'right';
                    break;
                case 32: // 空格键 - 暂停/继续
                    togglePause();
                    break;
                case 13: // 回车键 - 开始游戏
                    if (!gameRunning) startGame();
                    break;
                case 82: // R键 - 重新开始
                    restartGame();
                    break;
            }
        }

        // 移动设备控制
        function setupMobileControls() {
            upBtn.addEventListener('click', () => {
                if (direction !== 'down') nextDirection = 'up';
            });
            
            downBtn.addEventListener('click', () => {
                if (direction !== 'up') nextDirection = 'down';
            });
            
            leftBtn.addEventListener('click', () => {
                if (direction !== 'right') nextDirection = 'left';
            });
            
            rightBtn.addEventListener('click', () => {
                if (direction !== 'left') nextDirection = 'right';
            });
        }

        // 事件监听
        startBtn.addEventListener('click', startGame);
        pauseBtn.addEventListener('click', togglePause);
        restartBtn.addEventListener('click', restartGame);
        restartBtn2.addEventListener('click', restartGame);

        // 初始化游戏
        initGame();
        drawGame();
        
        // 设置移动控制
        setupMobileControls();
        
        // 监听键盘事件
        document.addEventListener('keydown', handleKeyDown);
        
        // 显示最高分
        highScoreElement.textContent = highScore;
        
        // 添加触摸事件支持，防止移动端拖动
        document.addEventListener('touchmove', function(e) {
            if (e.target.classList.contains('mobile-btn')) {
                e.preventDefault();
            }
        }, { passive: false });
    </script>
</body>
</html>