
<!DOCTYPE html>
<html lang="mn">
<head>
  <meta charset="UTF-8">
  <title>♻️ Хог Ангилах Тоглоом</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: #f0f8ff;
    }
    #score, #timer {
      font-size: 18px;
      margin-top: 20px;
    }
    #feedback {
      font-size: 20px;
      color: red;
    }
    .bin, .item {
      width: 100px;
      height: 100px;
      display: inline-block;
      margin: 20px;
      padding: 10px;
      border: 2px solid #000;
      border-radius: 8px;
      background-size: cover;
      cursor: pointer;
    }
    .bin {
      background-color: #d0f0c0;
    }
    .item {
      background-color: #fff8dc;
    }
    .container {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
    }
  </style>
</head>
<body>

  <h2>♻️ Хог Ангилах Тоглоом</h2>
  <div id="score">Оноо: 0</div>
  <div id="timer">⏱ Цаг: 60 секунд</div>
  <div id="feedback"></div>

  <div class="container">
    <div class="bin" id="plasticBin" data-type="plastic">♻️ Хуванцар</div>
    <div class="bin" id="paperBin" data-type="paper">📄 Цаас</div>
    <div class="bin" id="organicBin" data-type="organic">🍌 Органик</div>
    <div class="bin" id="glassBin" data-type="glass">🍾 Шил</div>
    <div class="bin" id="metalBin" data-type="metal">🔩 Төмөр</div>
  </div>

  <div id="items"></div>

  <script>
    let score = 0;
    let timeLeft = 60;
    const trashItems = [
      { emoji: "🥤 Сав", type: "plastic" },
      { emoji: "📰 Сонин", type: "paper" },
      { emoji: "🍌 Бананы хальс", type: "organic" },
      { emoji: "🍾 Шилэн лонх", type: "glass" },
      { emoji: "🔩 Төмөр", type: "metal" }
    ];

    function showRandomTrash() {
      const item = trashItems[Math.floor(Math.random() * trashItems.length)];
      const div = document.createElement("div");
      div.className = "item";
      div.draggable = true;
      div.textContent = item.emoji;
      div.dataset.type = item.type;
      div.addEventListener("dragstart", e => {
        e.dataTransfer.setData("type", div.dataset.type);
      });
      document.getElementById("items").innerHTML = '';
      document.getElementById("items").appendChild(div);
    }

    const bins = document.querySelectorAll(".bin");
    bins.forEach(bin => {
      bin.addEventListener("dragover", e => e.preventDefault());
      bin.addEventListener("drop", e => {
        e.preventDefault();
        const type = e.dataTransfer.getData("type");
        const correctBin = bin.dataset.type === type;
        const feedback = document.getElementById("feedback");

        if (correctBin) {
          score += 10;
          feedback.textContent = "Зөв! Сайн ажиллалаа!";
        } else {
          score -= 5;
          feedback.textContent = "Буруу сав! Ойлгохыг хичээ!";
        }

        document.getElementById("score").textContent = "Оноо: " + score;
        showRandomTrash();
      });
    });

    function countdown() {
      const interval = setInterval(() => {
        timeLeft--;
        document.getElementById("timer").textContent = `⏱ Цаг: ${timeLeft} секунд`;
        if (timeLeft <= 0) {
          clearInterval(interval);
          document.getElementById("feedback").textContent = "Тоглоом дууслаа! Таны оноо: " + score;
        }
      }, 1000);
    }

    showRandomTrash();
    countdown();
  </script>

</body>
</html>

