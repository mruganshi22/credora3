<!DOCTYPE html>
<html>
<head>
  <title>Dark Stopwatch with Laps</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 30px;
      background-color: #121212;
      color: #ffffff;
    }

    .time {
      font-size: 36px;
      margin-bottom: 20px;
      color: #ffffff;
    }

    button {
      margin: 5px;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      background-color: #333;
      color: #fff;
      transition: background-color 0.2s;
    }

    button:hover {
      background-color: #444;
    }

    #laps {
      margin-top: 20px;
      max-height: 200px;
      overflow-y: auto;
      border-top: 1px solid #444;
    }

    .lap {
      margin: 5px 0;
      padding: 5px;
      background-color: #1e1e1e;
      border-radius: 4px;
    }
  </style>
</head>
<body>

  <h1>Dark Stopwatch</h1>
  <div class="time" id="display">00:00:00.000</div>

  <button onclick="start()">Start</button>
  <button onclick="pause()">Pause</button>
  <button onclick="reset()">Reset</button>
  <button onclick="lap()">Lap</button>

  <div id="laps"></div>

  <script>
    let startTime = 0;
    let elapsedTime = 0;
    let timerInterval;
    let laps = [];
    let lastLapTime = 0;

    function formatTime(ms) {
      let milliseconds = ms % 1000;
      let totalSeconds = Math.floor(ms / 1000);
      let seconds = totalSeconds % 60;
      let minutes = Math.floor((totalSeconds / 60) % 60);
      let hours = Math.floor(totalSeconds / 3600);

      return (
        String(hours).padStart(2, '0') + ':' +
        String(minutes).padStart(2, '0') + ':' +
        String(seconds).padStart(2, '0') + '.' +
        String(milliseconds).padStart(3, '0')
      );
    }

    function updateDisplay() {
      const now = Date.now();
      const time = elapsedTime + (now - startTime);
      document.getElementById('display').innerText = formatTime(time);
    }

    function start() {
      if (!timerInterval) {
        startTime = Date.now();
        timerInterval = setInterval(updateDisplay, 10);
      }
    }

    function pause() {
      if (timerInterval) {
        clearInterval(timerInterval);
        elapsedTime += Date.now() - startTime;
        timerInterval = null;
      }
    }

    function reset() {
      clearInterval(timerInterval);
      startTime = 0;
      elapsedTime = 0;
      timerInterval = null;
      lastLapTime = 0;
      document.getElementById('display').innerText = "00:00:00.000";
      document.getElementById('laps').innerHTML = "";
      laps = [];
    }

    function lap() {
      const now = Date.now();
      const currentTime = elapsedTime + (now - startTime);
      const lapTime = currentTime - lastLapTime;
      lastLapTime = currentTime;
      laps.push({ lap: laps.length + 1, time: lapTime });

      const lapDiv = document.createElement("div");
      lapDiv.className = "lap";
      lapDiv.innerText = "Lap " + laps.length + ": " + formatTime(lapTime);
      document.getElementById("laps").prepend(lapDiv);
    }
  </script>

</body>
</html>
