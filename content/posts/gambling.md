---
title: "Black-Jack"
markup: "html"
weight: 5
---

<div id="game">
  <h2>Blackjack</h2>
  <p>Geld: <span id="money">1000</span></p>

  <input type="number" id="bet" placeholder="Einsatz" min="1">
  <br><br>

  <button onclick="startGame()">Start</button>
  <button onclick="hit()">Hit</button>
  <button onclick="stand()">Stand</button>

  <p id="status">Setze Geld und starte.</p>
</div>

<script>
let money = 1000;
let bet = 0;
let player = 0;
let dealer = 0;

function draw() {
  return Math.floor(Math.random() * 10) + 1;
}

function updateMoney() {
  document.getElementById("money").innerText = money;
}

function startGame() {
  bet = parseInt(document.getElementById("bet").value);
  if (!bet || bet <= 0 || bet > money) {
    document.getElementById("status").innerText = "Ungültiger Einsatz.";
    return;
  }

  player = draw() + draw();
  dealer = draw();
  document.getElementById("status").innerText =
    "Du: " + player + " | Dealer zeigt: " + dealer;
}

function hit() {
  player += draw();
  if (player > 21) {
    money -= bet;
    updateMoney();
    document.getElementById("status").innerText = "Bust! Du verlierst " + bet;
  } else {
    document.getElementById("status").innerText =
      "Du: " + player + " | Dealer zeigt: " + dealer;
  }
}

function stand() {
  while (dealer < 17) dealer += draw();

  if (dealer > 21 || player > dealer) {
    money += bet;
    updateMoney();
    document.getElementById("status").innerText =
      "Du gewinnst! +" + bet;
  } else {
    money -= bet;
    updateMoney();
    document.getElementById("status").innerText =
      "Dealer gewinnt. -" + bet;
  }
}
</script>
