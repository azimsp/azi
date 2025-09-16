# azi.msp / kjhkg

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Simple Ludo Game</title>
<style>
  body { font-family: sans-serif; text-align: center; background: #f2f2f2; }
  #board {
    display: grid;
    grid-template-columns: repeat(15, 40px);
    grid-template-rows: repeat(15, 40px);
    gap: 1px;
    margin: 20px auto;
  }
  .cell {
    width: 40px; height: 40px;
    background: white; border: 1px solid #ccc;
    display: flex; align-items: center; justify-content: center;
  }
  .p1 { background: red; border-radius: 50%; width: 30px; height: 30px; }
  .p2 { background: green; border-radius: 50%; width: 30px; height: 30px; }
  .p3 { background: blue; border-radius: 50%; width: 30px; height: 30px; }
  .p4 { background: yellow; border-radius: 50%; width: 30px; height: 30px; }
  #dice { font-size: 1.5em; margin: 15px; }
</style>
</head>
<body>

<h1>🎲 Ludo</h1>
<div id="board"></div>
<button onclick="rollDice()">🎲 Roll Dice</button>
<div id="dice">Turn: Player 1 | Dice: -</div>

<script>
const board = document.getElementById('board');
const diceDiv = document.getElementById('dice');

// Build the board
let cells = [];
for (let i = 0; i < 225; i++) {
  const cell = document.createElement('div');
  cell.classList.add('cell');
  cells.push(cell);
  board.appendChild(cell);
}

// Path indices — simplified (shorter) path for quick testing
const path = [0,1,2,3,4,5,6,7,8,9,10,
              25,40,55,70,85,100,115,130,145,160,175,
              174,173,172,171,170,169,168,167,166,165,164,
              149,134,119,104,89,74,59,44,29,14];

// Players and pawns (-1 means at home)
let players = [
  { color: 'p1', pawns: [-1,-1,-1,-1] },
  { color: 'p2', pawns: [-1,-1,-1,-1] },
  { color: 'p3', pawns: [-1,-1,-1,-1] },
  { color: 'p4', pawns: [-1,-1,-1,-1] }
];

let currentPlayer = 0;
let dice = 0;

// Render pieces on the board
function drawBoard() {
  cells.forEach(c => c.innerHTML = '');
  players.forEach((pl) => {
    pl.pawns.forEach(pos => {
      if (pos >= 0) {
        const pawn = document.createElement('div');
        pawn.className = pl.color;
        cells[path[pos]].appendChild(pawn);
      }
    });
  });
}

// Dice roll handler
function rollDice() {
  dice = Math.floor(Math.random() * 6) + 1;
  diceDiv.innerText = `Turn: Player ${currentPlayer+1} | Dice: ${dice}`;
  movePawn();
}

// Move logic (very simple, auto-selects the first available pawn)
function movePawn() {
  let player = players[currentPlayer];
  let moved = false;

  // If no pawn is on the board and dice is not 6 → skip turn
  if (!player.pawns.some(p => p >= 0) && dice !== 6) {
    nextTurn();
    return;
  }

  // Enter a pawn onto the board on a 6
  for (let i = 0; i < 4; i++) {
    if (player.pawns[i] === -1 && dice === 6) {
      player.pawns[i] = 0;
      moved = true;
      break;
    }
  }

  // Otherwise move the first pawn found on the board
  if (!moved) {
    for (let i = 0; i < 4; i++) {
      if (player.pawns[i] >= 0) {
        player.pawns[i] = (player.pawns[i] + dice) % path.length;
        hitCheck(player, i);
        moved = true;
        break;
      }
    }
  }

  drawBoard();
  if (dice !== 6) nextTurn();
}

// Send opponent pawn home if landed on the same path index
function hitCheck(player, pawnIndex) {
  let pos = player.pawns[pawnIndex];
  players.forEach(pl => {
    if (pl !== player) {
      pl.pawns = pl.pawns.map(p => (p === pos ? -1 :p ));
    }
  });
}

// Advance to next player's turn
function nextTurn() {
  currentPlayer = (currentPlayer + 1) % 4;
  diceDiv.innerText = `Turn: Player ${currentPlayer+1} | Dice: -`;
}

drawBoard();
</script>

</body>
</html>


