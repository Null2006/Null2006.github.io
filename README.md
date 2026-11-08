<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>贪吃蛇</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{display:flex;flex-direction:column;align-items:center;padding:10px;background:#eee;font-family:system-ui}
#info{font-size:22px;margin-bottom:8px}
canvas{border:2px solid #333;background:#f5f5f5;touch-action:none}
#restart{margin-top:10px;padding:8px 20px;font-size:18px;cursor:pointer}
</style>
</head>
<body>
<div id="info">分数：<span id="score">0</span></div>
<canvas id="c" width="400" height="400"></canvas>
<button id="restart">重新开始</button>

<script>
const ctx = c.getContext('2d');
const s = 20;
let snake,dx,dy,food,over,score;

function resetGame(){
    snake = [{x:10,y:10}];
    dx=1; dy=0;
    food = {x:15,y:15};
    over = false;
    score = 0;
    document.getElementById('score').textContent = score;
}
resetGame();

// 键盘控制（电脑）
document.addEventListener('keydown',e=>{
    if(over) return;
    if(e.key==='W'&&dy!==1){dx=0;dy=-1}
    if(e.key==='S'&&dy!==-1){dx=0;dy=1}
    if(e.key==='A'&&dx!==1){dx=-1;dy=0}
    if(e.key==='D'&&dx!==-1){dx=1;dy=0}
})

// 触屏滑动（手机）
let touchStartX=0,touchStartY=0;
c.addEventListener('touchstart',e=>{
    touchStartX = e.touches[0].clientX;
    touchStartY = e.touches[0].clientY;
})
c.addEventListener('touchend',e=>{
    if(over) return;
    const endX = e.changedTouches[0].clientX;
    const endY = e.changedTouches[0].clientY;
    const diffX = endX - touchStartX;
    const diffY = endY - touchStartY;
    // 判断滑动方向
    if(Math.abs(diffX) > Math.abs(diffY)){
        if(diffX>15 && dx!==-1){dx=1;dy=0}
        if(diffX<-15 && dx!==1){dx=-1;dy=0}
    }else{
        if(diffY>15 && dy!==-1){dx=0;dy=1}
        if(diffY<-15 && dy!==1){dx=0;dy=-1}
    }
})

document.getElementById('restart').onclick = resetGame;

function draw(){
    if(over){
        ctx.fillStyle='#f33';
        ctx.font='30px sans-serif';
        ctx.fillText('游戏结束',130,200);
        return;
    }
    ctx.clearRect(0,0,c.width,c.height);
    //食物
    ctx.fillStyle='#f44';
    ctx.fillRect(food.x*s,food.y*s,s-1,s-1);
    //蛇
    ctx.fillStyle='#28a745';
    snake.forEach(p=>ctx.fillRect(p.x*s,p.y*s,s-1,s-1));

    let head = {x:snake[0].x+dx, y:snake[0].y+dy};
    //撞墙
    if(head.x<0||head.y<0||head.x>=20||head.y>=20) over=true;
    //撞自己
    for(let p of snake) if(p.x===head.x&&p.y===head.y) over=true;

    snake.unshift(head);
    if(head.x===food.x&&head.y===food.y){
        score += 10;
        document.getElementById('score').textContent = score;
        food = {x:Math.random()*20|0, y:Math.random()*20|0};
    }else{
        snake.pop();
    }
}
setInterval(draw,120);
</script>
</body>
</html>
