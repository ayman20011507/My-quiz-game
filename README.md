<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>لعبة من هذا اللاعب؟</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f0f0;
      text-align: center;
      padding: 20px;
    }
    .quiz-container {
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      max-width: 400px;
      margin: auto;
      box-shadow: 0 0 10px #ccc;
    }
    img {
      width: 100%;
      max-height: 250px;
      border-radius: 10px;
    }
    .letters {
      margin-top: 20px;
    }
    .letters button {
      padding: 10px;
      margin: 5px;
      font-size: 1.2em;
      cursor: pointer;
    }
    .answer-box {
      font-size: 1.5em;
      margin: 15px;
      min-height: 30px;
      border: 1px solid #aaa;
      padding: 5px;
    }
    .controls button {
      margin-top: 10px;
      padding: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }
    .controls button:hover {
      background-color: #0056b3;
    }
    .coins {
      margin-top: 10px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <h2>من هذا اللاعب؟</h2>
    <img id="playerImage" src="" alt="player" />
    <div class="answer-box" id="answerBox"></div>
    <div class="letters" id="letters"></div>

    <div class="controls">
      <button onclick="showAd('coin')">🎁 شاهد إعلان و احصل على نقطة</button><br>
      <button onclick="showAd('hint')">🧠 شاهد إعلان و احصل على حرف مساعد</button>
    </div>

    <div class="coins" id="coins">النقاط: 0</div>
  </div>

  <script>
    const levels = [
      {
        name: "رونالدو",
        image: "https://upload.wikimedia.org/wikipedia/commons/8/8c/Cristiano_Ronaldo_2018.jpg",
        letters: ["و", "ا", "م", "ل", "ه", "ر", "ل", "ن", "د", "و"]
      },
      {
        name: "ميسي",
        image: "https://upload.wikimedia.org/wikipedia/commons/8/89/Leo_Messi_vs_Nigeria_2018.jpg",
        letters: ["م", "ي", "س", "ت", "ا", "ق", "و", "ب"]
      },
      {
        name: "سواريز",
        image: "https://upload.wikimedia.org/wikipedia/commons/6/6c/Luis_Suarez_2018.jpg",
        letters: ["س", "و", "ا", "ر", "ي", "ز", "ن", "ك", "م"]
      }
    ];

    let currentLevel = 0;
    let userAnswer = "";
    let coins = 0;

    const imageEl = document.getElementById("playerImage");
    const lettersEl = document.getElementById("letters");
    const answerBox = document.getElementById("answerBox");
    const coinsEl = document.getElementById("coins");

    function loadLevel() {
      const level = levels[currentLevel];
      imageEl.src = level.image;
      answerBox.textContent = "";
      userAnswer = "";
      lettersEl.innerHTML = "";

      shuffleArray(level.letters).forEach(letter => {
        const btn = document.createElement("button");
        btn.textContent = letter;
        btn.onclick = () => {
          userAnswer += letter;
          answerBox.textContent = userAnswer;
          if (userAnswer === level.name) {
            coins += 3;
            coinsEl.textContent = `النقاط: ${coins}`;
            setTimeout(() => {
              alert("إجابة صحيحة! 👍");
              currentLevel++;
              if (currentLevel < levels.length) {
                loadLevel();
              } else {
                alert("انتهت اللعبة، مجموع نقاطك: " + coins);
              }
            }, 500);
          }
        };
        lettersEl.appendChild(btn);
      });
    }

    function showAd(type) {
      if (type === "coin") {
        coins += 1;
        coinsEl.textContent = `النقاط: ${coins}`;
        alert("🎉 حصلت على نقطة!");
      } else if (type === "hint") {
        const level = levels[currentLevel];
        const nextLetter = level.name[userAnswer.length];
        if (nextLetter) {
          userAnswer += nextLetter;
          answerBox.textContent = userAnswer;
        }
      }
    }

    function shuffleArray(arr) {
      return arr.sort(() => Math.random() - 0.5);
    }

    loadLevel();
  </script>
</body>
</html>
