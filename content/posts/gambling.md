---
title: "Black-Jack"
markup: "html"
weight: 105
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
document.addEventListener("DOMContentLoaded", function () {
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
    player = 0;
    dealer = 0;
    inGame = false;
  }

  startBtn.addEventListener("click", function () {
    if (inGame) {
      setStatus("Runde läuft bereits.");
      return;
    }

    bet = parseInt(betInput.value);

    if (!bet || bet < 1) {
      setStatus("Einsatz muss mindestens 1 sein.");
      return;
    }

    if (bet > money) {
      setStatus("Du hast nicht genug Geld.");
      return;
    }

    player = draw() + draw();
    dealer = draw();
    inGame = true;

    setStatus("Du: " + player + " | Dealer zeigt: " + dealer);
  });

  hitBtn.addEventListener("click", function () {
    if (!inGame) return;

    player += draw();

    if (player > 21) {
      money -= bet;
      updateMoney();
      setStatus("Bust! Du verlierst " + bet, "red");
      resetRound();
    } else {
      setStatus("Du: " + player + " | Dealer zeigt: " + dealer);
    }
  });

  standBtn.addEventListener("click", function () {
    if (!inGame) return;

    let dealerCards = [];

    while (dealer < 17) {
      let card = draw();
      dealer += card;
      dealerCards.push(card);
    }

    if (dealer > 21 || player > dealer) {
      money += bet;
      setStatus(
        "Du gewinnst! +" + bet +
        " | Dealer zog: " + dealerCards.join(", ") +
        " (Summe: " + dealer + ")",
        "green"
      );
    } else if (player === dealer) {
      setStatus(
        "Unentschieden. Einsatz zurück." +
        " | Dealer zog: " + dealerCards.join(", ") +
        " (Summe: " + dealer + ")",
        "orange"
      );
    } else {
      money -= bet;
      setStatus(
        "Dealer gewinnt. -" + bet +
        " | Dealer zog: " + dealerCards.join(", ") +
        " (Summe: " + dealer + ")",
        "red"
      );
    }

    updateMoney();
    resetRound();
  });

  updateMoney();
});
</script>
