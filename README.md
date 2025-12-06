<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>宇宙ドングリ落とし</title>
<style>
  body {
    margin: 0;
    background: #000;
    color: #fff;
    font-family: sans-serif;
    text-align: center;
    overflow: hidden;
  }
  #game {
    position: relative;
    width: 100vw;
    height: 100vh;
  }
  #player {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    width: 80px;
    height: 60px;
    background: pink;
    border-radius: 40px;
  }
  .drop {
    position: absolute;
    width: 20px;
    height: 20px;
    background: yellow;
    border-radius: 50%;
  }
  #score, #timer {
    font-size: 24px;
    margin-top: 10px;
  }
</style>
</head>
<body>
<div id="score">スコア: 0</div>
<div id="timer">残り: 30秒</div>

<!-- ←ここに広告を入れ放題 -->

<div id="game"></div>

<script>
const game = document.getElementById("game");
const player = document.createElement("div");
player.id = "player";
game.appendChild(player);

let score = 0;
let time = 30;

function updateScore() {
  document.getElementById("score").innerText = "スコア: " + score;
}

function updateTimer() {
  document.getElementById("timer").innerText = "残り: " + time + "秒";
}

// ドングリ落下
function drop() {
  const d = document.createElement("div");
  d.className = "drop";
  d.style.left = Math.random() * window.innerWidth + "px";
  d.style.top = "0px";
  game.appendChild(d);

  const fall = setInterval(() => {
    const t = parseInt(d.style.top);
    d.style.top = t + 5 + "px";

    // 当たり判定
    const px = player.getBoundingClientRect();
    const dx = d.getBoundingClientRect();

    if (
      dx.bottom >= px.top &&
      dx.left < px.right &&
      dx.right > px.left
    ) {
      score++;
      updateScore();
      clearInterval(fall);
      d.remove();
    }

    // 画面外
    if (t > window.innerHeight) {
      clearInterval(fall);
      d.remove();
    }
  }, 20);
}

// タップで落とす
game.addEventListener("click", drop);

// 30秒タイマー
const timer = setInterval(() => {
  time--;
  updateTimer();
  if (time <= 0) {
    clearInterval(timer);
    alert("終了！ スコア：" + score);
  }
}, 1000);
</script>
</body>
</html>
