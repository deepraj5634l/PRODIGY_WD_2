# PRODIGY_WD_02

[index.html](https://github.com/user-attachments/files/24739945/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prodigy Precise Stopwatch</title>
    <style>
        /* CSS: Styling for a professional look */
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --text-main: #f8fafc;
            --accent: #3b82f6;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .container {
            background: var(--card-bg);
            padding: 2.5rem;
            border-radius: 20px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
            text-align: center;
            width: 320px;
        }

        .display {
            font-size: 3.5rem;
            font-weight: bold;
            margin-bottom: 2rem;
            font-variant-numeric: tabular-nums; /* Prevents text jumping */
            color: var(--accent);
        }

        .controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        button {
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        #startBtn { background: #22c55e; color: white; }
        #stopBtn { background: #ef4444; color: white; display: none; }
        #resetBtn { background: #64748b; color: white; }
        #lapBtn { background: var(--accent); color: white; grid-column: span 2; }

        button:hover { filter: brightness(1.1); transform: translateY(-1px); }
        button:active { transform: translateY(1px); }

        .laps-container {
            margin-top: 20px;
            max-height: 200px;
            overflow-y: auto;
            border-top: 1px solid #334155;
            padding-top: 10px;
        }

        .lap-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            font-size: 0.9rem;
            border-bottom: 1px solid #334155;
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="display" id="display">00:00:00</div>
        
        <div class="controls">
            <button id="startBtn">Start</button>
            <button id="stopBtn">Stop</button>
            <button id="resetBtn">Reset</button>
            <button id="lapBtn">Lap</button>
        </div>

        <div class="laps-container" id="laps">
            </div>
    </div>

    <script>
        // JavaScript Logic
        let startTime;
        let elapsedTime = 0;
        let timerInterval;

        const display = document.getElementById('display');
        const startBtn = document.getElementById('startBtn');
        const stopBtn = document.getElementById('stopBtn');
        const resetBtn = document.getElementById('resetBtn');
        const lapBtn = document.getElementById('lapBtn');
        const lapsList = document.getElementById('laps');

        function timeToString(time) {
            let diffInHrs = time / 3600000;
            let hh = Math.floor(diffInHrs);

            let diffInMin = (diffInHrs - hh) * 60;
            let mm = Math.floor(diffInMin);

            let diffInSec = (diffInMin - mm) * 60;
            let ss = Math.floor(diffInSec);

            let diffInMs = (diffInSec - ss) * 100;
            let ms = Math.floor(diffInMs);

            let formattedMM = mm.toString().padStart(2, "0");
            let formattedSS = ss.toString().padStart(2, "0");
            let formattedMS = ms.toString().padStart(2, "0");

            return `${formattedMM}:${formattedSS}:${formattedMS}`;
        }

        function showButton(buttonKey) {
            if (buttonKey === "STOP") {
                startBtn.style.display = "none";
                stopBtn.style.display = "block";
            } else {
                startBtn.style.display = "block";
                stopBtn.style.display = "none";
            }
        }

        function start() {
            startTime = Date.now() - elapsedTime;
            timerInterval = setInterval(function printTime() {
                elapsedTime = Date.now() - startTime;
                display.innerHTML = timeToString(elapsedTime);
            }, 10);
            showButton("STOP");
        }

        function stop() {
            clearInterval(timerInterval);
            showButton("START");
        }

        function reset() {
            clearInterval(timerInterval);
            display.innerHTML = "00:00:00";
            elapsedTime = 0;
            lapsList.innerHTML = "";
            showButton("START");
        }

        function lap() {
            if (elapsedTime > 0) {
                const lapTime = timeToString(elapsedTime);
                const lapItem = document.createElement('div');
                lapItem.classList.add('lap-item');
                lapItem.innerHTML = `<span>Lap ${lapsList.children.length + 1}</span> <span>${lapTime}</span>`;
                lapsList.prepend(lapItem); // Newest lap on top
            }
        }

        // Event Listeners
        startBtn.addEventListener("click", start);
        stopBtn.addEventListener("click", stop);
        resetBtn.addEventListener("click", reset);
        lapBtn.addEventListener("click", lap);
    </script>
</body>
</html>
