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

  <button id="startBtn">Start</button>
  <button id="hitBtn">Hit</button>
  <button id="standBtn">Stand</button>

  <p id="status">Setze Geld und starte.</p>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
  let money = parseInt(localStorage.getItem("blackjackMoney")) || 1000;
  let bet = 0;
  let player = 0;
  let dealer = 0;
  let inGame = false;

  const moneyEl = document.getElementById("money");
  const statusEl = document.getElementById("status");
  const startBtn = document.getElementById("startBtn");
  const hitBtn = document.getElementById("hitBtn");
  const standBtn = document.getElementById("standBtn");
  const betInput = document.getElementById("bet");

  function draw() {
    return Math.floor(Math.random() * 10) + 1;
  }

  function updateMoney() {
    moneyEl.innerText = money;
    localStorage.setItem("blackjackMoney", money);
  }

  function setStatus(text, color = "") {
    statusEl.innerText = text;
    statusEl.style.color = color;
  }

  function resetRound() {
    // Spiel beenden, aber Status nicht überschreiben
    player = 0;
    dealer = 0;
    inGame = false;
  }

  startBtn.addEventListener("click", function() {
    if (inGame) {
      setStatus("Runde läuft bereits!", "orange");
      return;
    }

    bet = parseInt(betInput.value);
    if (!bet || bet < 1) {
      setStatus("Einsatz muss mindestens 1 sein.", "red");
      return;
    }

    if (bet > money) {
      setStatus("Du hast nicht genug Geld.", "red");
      return;
    }

    player = draw() + draw();
    dealer = draw();
    inGame = true;

    setStatus("Du: " + player + " | Dealer zeigt: " + dealer, "black");
  });

  hitBtn.addEventListener("click", function() {
    if (!inGame) return;

    player += draw();

    if (player > 21) {
      money -= bet;
      updateMoney();
      setStatus("Bust! Du verlierst " + bet, "red");
      resetRound(); // nur Runde zurücksetzen, Text bleibt
    } else {
      setStatus("Du: " + player + " | Dealer zeigt: " + dealer, "black");
    }
  });

  standBtn.addEventListener("click", function() {
    if (!inGame) return;

    while (dealer < 17) dealer += draw();

    if (dealer > 21 || player > dealer) {
      money += bet;
      setStatus("Du gewinnst! +" + bet, "green");
    } else {
      money -= bet;
      setStatus("Dealer gewinnt. -" + bet, "red");
    }

    updateMoney();
    resetRound(); // Text bleibt sichtbar
  });

  updateMoney();
});
</script>
