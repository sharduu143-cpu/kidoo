# kidoo


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kidoo - Kids Learning & Animal Arcade</title>
    <style>
        :root {
            --bg-color: #f0f8ff;
            --primary-color: #ff6b6b;
            --secondary-color: #4ecdc4;
            --accent-color: #ffe66d;
            --card-bg: #ffffff;
        }

        body {
            font-family: 'Comic Sans MS', 'Chalkboard SE', sans-serif;
            background-color: var(--bg-color);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        .brand-title {
            color: #ff4757;
            font-size: 2.8rem;
            margin: 0 0 5px 0;
            text-shadow: 3px 3px var(--accent-color);
            letter-spacing: 2px;
        }

        .tagline {
            color: #57606f;
            font-size: 1.1rem;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .nav-buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
            justify-content: center;
        }

        button {
            font-family: inherit;
            font-size: 1.1rem;
            font-weight: bold;
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: transform 0.1s, background-color 0.2s;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        button:active {
            transform: scale(0.95);
        }

        .nav-btn {
            background-color: var(--secondary-color);
            color: white;
        }

        .nav-btn.active {
            background-color: var(--primary-color);
        }

        .game-container {
            background: white;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
            min-width: 320px;
            max-width: 500px;
            width: 100%;
            box-sizing: border-box;
        }

        .game-screen {
            display: none;
            width: 100%;
            flex-direction: column;
            align-items: center;
        }

        .game-screen.active {
            display: flex;
        }

        .score-board {
            font-size: 1.3rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 15px;
        }

        /* Match Cards Game Styles */
        .card-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            width: 100%;
        }

        .card {
            aspect-ratio: 1;
            background-color: var(--secondary-color);
            color: white;
            font-size: 2rem;
            font-weight: bold;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 12px;
            cursor: pointer;
            user-select: none;
            transition: transform 0.3s;
            transform-style: preserve-3d;
        }

        .card.flipped {
            background-color: var(--accent-color);
            color: #333;
            transform: rotateY(180deg);
        }

        .card.matched {
            background-color: #87d37c;
            visibility: hidden;
        }

        /* Match-3 Animal Grid Styles */
        .match3-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 5px;
            background-color: #eee;
            padding: 10px;
            border-radius: 10px;
            touch-action: none;
        }

        .animal-cell {
            aspect-ratio: 1;
            background-color: white;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            cursor: pointer;
            user-select: none;
            box-shadow: inset 0 0 4px rgba(0,0,0,0.1);
        }

        .animal-cell.selected {
            border: 3px solid var(--primary-color);
            background-color: #fff3f3;
        }
    </style>
