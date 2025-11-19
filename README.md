<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Type Dash!</title>
  <style>
    * {
      box-sizing: border-box;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: radial-gradient(circle at top, #1e293b, #020617);
      color: #e5e7eb;
    }

    .game-wrapper {
      width: 100%;
      max-width: 720px;
      padding: 24px;
    }

    .game-card {
      background: rgba(15, 23, 42, 0.95);
      border-radius: 18px;
      box-shadow: 0 20px 45px rgba(0, 0, 0, 0.6);
      padding: 24px 24px 28px;
      border: 1px solid rgba(148, 163, 184, 0.3);
    }

    h1 {
      margin: 0 0 4px;
      font-size: 1.7rem;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    h1 span.logo-dot {
      display: inline-block;
      width: 10px;
      height: 10px;
      border-radius: 999px;
      background: #22c55e;
      box-shadow: 0 0 12px #22c55e;
    }

    .subtitle {
      font-size: 0.9rem;
      color: #9ca3af;
      margin-bottom: 16px;
    }

    .top-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      margin-bottom: 16px;
    }

    .stats {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      font-size: 0.9rem;
    }

    .stat-pill {
      padding: 6px 10px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(55, 65, 81, 0.9);
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .stat-label {
      color: #9ca3af;
    }

    .stat-value {
      font-weight: 600;
    }

    .button {
      border: none;
      border-radius: 999px;
      padding: 9px 18px;
      font-size: 0.95rem;
      font-weight: 600;
      cursor: pointer;
      background: linear-gradient(135deg, #22c55e, #16a34a);
      color: #0f172a;
      box-shadow: 0 10px 25px rgba(34, 197, 94, 0.4);
      transition: transform 0.1s ease, box-shadow 0.1s ease, filter 0.1s ease;
      white-space: nowrap;
    }

    .button:hover {
      transform: translateY(-1px);
      filter: brightness(1.05);
      box-shadow: 0 14px 30px rgba(34, 197, 94, 0.5);
    }

    .button:active {
      transform: translateY(1px) scale(0.98);
      box-shadow: 0 7px 15px rgba(34, 197, 94, 0.3);
    }

    .time-bar-container {
      margin-bottom: 16px;
      height: 10px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      overflow: hidden;
      border: 1px solid rgba(55, 65, 81, 0.9);
    }

    .time-bar {
      height: 100%;
      width: 100%;
      background: linear-gradient(90deg, #22c55e, #eab308, #ef4444);
      transform-origin: left center;
      transition: transform 0.25s linear;
    }

    .word-area {
      background: radial-gradient(circle at top, #020617, #020617);
      border-radius: 14px;
      border: 1px solid rgba(55, 65, 81, 0.9);
      padding: 18px 16px;
      text-align: center;
      position: relative;
      overflow: hidden;
      margin-bottom: 16px;
    }

    .word-area::after {
      content: "";
      position: absolute;
      inset: 0;
      pointer-events: none;
      background: radial-gradient(circle at top, rgba(59, 130, 246, 0.12), transparent 55%);
      opacity: 0.7;
    }

    .word-label {
      font-size: 0.85rem;
      color: #64748b;
      margin-bottom: 8px;
      position: relative;
      z-index: 1;
    }

    .word-display {
      font-size: 1.7rem;
      letter-spacing: 0.06em;
      position: relative;
      z-index: 1;
      user-select: none;
    }

    #typed-part {
      color: #22c55e;
      text-shadow: 0 0 8px rgba(34, 197, 94, 0.6);
    }

    #remaining-part {
      color: #e5e7eb;
    }

    #caret {
      display: inline-block;
      width: 1px;
      background: #e5e7eb;
      height: 1.4em;
      vertical-align: text-bottom;
      margin-left: 1px;
      animation: blink 0.9s steps(1, end) infinite;
    }

    @keyframes blink {
      0%, 50% { opacity: 1; }
      50.01%, 100% { opacity: 0; }
    }

    .hint {
      font-size: 0.8rem;
      color: #9ca3af;
      text-align: center;
      margin-top: 4px;
    }

    .message {
      margin-top: 10px;
      text-align: center;
      font-size: 0.9rem;
      min-height: 1.1em;
    }

    .message.ok {
      color: #22c55e;
    }

    .message.bad {
      color: #f97316;
    }

    .game-over {
      color: #f97316;
      font-weight: 600;
    }

    .shake {
      animation: shake 0.2s linear;
    }

    @keyframes shake {
      0% { transform: translateX(0); }
      25% { transform: translateX(-4px); }
      50% { transform: translateX(4px); }
      75% { transform: translateX(-3px); }
      100% { transform: translateX(0); }
    }

    @media (max-width: 540px) {
      .game-card {
        padding: 18px 14px 22px;
      }
      .word-display {
        font-size: 1.4rem;
      }
      .top-row {
        flex-direction: column;
        align-items: flex-start;
      }
      .button {
        width: 100%;
        justify-content: center;
        text-align: center;
      }
    }
  </style>
</head>
<body>
  <div class="game-wrapper">
    <div class="game-card">
      <div class="top-row">
        <div>
          <h1><span class="logo-dot"></span> Type Dash!</h1>
          <div class="subtitle">
            Type as many words as you can in <strong>60 seconds</strong>.<br>
            You’ll hear sounds for hits, mistakes, and completed words.
          </div>
        </div>
        <button class="button" id="startBtn">Start Game</button>
      </div>

      <div class="stats">
        <div class="stat-pill">
          <span class="stat-label">Time</span>
          <span class="stat-value" id="timeDisplay">60s</span>
        </div>
        <div class="stat-pill">
          <span class="stat-label">Score</span>
          <span class="stat-value" id="scoreDisplay">0</span>
        </div>
        <div class="stat-pill">
          <span class="stat-label">Words</span>
          <span class="stat-value" id="wordsDisplay">0</span>
        </div>
        <div class="stat-pill">
          <span class="stat-label">Accuracy</span>
          <span class="stat-value" id="accuracyDisplay">100%</span>
        </div>
      </div>

      <div class="time-bar-container">
        <div class="time-bar" id="timeBar"></div>
      </div>

      <div class="word-area" id="wordArea">
        <div class="word-label" id="statusLabel">Press “Start Game” to begin</div>
        <div class="word-display">
          <span id="typed-part"></span><span id="remaining-part"></span><span id="caret"></span>
        </div>
      </div>

      <div class="hint">Just type on your keyboard – no input box needed.</div>
      <div class="message" id="message"></div>
    </div>
  </div>

  <script>
    // Word list
    const WORDS = [
      "keyboard", "monitor", "javascript", "rocket", "galaxy", "ocean",
      "piano", "thunder", "banana", "velocity", "matrix", "quantum",
      "shadow", "penguin", "laptop", "syntax", "cosmic", "whisper",
      "dragon", "nebula", "typing", "rhythm", "pixel", "battery",
      "firefly", "gravity", "wizard", "orange", "buffer", "algorithm",
      "coffee", "random", "server", "network", "router", "crypto",
      "future", "portal", "galactic", "asteroid", "magnet", "fusion",
      "virtual", "signal", "cursor", "dynamic", "electric", "throttle"
    ];

    // UI elements
    const startBtn = document.getElementById("startBtn");
    const timeDisplay = document.getElementById("timeDisplay");
    const scoreDisplay = document.getElementById("scoreDisplay");
    const wordsDisplay = document.getElementById("wordsDisplay");
    const accuracyDisplay = document.getElementById("accuracyDisplay");
    const typedSpan = document.getElementById("typed-part");
    const remainingSpan = document.getElementById("remaining-part");
    const statusLabel = document.getElementById("statusLabel");
    const wordArea = document.getElementById("wordArea");
    const message = document.getElementById("message");
    const timeBar = document.getElementById("timeBar");

    // Game state
    let currentWord = "";
    let currentIndex = 0;
    let score = 0;
    let wordsCompleted = 0;
    let totalKeystrokes = 0;
    let correctKeystrokes = 0;
    let timeLeft = 60;
    let timerId = null;
    let isPlaying = false;

    // Audio
    let audioCtx = null;

    function ensureAudioContext() {
      if (!audioCtx) {
        const Ctx = window.AudioContext || window.webkitAudioContext;
        if (Ctx) {
          audioCtx = new Ctx();
        }
      } else if (audioCtx.state === "suspended") {
        audioCtx.resume();
      }
    }

    function playTone(freq, duration, type = "sine", volume = 0.3) {
      ensureAudioContext();
      if (!audioCtx) return;

      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      const now = audioCtx.currentTime;

      osc.type = type;
      osc.frequency.setValueAtTime(freq, now);

      gain.gain.setValueAtTime(0.0001, now);
      gain.gain.exponentialRampToValueAtTime(volume, now + 0.01);
      gain.gain.exponentialRampToValueAtTime(0.0001, now + duration);

      osc.connect(gain);
      gain.connect(audioCtx.destination);

      osc.start(now);
      osc.stop(now + duration + 0.03);
    }

    function playSound(kind) {
      switch (kind) {
        case "correct":
          // short high beep
          playTone(880, 0.08, "square", 0.25);
          break;
        case "wrong":
          // lower buzz
          playTone(160, 0.14, "sawtooth", 0.35);
          break;
        case "word":
          // small success jingle (two notes)
          playTone(660, 0.12, "triangle", 0.28);
          setTimeout(() => playTone(990, 0.16, "triangle", 0.32), 120);
          break;
        case "end":
          // descending game-over
          playTone(520, 0.18, "square", 0.3);
          setTimeout(() => playTone(330, 0.2, "square", 0.26), 170);
          setTimeout(() => playTone(220, 0.22, "square", 0.22), 360);
          break;
      }
    }

    function pickRandomWord() {
      return WORDS[Math.floor(Math.random() * WORDS.length)];
    }

    function updateWordDisplay() {
      typedSpan.textContent = currentWord.slice(0, currentIndex);
      remainingSpan.textContent = currentWord.slice(currentIndex);
    }

    function updateStatsDisplay() {
      timeDisplay.textContent = timeLeft + "s";
      scoreDisplay.textContent = score;
      wordsDisplay.textContent = wordsCompleted;

      const accuracy = totalKeystrokes === 0
        ? 100
        : Math.round((correctKeystrokes / totalKeystrokes) * 100);
      accuracyDisplay.textContent = accuracy + "%";

      const fraction = timeLeft / 60;
      timeBar.style.transform = `scaleX(${fraction})`;
    }

    function setMessage(text, type = "") {
      message.textContent = text;
      message.classList.remove("ok", "bad", "game-over");
      if (type) message.classList.add(type);
    }

    function startGame() {
      ensureAudioContext();

      isPlaying = true;
      score = 0;
      wordsCompleted = 0;
      totalKeystrokes = 0;
      correctKeystrokes = 0;
      timeLeft = 60;

      currentWord = pickRandomWord();
      currentIndex = 0;

      updateWordDisplay();
      updateStatsDisplay();
      statusLabel.textContent = "Type this word:";
      setMessage("Go! 🎯", "ok");

      wordArea.classList.remove("shake");

      if (timerId) clearInterval(timerId);
      timerId = setInterval(tick, 1000);

      startBtn.textContent = "Restart";
    }

    function endGame() {
      isPlaying = false;
      if (timerId) {
        clearInterval(timerId);
        timerId = null;
      }

      playSound("end");

      const accuracy = totalKeystrokes === 0
        ? 100
        : Math.round((correctKeystrokes / totalKeystrokes) * 100);

      statusLabel.textContent = "Time's up!";
      setMessage(
        `Final score: ${score} • Words: ${wordsCompleted} • Accuracy: ${accuracy}%`,
        "game-over"
      );
    }

    function tick() {
      if (!isPlaying) return;

      timeLeft -= 1;
      if (timeLeft <= 0) {
        timeLeft = 0;
        updateStatsDisplay();
        endGame();
      } else {
        updateStatsDisplay();
      }
    }

    function handleKey(e) {
      if (!isPlaying) return;

      // Ignore modifier keys etc.
      if (e.key.length !== 1) {
        return;
      }

      e.preventDefault(); // avoid scrolling on space etc.

      const pressed = e.key.toLowerCase();

      // Only letters allowed for scoring
      if (!/^[a-z]$/.test(pressed)) {
        return;
      }

      totalKeystrokes++;

      const expected = currentWord[currentIndex];

      if (pressed === expected.toLowerCase()) {
        // Correct character
        currentIndex++;
        correctKeystrokes++;
        playSound("correct");
        setMessage("", "");

        if (currentIndex >= currentWord.length) {
          // Word completed
          wordsCompleted++;
          score += 10;
          playSound("word");
          currentWord = pickRandomWord();
          currentIndex = 0;
          statusLabel.textContent = "Nice! Next word:";
        }

        updateWordDisplay();
        updateStatsDisplay();
      } else {
        // Wrong character
        score = Math.max(0, score - 1);
        playSound("wrong");
        setMessage("Oops, wrong letter!", "bad");
        updateStatsDisplay();

        // Little shake animation
        wordArea.classList.remove("shake");
        void wordArea.offsetWidth; // force reflow
        wordArea.classList.add("shake");
      }
    }

    // Event listeners
    startBtn.addEventListener("click", startGame);
    document.addEventListener("keydown", handleKey);
  </script>
</body>
</html>
