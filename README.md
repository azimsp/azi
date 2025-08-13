# azi

<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>بازی منچ آنلاین</title>
<style>
  body {
    font-family: sans-serif;
    text-align: center;
    background: #f2f2f2;
  }
  #board {
    display: grid;
    grid-template-columns: repeat(11, 50px);
    grid-template-rows: repeat(11, 50px);
    gap: 2px;
    margin: 20px auto;
  }
  .cell {
    width: 50px;
    height: 50px;
    background: white;
    border: 1px solid #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
  }
  .player1 { background: red; color: white; border-radius: 50%; }
  .player2 { background: green; color: white; border-radius: 50%; }
  .player3 { background: blue; color: white; border-radius: 50%; }
  .player4 { background: yellow; color: black; border-radius: 50%; }
  #dice {
    font-size: 2em;
    margin: 15px;
  }
</style>
</head>
<body>

<h1>🎲 بازی منچ</h1>
<div id="board"></div>
<button onclick="rollDice()">🎲 انداختن تاس</button>
<div id="dice">عدد تاس: -</div>

<script>
const board = document.getElementById('board');
const diceDiv = document.getElementById('dice');

// ایجاد خانه‌ها
for (let i = 0; i < 121; i++) {
  const cell = document.createElement('div');
  cell.classList.add('cell');
  board.appendChild(cell);
}

let dice = 0;
function rollDice() {
  dice = Math.floor(Math.random() * 6) + 1;
  diceDiv.innerText = "عدد تاس: " + dice;
}
</script>

</body>
</html>
