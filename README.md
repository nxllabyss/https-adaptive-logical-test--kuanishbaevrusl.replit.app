[index.html](https://github.com/user-attachments/files/25487758/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Adaptive Logical Test</title>
<style>
  :root {
    --bg-color: #121212;
    --card-bg: #1e1e1e;
    --text-color: #e0e0e0;
    --primary-color: #bb86fc;
    --primary-hover: #9965f4;
    --danger-color: #cf6679;
    --success-color: #03dac6;
    --border-color: #333;
  }
  body {
    background-color: var(--bg-color);
    color: var(--text-color);
    font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
  }
  #app {
    background-color: var(--card-bg);
    border-radius: 12px;
    padding: 30px;
    width: 90%;
    max-width: 600px;
    box-shadow: 0 8px 24px rgba(0,0,0,0.5);
    border: 1px solid var(--border-color);
  }
  h1, h2 {
    text-align: center;
    margin-top: 0;
  }
  .hidden {
    display: none !important;
  }
  .form-group {
    margin-bottom: 20px;
  }
  label {
    display: block;
    margin-bottom: 8px;
    font-weight: 600;
  }
  select {
    width: 100%;
    padding: 10px;
    background-color: var(--bg-color);
    color: var(--text-color);
    border: 1px solid var(--border-color);
    border-radius: 6px;
    font-size: 16px;
  }
  button {
    width: 100%;
    padding: 12px;
    background-color: var(--primary-color);
    color: #000;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.2s, transform 0.1s;
  }
  button:hover {
    background-color: var(--primary-hover);
  }
  button:active {
    transform: scale(0.98);
  }
  .header-info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    font-size: 14px;
    border-bottom: 1px solid var(--border-color);
    padding-bottom: 10px;
  }
  .timers {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    font-weight: bold;
  }
  .question-container {
    margin-bottom: 30px;
  }
  .question-text {
    font-size: 18px;
    margin-bottom: 20px;
  }
  .options {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .option-btn {
    background-color: var(--bg-color);
    color: var(--text-color);
    border: 1px solid var(--border-color);
    text-align: left;
    padding: 15px;
    font-weight: normal;
  }
  .option-btn:hover {
    background-color: #2a2a2a;
  }
  .option-btn.selected {
    background-color: rgba(187, 134, 252, 0.2);
    border-color: var(--primary-color);
  }
  #next-btn {
    margin-top: 20px;
  }
  .result-stat {
    font-size: 18px;
    margin: 10px 0;
    display: flex;
    justify-content: space-between;
  }
  .badge {
    padding: 4px 8px;
    border-radius: 4px;
    font-size: 12px;
    font-weight: bold;
    text-transform: uppercase;
  }
  .badge-easy { background-color: var(--success-color); color: #000; }
  .badge-medium { background-color: #ffb300; color: #000; }
  .badge-hard { background-color: var(--danger-color); color: #000; }
  .badge-adaptive { background-color: var(--primary-color); color: #000; }
</style>
</head>
<body>

<div id="app">
  <!-- Start Screen -->
  <div id="start-screen">
    <h1>Adaptive Logical Test</h1>
    
    <div class="form-group">
      <label for="difficulty">Difficulty</label>
      <select id="difficulty">
        <option value="adaptive">Adaptive Mode</option>
        <option value="easy">Easy</option>
        <option value="medium">Medium</option>
        <option value="hard">Hard</option>
      </select>
    </div>

    <div class="form-group">
      <label for="question-count">Number of Questions</label>
      <select id="question-count">
        <option value="10">10 Questions</option>
        <option value="20">20 Questions</option>
        <option value="30">30 Questions</option>
      </select>
    </div>

    <div class="form-group">
      <label for="timer-mode">Timer Mode</label>
      <select id="timer-mode">
        <option value="practice">Practice Mode (No time limit per question)</option>
        <option value="timed">Timed Mode (60s per question)</option>
      </select>
    </div>

    <button onclick="startTest()">Start Test</button>
  </div>

  <!-- Test Screen -->
  <div id="test-screen" class="hidden">
    <div class="header-info">
      <span id="question-tracker">Question 1 / 10</span>
      <span id="level-tracker" class="badge badge-medium">MEDIUM</span>
      <span id="score-tracker">Score: 0</span>
    </div>

    <div class="timers">
      <span id="total-timer">Total Time: 00:00</span>
      <span id="question-timer" class="hidden">Time left: 60s</span>
    </div>

    <div class="question-container">
      <div id="question-text" class="question-text">Question goes here...</div>
      <div id="options-container" class="options">
        <!-- Options will be injected here -->
      </div>
    </div>

    <button id="next-btn" onclick="submitAnswer()" disabled style="opacity: 0.5; cursor: not-allowed;">Next Question</button>
  </div>

  <!-- Result Screen -->
  <div id="result-screen" class="hidden">
    <h2>Test Complete!</h2>
    
    <div style="margin: 30px 0;">
      <div class="result-stat">
        <span>Final Score:</span>
        <span id="final-score">0 / 10</span>
      </div>
      <div class="result-stat">
        <span>Percentage:</span>
        <span id="final-percentage">0%</span>
      </div>
      <div class="result-stat">
        <span>Performance Level:</span>
        <span id="final-level" style="font-weight: bold; color: var(--primary-color);">Beginner</span>
      </div>
      <div class="result-stat">
        <span>Total Time Taken:</span>
        <span id="final-time">00:00</span>
      </div>
    </div>

    <button onclick="location.reload()">Restart Test</button>
  </div>
</div>

<script>
  // Question Bank
  const easyQuestions = [
    { q: "Which number comes next: 2, 4, 6, 8, ?", options: ["9", "10", "11", "12"], a: 1 },
    { q: "If A=1, B=2, C=3, what is D?", options: ["3", "4", "5", "6"], a: 1 },
    { q: "What is the opposite of North?", options: ["East", "West", "South", "Up"], a: 2 },
    { q: "Sun is to Day as Moon is to ?", options: ["Night", "Star", "Space", "Dark"], a: 0 },
    { q: "Which is the odd one out?", options: ["Apple", "Banana", "Carrot", "Car"], a: 3 },
    { q: "10 - 3 = ?", options: ["6", "7", "8", "9"], a: 1 },
    { q: "Cat is to Kitten as Dog is to ?", options: ["Puppy", "Cub", "Doggy", "Pet"], a: 0 },
    { q: "How many sides does a triangle have?", options: ["2", "3", "4", "5"], a: 1 },
    { q: "What comes after Monday?", options: ["Sunday", "Tuesday", "Wednesday", "Thursday"], a: 1 },
    { q: "Water is solid when it is?", options: ["Steam", "Liquid", "Ice", "Cloud"], a: 2 },
    { q: "Which animal is known as man's best friend?", options: ["Cat", "Dog", "Horse", "Bird"], a: 1 },
    { q: "What color is the sky on a clear day?", options: ["Green", "Blue", "Red", "Yellow"], a: 1 },
    { q: "If you have 2 apples and eat 1, how many are left?", options: ["0", "1", "2", "3"], a: 1 },
    { q: "Which is a fruit?", options: ["Potato", "Onion", "Apple", "Carrot"], a: 2 },
    { q: "How many legs does a spider have?", options: ["6", "8", "10", "12"], a: 1 }
  ];

  const mediumQuestions = [
    { q: "Which number comes next: 1, 1, 2, 3, 5, 8, ?", options: ["11", "12", "13", "14"], a: 2 },
    { q: "If APPLE is coded as BQQMF, how is DOG coded?", options: ["EPH", "FPI", "EPG", "DPH"], a: 0 },
    { q: "A man walks 5 miles North, then turns right and walks 5 miles. What direction is he facing?", options: ["North", "South", "East", "West"], a: 2 },
    { q: "How many months have 28 days?", options: ["1", "2", "6", "12"], a: 3 },
    { q: "Find the missing number: 8, 16, 32, 64, ?", options: ["96", "112", "128", "144"], a: 2 },
    { q: "If you rearrange the letters CIFAIPC, you have the name of a(n):", options: ["City", "Animal", "Ocean", "River"], a: 2 },
    { q: "John is taller than Peter. Peter is shorter than Paul. Who is the tallest?", options: ["John", "Peter", "Paul", "Cannot be determined"], a: 3 },
    { q: "What gets wetter the more it dries?", options: ["Water", "Towel", "Sponge", "Air"], a: 1 },
    { q: "Which word does not belong?", options: ["Sing", "Talk", "Speak", "Hear"], a: 3 },
    { q: "If 5 = 25 and 6 = 36, then 7 = ?", options: ["42", "47", "49", "56"], a: 2 },
    { q: "Mary's father has 5 daughters: 1. Nana, 2. Nene, 3. Nini, 4. Nono. What is the name of the 5th?", options: ["Nunu", "Nina", "Mary", "None"], a: 2 },
    { q: "A farmer has 17 sheep and all but 9 die. How many are left?", options: ["8", "9", "17", "0"], a: 1 },
    { q: "Which word is an anagram of 'LISTEN'?", options: ["SILENT", "INLETS", "ENLIST", "All of the above"], a: 3 },
    { q: "What runs around the whole yard without moving?", options: ["Dog", "Fence", "Tree", "Child"], a: 1 },
    { q: "What has a head and a tail but no body?", options: ["Coin", "Snake", "Arrow", "Monkey"], a: 0 }
  ];

  const hardQuestions = [
    { q: "Which number comes next: 2, 6, 12, 20, 30, ?", options: ["40", "42", "44", "46"], a: 1 },
    { q: "If 3 cats catch 3 mice in 3 minutes, how many cats to catch 100 mice in 100 minutes?", options: ["3", "33", "100", "300"], a: 0 },
    { q: "A bat and ball cost $1.10. The bat costs $1.00 more than the ball. How much is the ball?", options: ["$0.05", "$0.10", "$0.15", "$0.20"], a: 0 },
    { q: "It takes 5 machines 5 min to make 5 widgets. How long for 100 machines to make 100 widgets?", options: ["5 min", "20 min", "100 min", "500 min"], a: 0 },
    { q: "A patch of lily pads doubles every day. It covers the lake in 48 days. How long to cover half?", options: ["24 days", "47 days", "46 days", "12 days"], a: 1 },
    { q: "I speak without a mouth and hear without ears. I come alive with wind. What am I?", options: ["Cloud", "Tree", "Echo", "Bird"], a: 2 },
    { q: "A boat is filled with people, yet not a single person is on board. How?", options: ["It's sinking", "They are married", "They are ghosts", "It's a submarine"], a: 1 },
    { q: "What has keys but can't open locks?", options: ["Map", "Piano", "Treasure", "Door"], a: 1 },
    { q: "The day before two days after the day before tomorrow is Saturday. What day is today?", options: ["Thursday", "Friday", "Saturday", "Sunday"], a: 1 },
    { q: "Find the odd one out:", options: ["Hexagon", "Heptagon", "Octagon", "Cylinder"], a: 3 },
    { q: "If you divide 30 by half and add ten, what do you get?", options: ["25", "40", "70", "15"], a: 2 },
    { q: "You are in a race and you overtake the person in second place. What place are you in?", options: ["First", "Second", "Third", "Last"], a: 1 },
    { q: "How many times can you subtract 10 from 100?", options: ["10", "1", "9", "100"], a: 1 },
    { q: "What goes up but never comes down?", options: ["Age", "Bird", "Sun", "Balloon"], a: 0 },
    { q: "I have branches, but no fruit, trunk or leaves. What am I?", options: ["Bank", "Tree", "River", "Road"], a: 0 }
  ];

  // State
  let config = { difficulty: 'adaptive', count: 10, timed: false };
  let state = {
    currentQIndex: 0,
    score: 0,
    currentLevel: 2, // 1: Easy, 2: Medium, 3: Hard
    usedQuestions: { easy: [], medium: [], hard: [] },
    selectedAnswer: null,
    totalSeconds: 0,
    qSecondsLeft: 60,
    totalTimerInterval: null,
    qTimerInterval: null,
    currentQuestionData: null
  };

  // Helper: Get random question that hasn't been used
  function getRandomQuestion(level) {
    let pool, usedArray;
    if (level === 1) { pool = easyQuestions; usedArray = state.usedQuestions.easy; }
    else if (level === 2) { pool = mediumQuestions; usedArray = state.usedQuestions.medium; }
    else { pool = hardQuestions; usedArray = state.usedQuestions.hard; }

    let available = pool.filter((_, i) => !usedArray.includes(i));
    
    // If we run out of questions in a difficulty, reset the used questions for that difficulty
    if (available.length === 0) {
      usedArray.length = 0;
      available = pool;
    }

    const randIndex = Math.floor(Math.random() * available.length);
    const actualIndex = pool.indexOf(available[randIndex]);
    usedArray.push(actualIndex);
    return available[randIndex];
  }

  // Formatting time
  function formatTime(secs) {
    const m = Math.floor(secs / 60).toString().padStart(2, '0');
    const s = (secs % 60).toString().padStart(2, '0');
    return `${m}:${s}`;
  }

  // startTest() - start test based on selected mode
  function startTest() {
    config.difficulty = document.getElementById('difficulty').value;
    config.count = parseInt(document.getElementById('question-count').value);
    config.timed = document.getElementById('timer-mode').value === 'timed';

    if (config.difficulty === 'easy') state.currentLevel = 1;
    else if (config.difficulty === 'hard') state.currentLevel = 3;
    else state.currentLevel = 2; // Medium or Adaptive

    document.getElementById('start-screen').classList.add('hidden');
    document.getElementById('test-screen').classList.remove('hidden');

    if (config.timed) {
      document.getElementById('question-timer').classList.remove('hidden');
    }

    startTimers();
    loadQuestion();
  }

  // startTimers() - start question and total timers
  function startTimers() {
    state.totalTimerInterval = setInterval(() => {
      state.totalSeconds++;
      document.getElementById('total-timer').innerText = `Total Time: ${formatTime(state.totalSeconds)}`;
    }, 1000);

    if (config.timed) {
      state.qTimerInterval = setInterval(() => {
        state.qSecondsLeft--;
        document.getElementById('question-timer').innerText = `Time left: ${state.qSecondsLeft}s`;
        if (state.qSecondsLeft <= 0) {
          submitAnswer(true); // timeout
        }
      }, 1000);
    }
  }

  // stopTimers() - stop timers
  function stopTimers() {
    clearInterval(state.totalTimerInterval);
    clearInterval(state.qTimerInterval);
  }

  // loadQuestion() - show question, answers, and current level
  function loadQuestion() {
    state.selectedAnswer = null;
    const nextBtn = document.getElementById('next-btn');
    nextBtn.disabled = true;
    nextBtn.style.opacity = '0.5';
    nextBtn.style.cursor = 'not-allowed';
    nextBtn.innerText = state.currentQIndex === config.count - 1 ? 'Finish Test' : 'Next Question';
    
    if (config.timed) {
      state.qSecondsLeft = 60;
      document.getElementById('question-timer').innerText = `Time left: 60s`;
    }

    document.getElementById('question-tracker').innerText = `Question ${state.currentQIndex + 1} / ${config.count}`;
    document.getElementById('score-tracker').innerText = `Score: ${state.score}`;

    const levelEl = document.getElementById('level-tracker');
    levelEl.className = 'badge';
    if (state.currentLevel === 1) { levelEl.innerText = 'EASY'; levelEl.classList.add('badge-easy'); }
    else if (state.currentLevel === 2) { levelEl.innerText = 'MEDIUM'; levelEl.classList.add('badge-medium'); }
    else { levelEl.innerText = 'HARD'; levelEl.classList.add('badge-hard'); }

    state.currentQuestionData = getRandomQuestion(state.currentLevel);
    document.getElementById('question-text').innerText = state.currentQuestionData.q;

    const optionsContainer = document.getElementById('options-container');
    optionsContainer.innerHTML = '';
    
    state.currentQuestionData.options.forEach((opt, idx) => {
      const btn = document.createElement('button');
      btn.className = 'option-btn';
      btn.innerText = opt;
      btn.onclick = () => selectOption(idx, btn);
      optionsContainer.appendChild(btn);
    });
  }

  // Handle selection
  function selectOption(idx, btn) {
    state.selectedAnswer = idx;
    const btns = document.querySelectorAll('.option-btn');
    btns.forEach(b => b.classList.remove('selected'));
    btn.classList.add('selected');
    
    const nextBtn = document.getElementById('next-btn');
    nextBtn.disabled = false;
    nextBtn.style.opacity = '1';
    nextBtn.style.cursor = 'pointer';
  }

  // checkAnswer() - check user's answer and proceed
  function submitAnswer(timeout = false) {
    let correct = false;
    if (!timeout && state.selectedAnswer === state.currentQuestionData.a) {
      correct = true;
      state.score++;
    }

    updateDifficulty(correct);

    state.currentQIndex++;
    if (state.currentQIndex >= config.count) {
      showResult();
    } else {
      loadQuestion();
    }
  }

  // updateDifficulty() - change difficulty if adaptive
  function updateDifficulty(correct) {
    if (config.difficulty === 'adaptive') {
      if (correct) {
        state.currentLevel = Math.min(3, state.currentLevel + 1);
      } else {
        state.currentLevel = Math.max(1, state.currentLevel - 1);
      }
    }
  }

  // showResult() - show result screen and score
  function showResult() {
    stopTimers();
    document.getElementById('test-screen').classList.add('hidden');
    document.getElementById('result-screen').classList.remove('hidden');

    const percentage = Math.round((state.score / config.count) * 100);
    document.getElementById('final-score').innerText = `${state.score} / ${config.count}`;
    document.getElementById('final-percentage').innerText = `${percentage}%`;
    document.getElementById('final-time').innerText = formatTime(state.totalSeconds);

    const levelEl = document.getElementById('final-level');
    if (percentage <= 30) {
      levelEl.innerText = 'Beginner';
      levelEl.style.color = 'var(--danger-color)';
    } else if (percentage <= 70) {
      levelEl.innerText = 'Intermediate';
      levelEl.style.color = '#ffb300';
    } else {
      levelEl.innerText = 'Advanced';
      levelEl.style.color = 'var(--success-color)';
    }
  }
</script>
</body>
</html>