</head>
<body>

    <h1 class="brand-title">🎈 KIDOO 🐶</h1>
    <div class="tagline">Play, Learn & Match!</div>

    <div class="nav-buttons">
        <button class="nav-btn active" onclick="switchGame('letters')">Alphabet Match</button>
        <button class="nav-btn" onclick="switchGame('numbers')">Number Match</button>
        <button class="nav-btn" onclick="switchGame('animals')">Animal Match-3</button>
    </div>

    <div class="game-container">
        <!-- Alphabet Match Game -->
        <div id="letters-game" class="game-screen active">
            <div class="score-board">Letter Pairs Found: <span id="letter-score">0</span> / 4</div>
            <div class="card-grid" id="letter-grid"></div>
            <button style="margin-top: 15px; background-color: var(--primary-color); color: white;" onclick="startLetterGame()">Reset Game</button>
        </div>

        <!-- Number Match Game -->
        <div id="numbers-game" class="game-screen">
            <div class="score-board">Number Pairs Found: <span id="number-score">0</span> / 4</div>
            <div class="card-grid" id="number-grid"></div>
            <button style="margin-top: 15px; background-color: var(--primary-color); color: white;" onclick="startNumberGame()">Reset Game</button>
        </div>

        <!-- Animal Match-3 Game -->
        <div id="animals-game" class="game-screen">
            <div class="score-board">Score: <span id="animal-score">0</span></div>
            <div class="match3-grid" id="animal-grid"></div>
            <button style="margin-top: 15px; background-color: var(--primary-color); color: white;" onclick="startAnimalGame()">Reset Board</button>
        </div>
    </div>

    <script>
        // Game Switching
        function switchGame(gameType) {
            document.querySelectorAll('.game-screen').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(el => el.classList.remove('active'));

            if (gameType === 'letters') {
                document.getElementById('letters-game').classList.add('active');
                event.target.classList.add('active');
                startLetterGame();
            } else if (gameType === 'numbers') {
                document.getElementById('numbers-game').classList.add('active');
                event.target.classList.add('active');
                startNumberGame();
            } else if (gameType === 'animals') {
                document.getElementById('animals-game').classList.add('active');
                event.target.classList.add('active');
                startAnimalGame();
            }
        }

        /* --- Memory Matching Games Logic --- */
        function createMemoryGame(gridId, items, scoreId) {
            const grid = document.getElementById(gridId);
            grid.innerHTML = '';
            let score = 0;
            document.getElementById(scoreId).textContent = score;

            // Pick 4 random items and duplicate them for matching pairs
            const selected = [...items].sort(() => 0.5 - Math.random()).slice(0, 4);
            const deck = [...selected, ...selected].sort(() => 0.5 - Math.random());

            let firstCard = null;
            let secondCard = null;
            let lockBoard = false;

            deck.forEach(val => {
                const card = document.createElement('div');
                card.classList.add('card');
                card.dataset.value = val;
                card.textContent = '?';

                card.addEventListener('click', () => {
                    if (lockBoard || card === firstCard || card.classList.contains('matched')) return;

                    card.classList.add('flipped');
                    card.textContent = val;

                    if (!firstCard) {
                        firstCard = card;
                        return;
                    }

                    secondCard = card;
                    lockBoard = true;

                    if (firstCard.dataset.value === secondCard.dataset.value) {
                        setTimeout(() => {
                            firstCard.classList.add('matched');
                            secondCard.classList.add('matched');
                            score++;
                            document.getElementById(scoreId).textContent = score;
                            resetTurn();
                        }, 500);
                    } else {
                        setTimeout(() => {
                            firstCard.classList.remove('flipped');
                            secondCard.classList.remove('flipped');
                            firstCard.textContent = '?';
                            secondCard.textContent = '?';
                            resetTurn();
                        }, 1000);
                    }
                });

                grid.appendChild(card);
            });

            function resetTurn() {
                [firstCard, secondCard] = [null, null];
                lockBoard = false;
            }
        }

        function startLetterGame() {
            const letters = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'];
            createMemoryGame('letter-grid', letters, 'letter-score');
        }

        function startNumberGame() {
            const numbers = ['1', '2', '3', '4', '5', '6', '7', '8'];
            createMemoryGame('number-grid', numbers, 'number-score');
        }

        /* --- Animal Match-3 Game Logic --- */
        const animals = ['🐶', '🐱', '🐰', '🦊', '🐼', '🐵'];
        const gridWidth = 6;
        let animalGrid = [];
        let selectedCell = null;
        let match3Score = 0;

        function startAnimalGame() {
            const gridElement = document.getElementById('animal-grid');
            gridElement.innerHTML = '';
            animalGrid = [];
            match3Score = 0;
            document.getElementById('animal-score').textContent = match3Score;

            for (let i = 0; i < gridWidth * gridWidth; i++) {
                const cell = document.createElement('div');
                cell.classList.add('animal-cell');
                cell.dataset.id = i;
                
                let randomAnimal;
                do {
                    randomAnimal = animals[Math.floor(Math.random() * animals.length)];
                } while (wouldCreateMatch(i, randomAnimal));

                cell.textContent = randomAnimal;
                cell.addEventListener('click', handleCellClick);
                gridElement.appendChild(cell);
                animalGrid.push(cell);
            }
        }

        function wouldCreateMatch(index, animal) {
            const row = Math.floor(index / gridWidth);
            const col = index % gridWidth;

            if (col >= 2 && animalGrid[index - 1]?.textContent === animal && animalGrid[index - 2]?.textContent === animal) return true;
            if (row >= 2 && animalGrid[index - gridWidth]?.textContent === animal && animalGrid[index - (2 * gridWidth)]?.textContent === animal) return true;

            return false;
        }

        function handleCellClick(e) {
            const clickedCell = e.target;

            if (!selectedCell) {
                selectedCell = clickedCell;
                selectedCell.classList.add('selected');
            } else {
                const index1 = parseInt(selectedCell.dataset.id);
                const index2 = parseInt(clickedCell.dataset.id);

                const isAdjacent = Math.abs(index1 - index2) === 1 && Math.floor(index1 / gridWidth) === Math.floor(index2 / gridWidth) ||
                                   Math.abs(index1 - index2) === gridWidth;

                if (isAdjacent) {
                    swapCells(selectedCell, clickedCell);
                    if (!checkAndClearMatches()) {
                        setTimeout(() => swapCells(selectedCell, clickedCell), 200);
                    }
                }

                selectedCell.classList.remove('selected');
                selectedCell = null;
            }
        }

        function swapCells(cell1, cell2) {
            const temp = cell1.textContent;
            cell1.textContent = cell2.textContent;
            cell2.textContent = temp;
        }

        function checkAndClearMatches() {
            let matchedIndices = new Set();

            // Check rows
            for (let r = 0; r < gridWidth; r++) {
                for (let c = 0; c < gridWidth - 2; c++) {
                    let idx = r * gridWidth + c;
                    let val = animalGrid[idx].textContent;
                    if (val && val === animalGrid[idx + 1].textContent && val === animalGrid[idx + 2].textContent) {
                        matchedIndices.add(idx);
                        matchedIndices.add(idx + 1);
                        matchedIndices.add(idx + 2);
                    }
                }
            }

            // Check columns
            for (let c = 0; c < gridWidth; c++) {
                for (let r = 0; r < gridWidth - 2; r++) {
                    let idx = r * gridWidth + c;
                    let val = animalGrid[idx].textContent;
                    if (val && val === animalGrid[idx + gridWidth].textContent && val === animalGrid[idx + (2 * gridWidth)].textContent) {
                        matchedIndices.add(idx);
                        matchedIndices.add(idx + gridWidth);
                        matchedIndices.add(idx + (2 * gridWidth));
                    }
                }
            }

            if (matchedIndices.size > 0) {
                match3Score += matchedIndices.size * 10;
                document.getElementById('animal-score').textContent = match3Score;

                matchedIndices.forEach(idx => {
                    animalGrid[idx].textContent = animals[Math.floor(Math.random() * animals.length)];
                });

                setTimeout(checkAndClearMatches, 300);
                return true;
            }

            return false;
        }

        // Initialize default game
        startLetterGame();
    </script>
</body>
</html>




give the name kidoo