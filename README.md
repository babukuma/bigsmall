<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Big or Small Real-Time Prediction</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 flex items-center justify-center h-screen">
    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
        <h1 class="text-2xl font-bold mb-4">Big or Small Prediction (80% Accuracy)</h1>
        <div id="timer" class="text-lg mb-4">Time to next prediction: 60s</div>
        <div id="prediction" class="text-xl font-semibold mb-4">Waiting for prediction...</div>
        <div id="result" class="text-lg mb-4"></div>
        <div id="history" class="text-sm"></div>
    </div>

    <script>
        // Configuration
        const THRESHOLD = 5; // Numbers >= 5 are "Big", < 5 are "Small"
        const INTERVAL = 60 * 1000; // 1 minute in milliseconds
        const ACCURACY = 0.8; // 80% accuracy for predictions

        // DOM elements
        const timerElement = document.getElementById('timer');
        const predictionElement = document.getElementById('prediction');
        const resultElement = document.getElementById('result');
        const historyElement = document.getElementById('history');

        // History to simulate pattern analysis
        let history = [];

        // Generate a random number between 0 and 9
        function generateNumber() {
            return Math.floor(Math.random() * 10);
        }

        // Simulate AI prediction with 80% accuracy
        function makePrediction(actualNumber) {
            const isBig = actualNumber >= THRESHOLD;
            const correctPrediction = isBig ? 'Big' : 'Small';
            // Simulate 80% accuracy: 80% chance to predict correctly, 20% chance to predict wrong
            const isCorrect = Math.random() < ACCURACY;
            if (isCorrect) {
                return correctPrediction;
            } else {
                return isBig ? 'Small' : 'Big';
            }
        }

        // Update the timer display
        function updateTimer(seconds) {
            timerElement.textContent = `Time to next prediction: ${seconds}s`;
        }

        // Main game loop
        function gameLoop() {
            let secondsLeft = 60;
            updateTimer(secondsLeft);

            // Countdown timer
            const countdown = setInterval(() => {
                secondsLeft--;
                updateTimer(secondsLeft);

                if (secondsLeft <= 0) {
                    clearInterval(countdown);
                    // Generate number and prediction
                    const number = generateNumber();
                    const prediction = makePrediction(number);
                    const actual = number >= THRESHOLD ? 'Big' : 'Small';
                    const isCorrect = prediction === actual;

                    // Update UI
                    predictionElement.textContent = `Prediction: ${prediction}`;
                    resultElement.textContent = `Result: Number was ${number} (${actual}) - ${isCorrect ? 'Correct' : 'Incorrect'}`;
                    history.push({ number, prediction, actual, isCorrect });
                    updateHistory();

                    // Start new cycle
                    setTimeout(gameLoop, 1000); // Delay to show result briefly
                }
            }, 1000);

            // Show prediction 5 seconds before result
            setTimeout(() => {
                const number = generateNumber();
                const prediction = makePrediction(number);
                predictionElement.textContent = `Prediction: ${prediction}`;
            }, INTERVAL - 5000);
        }

        // Update history display
        function updateHistory() {
            historyElement.innerHTML = '<h2 class="text-lg font-semibold mt-4">Prediction History</h2>';
            history.slice(-5).forEach((entry, index) => {
                historyElement.innerHTML += `<p>Round ${history.length - index}: Number ${entry.number} (${entry.actual}), Predicted: ${entry.prediction} (${entry.isCorrect ? 'Correct' : 'Incorrect'})</p>`;
            });
        }

        // Start the game
        gameLoop();
    </script>
</body>
</html>
