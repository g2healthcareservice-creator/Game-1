<!DOCTYPE html>
<html lang="en">
<script type="module">
  // Import the functions you need from the SDKs you need
  import { initializeApp } from "https://www.gstatic.com/firebasejs/12.2.1/firebase-app.js";
  import { getAnalytics } from "https://www.gstatic.com/firebasejs/12.2.1/firebase-analytics.js";
  // TODO: Add SDKs for Firebase products that you want to use
  // https://firebase.google.com/docs/web/setup#available-libraries

  // Your web app's Firebase configuration
  // For Firebase JS SDK v7.20.0 and later, measurementId is optional
  const firebaseConfig = {
    apiKey: "AIzaSyCCV_AJDvPU0YKbwvAp2zFoZH6uBTygEEs",
    authDomain: "test-ac02c.firebaseapp.com",
    projectId: "test-ac02c",
    storageBucket: "test-ac02c.firebasestorage.app",
    messagingSenderId: "994343007380",
    appId: "1:994343007380:web:911eebae17faff17977081",
    measurementId: "G-1DFE0N2KV7"
  };

  // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  const analytics = getAnalytics(app);
</script>

// Save score to Firebase
db.collection("scores").add({
  name: playerName,
  score: score,
  timestamp: Date.now()
});
<head>
  <meta charset="UTF-8">
  <title>ICE BRIDGE Quiz</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f9ff; padding: 20px; }
    .quiz-container { max-width: 600px; margin: auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 0 10px #ccc; }
    h2 { text-align: center; }
    .question { display: none; margin-bottom: 15px; }
    button { background: #007BFF; color: white; border: none; padding: 10px 15px; border-radius: 5px; cursor: pointer; }
    button:hover { background: #0056b3; }
    #result { font-weight: bold; margin-top: 15px; }
    #leaderboard { margin-top: 20px; }
  </style>
</head>
<body>
  <div class="quiz-container">
    <h2>ICE BRIDGE Online Quiz</h2>

    <p id="timer">⏱️ Time left: 30s</p>

    <!-- Question 1 -->
    <div class="question" id="q1">
      <p>1. Why should you wash hands before eating??</p>
      <input type="radio" name="q1" value="a"> To look clean <br>
      <input type="radio" name="q1" value="b"> To remove germs <br>
      <input type="radio" name="q1" value="c"> To waste water <br>
      <input type="radio" name="q1" value="c"> None of these <br>
      <button onclick="checkAnswer('q1','b')">Submit</button>
    </div>

    <!-- Question 2 -->
    <div class="question" id="q2">
      <p>2. What is the emergency number in a hospital?</p>
      <input type="radio" name="q2" value="a"> 100 <br>
      <input type="radio" name="q2" value="b"> 102 <br>
      <input type="radio" name="q2" value="c"> 108 <br>
      <input type="radio" name="q2" value="c"> 101 <br>
      <button onclick="checkAnswer('q2','c')">Submit</button>
    </div>

    <!-- Question 3 -->
    <div class="question" id="q3">
      <p>3. Who should wear a mask in the hospital?</p>
      <input type="radio" name="q3" value="a"> Only doctors <br>
      <input type="radio" name="q3" value="b"> Patients, visitors, and staff <br>
      <input type="radio" name="q3" value="c"> Only nurses <br>
      <input type="radio" name="q3" value="c"> Nobody <br>
      <button onclick="checkAnswer('q3','b')">Submit</button>
    </div>

    <!-- Question 4 -->
    <div class="question" id="q4">
      <p>4. How can you help if you see a child in distress?</p>
      <input type="radio" name="q4" value="a"> Comfort them and call staff <br>
      <input type="radio" name="q4" value="b"> Ignore them <br>
      <input type="radio" name="q4" value="c"> Take a video <br>
      <input type="radio" name="q4" value="c"> Tell others but do nothing <br>
      <button onclick="checkAnswer('q4','a')">Submit</button>
    </div>

    <!-- Question 5 -->
    <div class="question" id="q5">
      <p>5. Why is patient safety important?</p>
      <input type="radio" name="q5" value="a"> To avoid medical errors <br>
      <input type="radio" name="q5" value="b"> To waste money <br>
      <input type="radio" name="q5" value="c"> To delay treatment <br>
      <input type="radio" name="q5" value="c"> For entertainment <br>
      <button onclick="checkAnswer('q5','a')">Submit</button>
    </div>

    <!-- Result -->
    <p id="result"></p>

    <!-- Leaderboard -->
    <div id="leaderboard">
      <h3>🏆 Leaderboard</h3>
      <ul id="scoresList"></ul>
    </div>
  </div>

  <script>
    let currentQuestion = 1;
    let timer;
    let timeLeft = 30;

    // show first question
    document.getElementById("q1").style.display = "block";
    startTimer();

    // ---------- TIMER ----------
    function startTimer() {
      timeLeft = 30;
      document.getElementById("timer").innerText = "⏱️ Time left: " + timeLeft + "s";
      clearInterval(timer);
      timer = setInterval(() => {
        timeLeft--;
        document.getElementById("timer").innerText = "⏱️ Time left: " + timeLeft + "s";
        if (timeLeft <= 0) {
          clearInterval(timer);
          endQuiz("⌛ Time up! Quiz ended.");
        }
      }, 1000);
    }

    // ---------- CHECK ANSWER ----------
    function checkAnswer(questionId, correctAnswer) {
      const selected = document.querySelector(`input[name="${questionId}"]:checked`);

      if (!selected) {
        alert("Please select an answer!");
        return;
      }

      if (selected.value === correctAnswer) {
        // Correct → next question
        document.getElementById(questionId).style.display = "none";
        currentQuestion++;
        const nextQuestion = document.getElementById("q" + currentQuestion);

        if (nextQuestion) {
          nextQuestion.style.display = "block";
          startTimer();
        } else {
          endQuiz("🎉 Congratulations! You completed the quiz!");
        }
      } else {
        endQuiz("❌ Wrong answer! Quiz ended.");
      }
    }

    // ---------- END QUIZ ----------
    function endQuiz(message) {
      document.querySelectorAll(".question").forEach(q => q.style.display = "none");
      clearInterval(timer);

      let score = currentQuestion - 1; // number of correct answers
      let playerName = prompt("Enter your name for the leaderboard:");
      if (playerName) saveScore(playerName, score);

      document.getElementById("result").innerText = message + " Your score: " + score;
    }

    // ---------- LEADERBOARD ----------
    function saveScore(name, score) {
      let scores = JSON.parse(localStorage.getItem("quizScores")) || [];
      scores.push({ name, score });
      scores.sort((a, b) => b.score - a.score); // high to low
      localStorage.setItem("quizScores", JSON.stringify(scores));
      displayScores();
    }

    function displayScores() {
      let scores = JSON.parse(localStorage.getItem("quizScores")) || [];
      let list = document.getElementById("scoresList");
      list.innerHTML = "";
      scores.forEach(s => {
        let li = document.createElement("li");
        li.innerText = `${s.name} — ${s.score}`;
        list.appendChild(li);
      });
    }

    // Show leaderboard on load
    displayScores();
  </script>
</body>
</html>
