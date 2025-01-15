const board = document.getElementById('board');
const statusText = document.getElementById('status');
let currentPlayer = 'X';
let gameActive = true;

// Tablica stan gry
let gameState = ['', '', '', '', '', '', '', '', ''];

// Kombinacje wygrywające
const winningConditions = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
];

// Budowanie planszy
function createBoard() {
    board.innerHTML = '';
    gameState = ['', '', '', '', '', '', '', '', ''];
    gameActive = true;
    currentPlayer = 'X';
    statusText.textContent = `Tura gracza: ${currentPlayer}`;
    for (let i = 0; i < 9; i++) {
        const cell = document.createElement('div');
        cell.classList.add('cell');
        cell.dataset.index = i;
        cell.addEventListener('click', handleCellClick);
        board.appendChild(cell);
    }
}

// Kliknięcie pola
function handleCellClick(event) {
    const cell = event.target;
    const index = parseInt(cell.dataset.index, 10);

    if (gameState[index] !== '' || !gameActive) return;

    gameState[index] = currentPlayer;
    cell.textContent = currentPlayer;
    cell.classList.add('taken', currentPlayer);

    if (checkWinner()) {
        statusText.textContent = `Gracz ${currentPlayer} wygrywa!`;
        gameActive = false;
        return;
    }

    if (gameState.every(cell => cell !== '')) {
        statusText.textContent = 'Remis!';
        gameActive = false;
        return;
    }

    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    statusText.textContent = `Tura gracza: ${currentPlayer}`;
}

// Sprawdzenie zwycięzcy
function checkWinner() {
    return winningConditions.some(condition => {
        const [a, b, c] = condition;
        return gameState[a] === currentPlayer &&
               gameState[a] === gameState[b] &&
               gameState[a] === gameState[c];
    });
}

// Reset gry
function resetGame() {
    createBoard();
}

// Inicjalizacja gry
createBoard();