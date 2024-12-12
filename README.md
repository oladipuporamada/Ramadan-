<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Number Puzzle Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f8ff;
            text-align: center;
            padding: 20px;
        }

        .game-container {
            margin: auto;
            padding: 20px;
            background: #fff;
            border: 2px solid #ddd;
            width: 300px;
            border-radius: 10px;
            box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
        }

        h1 {
            font-size: 24px;
            color: #333;
        }

        .puzzle {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
        }

        .number {
            width: 50px;
            height: 50px;
            margin: 5px;
            background-color: #007bff;
            color: white;
            font-size: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            border-radius: 5px;
            user-select: none;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            color: #fff;
            background-color: #007bff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Number Puzzle Game</h1>
        <p>Rearrange the numbers in ascending order!</p>
        <div id="puzzle" class="puzzle"></div>
        <button onclick="checkOrder()">Check Order</button>
        <p id="message"></p>
        <label for="difficulty">Select Difficulty: </label>
        <select id="difficulty" onchange="resetGame()">
            <option value="3">Easy (3 Numbers)</option>
            <option value="5">Medium (5 Numbers)</option>
            <option value="7">Hard (7 Numbers)</option>
        </select>
    </div>

    <script>
        let numbers = [];
        let targetNumbers = [];
        let difficulty = 3;

        // Shuffle function
        function shuffleArray(arr) {
            for (let i = arr.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [arr[i], arr[j]] = [arr[j], arr[i]]; // Swap
            }
        }

        // Function to update puzzle display
        function updatePuzzle() {
            const puzzleContainer = document.getElementById("puzzle");
            puzzleContainer.innerHTML = '';
            numbers.forEach(num => {
                const numElement = document.createElement("div");
                numElement.classList.add("number");
                numElement.textContent = num;
                numElement.setAttribute("draggable", true);
                numElement.addEventListener("dragstart", (e) => {
                    e.dataTransfer.setData("text", e.target.textContent);
                });
                puzzleContainer.appendChild(numElement);
            });
        }

        // Function to allow number dragging and dropping
        function allowDrop(e) {
            e.preventDefault();
        }

        function drop(e) {
            e.preventDefault();
            const draggedNumber = e.dataTransfer.getData("text");
            const targetElement = e.target;

            if (targetElement.classList.contains("number") && targetElement !== e.target) {
                const draggedIndex = numbers.indexOf(parseInt(draggedNumber));
                const targetIndex = numbers.indexOf(parseInt(targetElement.textContent));
                numbers[draggedIndex] = parseInt(targetElement.textContent);
                numbers[targetIndex] = parseInt(draggedNumber);
                updatePuzzle();
            }
        }

        // Function to check if the numbers are in correct order
        function checkOrder() {
            const message = document.getElementById("message");
            if (JSON.stringify(numbers) === JSON.stringify(targetNumbers)) {
                message.textContent = "🎉 Congratulations! You solved the puzzle!";
                message.style.color = "green";
            } else {
                message.textContent = "❌ Try again! The order is incorrect.";
                message.style.color = "red";
            }
        }

        // Function to reset the game based on difficulty level
        function resetGame() {
            difficulty = parseInt(document.getElementById("difficulty").value);
            numbers = Array.from({ length: difficulty }, (_, i) => i + 1);
            shuffleArray(numbers);
            targetNumbers = [...numbers].sort((a, b) => a - b);
            updatePuzzle();
        }

        // Initial game setup
        window.onload = resetGame;
    </script>
</body>
</html>
