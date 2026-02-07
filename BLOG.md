# 从零开始：用HTML5打造贪食蛇游戏并部署到GitHub Pages

## 前言

贪食蛇是许多人童年时期的经典游戏。今天，我将带你一起用纯前端技术栈，从零开始打造一个现代化的贪食蛇游戏，并将其部署到GitHub Pages，让全世界都能在线游玩。

## 技术栈

- **HTML5 Canvas** - 游戏画面渲染
- **CSS3** - 现代化UI设计（渐变、阴影、毛玻璃效果）
- **原生 JavaScript (ES6+)** - 游戏逻辑
- **GitHub Pages** - 免费静态网站托管

## 开发过程

### 第一步：项目规划

在开始编码前，我规划了游戏的核心功能：
- ✅ 流畅的蛇身移动
- ✅ 食物生成与吃食逻辑
- ✅ 碰撞检测（墙壁和自身）
- ✅ 实时分数统计
- ✅ 最高分本地存储
- ✅ 暂停/继续功能
- ✅ 现代化的视觉效果

### 第二步：画布设置

使用 HTML5 Canvas 作为游戏画布，设置 600x600 像素的游戏区域，采用 20x20 的网格系统：

```javascript
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const gridSize = 20;
const tileCount = canvas.width / gridSize; // 30x30网格
```

### 第三步：游戏核心逻辑

#### 蛇的数据结构
蛇身使用数组存储，每个元素代表一个身体段的坐标：

```javascript
let snake = [
    {x: 10, y: 10}, // 头部
    {x: 9, y: 10},  // 身体
    {x: 8, y: 10}   // 尾部
];
```

#### 移动机制
通过方向键控制移动方向，更新头部位置，并移除尾部实现移动效果：

```javascript
function updateGame() {
    const head = {
        x: snake[0].x + dx / gridSize, 
        y: snake[0].y + dy / gridSize
    };
    
    snake.unshift(head); // 添加新头部
    
    if (吃到食物) {
        score += 10;
        generateFood();
    } else {
        snake.pop(); // 移除尾部
    }
}
```

#### 碰撞检测
实现两种碰撞检测：

1. **墙壁碰撞**：检查头部坐标是否超出画布边界
2. **自身碰撞**：遍历蛇身，检查头部是否与其他段重叠

```javascript
// 撞墙检测
if (head.x < 0 || head.x >= tileCount || head.y < 0 || head.y >= tileCount) {
    gameOver();
    return;
}

// 自身碰撞检测
for (let segment of snake) {
    if (head.x === segment.x && head.y === segment.y) {
        gameOver();
        return;
    }
}
```

### 第四步：视觉效果设计

#### 渐变背景
使用 CSS3 的渐变功能打造现代化的视觉效果：

```css
body {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

#### 毛玻璃效果
为游戏容器添加毛玻璃效果，增强视觉层次：

```css
.game-container {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(4px);
    border: 1px solid rgba(255, 255, 255, 0.18);
    box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
}
```

#### 发光效果
蛇头和食物添加发光效果，提升游戏体验：

```javascript
ctx.shadowBlur = 10;
ctx.shadowColor = '#4CAF50'; // 绿色发光
```

### 第五步：细节优化

#### 蛇头方向指示
根据移动方向绘制眼睛，让玩家更清楚蛇的朝向：

```javascript
ctx.fillStyle = 'white';
if (dx > 0) { // 向右移动
    ctx.fillRect(headX + 12, headY + 5, 4, 4);
    ctx.fillRect(headX + 12, headY + 11, 4, 4);
}
```

#### 食物发光
食物使用圆形并添加红色发光效果：

```javascript
ctx.fillStyle = '#ff6b6b';
ctx.shadowBlur = 15;
ctx.shadowColor = '#ff6b6b';
ctx.beginPath();
ctx.arc(foodX, foodY, radius, 0, Math.PI * 2);
ctx.fill();
```

#### 本地存储最高分
使用 localStorage 持久化存储玩家的最高分记录：

```javascript
let highScore = localStorage.getItem('snakeHighScore') || 0;

// 更新最高分
if (score > highScore) {
    highScore = score;
    localStorage.setItem('snakeHighScore', highScore);
}
```

## 部署到 GitHub Pages

### 第一步：创建 GitHub 仓库

1. 在 GitHub 上创建新仓库 `snake-game-ai`
2. 设置为公开仓库
3. 初始化 README.md

### 第二步：推送代码

```bash
# 初始化本地仓库
git init

# 添加文件
git add .

# 提交代码
git commit -m "初始提交: 添加贪食蛇游戏"

# 关联远程仓库
git remote add origin https://github.com/username/snake-game-ai.git

# 推送代码
git push -u origin master
```

### 第三步：启用 GitHub Pages

1. 访问仓库的 Settings > Pages
2. Source 选择 `master` 分支，`/(root)` 目录
3. 点击 Save

等待 1-2 分钟后，游戏即可通过 `https://username.github.io/snake-game-ai` 访问。

## 遇到的挑战与解决方案

### 挑战 1：180度转向问题
**问题**：玩家可能快速按相反方向键导致蛇头直接撞向自己的身体。

**解决方案**：添加方向检查，禁止直接反向移动：

```javascript
case 'ArrowUp':
    if (dy === 0) { // 当前不是上下移动时才能向上
        dx = 0;
        dy = -gridSize;
    }
    break;
```

### 挑战 2：食物生成在蛇身上
**问题**：随机生成的食物可能出现在蛇身位置。

**解决方案**：递归检查并重生成：

```javascript
function generateFood() {
    food = {
        x: Math.floor(Math.random() * tileCount),
        y: Math.floor(Math.random() * tileCount)
    };
    
    // 检查是否在蛇身上
    for (let segment of snake) {
        if (segment.x === food.x && segment.y === food.y) {
            generateFood(); // 递归重试
            return;
        }
    }
}
```

### 挑战 3：游戏循环控制
**问题**：需要处理游戏状态（进行中、暂停、结束）的切换。

**解决方案**：使用状态变量和条件判断：

```javascript
let isPaused = false;
let isGameOver = false;

function updateGame() {
    if (isPaused || isGameOver) return;
    // 更新游戏逻辑
}
```

## 最终成果

🎮 **在线游玩**：https://alexbyyao.github.io/snake-game-ai

📁 **源码仓库**：https://github.com/AlexByYao/snake-game-ai

### 功能特性
- ✨ 流畅的游戏体验（100ms刷新率）
- 🎨 现代化渐变UI设计
- 👀 蛇头方向指示（眼睛动画）
- 💡 发光视觉效果
- 📊 实时分数和最高分记录
- ⏸️ 空格键暂停/继续
- 💾 本地存储最高分

## 总结

通过这个项目，我学习了：
- HTML5 Canvas 的绘图API
- JavaScript 游戏循环机制
- 碰撞检测算法
- CSS3 现代化视觉效果
- GitHub Pages 静态网站部署

这是一个非常适合初学者的练手项目，代码简洁易懂，但涵盖了前端开发的多个核心知识点。

## 后续优化方向

- 添加移动端触摸控制
- 实现不同难度级别
- 添加音效和背景音乐
- 实现双人模式
- 添加排行榜功能

---

*作者：Alex*  
*日期：2026年2月7日*  
*技术栈：HTML5 + CSS3 + JavaScript*
