# Connect-Four
// This program simulates a two player Connect Four game on a 6x7 game board. Players take turns dropping their designated pieces (Red or Yellow) into the columns they select, with the goal to connect four of their own pieces horizontally, vertically, or diagonally. The game continues until a player wins or the board is completely filled, resulting in a tie. 

#include <iostream>

using namespace std;

// Defines the dimensions of the board: 6 rows and 7 columns
const int ROWS = 6;
const int COLS = 7;

// Function to initialize the board with empty spaces represented by '-'
void initializeBoard(char board[ROWS][COLS]) {
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            board[i][j] = '-'; // Sets each cell to empty
        }
    }
} 

// Function to dispaly the current state of the board
void displayBoard(char board[ROWS][COLS]) {
    // Prints each row of the board
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            cout << board[i][j] << " ";
        }
        cout << endl;
    }

    // Prints a separator and column numbers for user reference
    cout << "=============" << endl;
    for (int j = 1; j <= COLS; j++) {
        cout << j << " ";
    }
    cout << endl << endl;
}

// Checks if the top cell in a column is filled
bool isColumnFull(char board[ROWS][COLS], int col) {
    return board[0][col] != '-';
}

// Tries to place the player's piece in the given column
//Returns true if successfull, false if the move is invalid
bool placePiece(char board[ROWS][COLS], int col, char player) {
    if (col < 0 || col >= COLS || isColumnFull(board, col)) {
        return false;   // Invalid column or full column
    }

    // Starts from the bottom of the column and places the piece in the first empty spot
    for (int i = ROWS - 1; i >=0; i--) {
        if (board[i][col] == '-') { // Finds the first empty space in the column
            board[i][col] = player; // Places the player's piece
            return true; // Move successful
        }
    }
    return false;
}

// Checks if the current player has won the game
bool checkWin(char board[ROWS][COLS], char player) {
    // Checks horizontal wins
    for (int i = 0; i < ROWS; i++) {
        for(int j = 0; j < COLS - 3; j++) {
            if (board[i][j] == player && board[i][j + 1] == player && board[i][j + 2] == player && board[i][j + 3] == player) {
                return true;
            }
        }
    }

    // Checks vertical wins
    for (int i = 0; i < ROWS - 3; i++) {
        for (int j = 0; j < COLS; j++) {
            if (board[i][j] == player && board[i + 1][j] == player && board[i + 2][j] == player && board[i + 3][j] == player) {
                return true;
            }
        }
    }

    // Checks diagonal wins (bottom-left to top-right)
    for (int i = 3; i < ROWS; i++) {
        for (int j = 0; j < COLS - 3; j++) {
            if (board[i][j] == player && board[i - 1][j + 1] == player && board[i - 2][j+2] == player && board[i - 3][j + 3] == player) {
                return true;
            }
        }
    }

    // Checks diagonal wins (top-left to bottom-right)
    for (int i = 0; i < ROWS - 3; i++) {
        for (int j = 0; j < COLS - 3; j++) {
            if (board[i][j] == player && board[i + 1][j + 1] == player && board[i + 2][j + 2] == player && board[i + 3][j + 3] == player) {
                return true;
            }
        }
    }
    return false; // No win condition met
}

// Checks if the board is completely filled for a tied game
bool isBoardFull(char board[ROWS][COLS]) {
    for (int j = 0; j < COLS; j++) {
        if (board[0][j] == '-') {
            return false; // At least one column has space
        }
    }
    return true; // Board is full
}

// Main game loop
int main() 
{
    char board[ROWS][COLS]; // Creates the game board
    initializeBoard(board); // Sets all positions to empty
    char currentPlayer = 'R'; // Red starts the game
    int move;

    while (true) {
        displayBoard(board); // Shows the current board
        string playerName;
        
        // Determines the current player's name
        if (currentPlayer == 'R') {
            playerName = "Red";
        } else {
            playerName = "Yellow";
        }
        
        bool validMove = false;
        
        // Prompts the player until a valid move is made
        while (!validMove) {
            cout << "It is " << playerName << "'s turn." << endl;
            cout << "In which column would you like to move (-1 to exit)? " << endl;
            cin >> move;
            
            // Allows the player to exit the game
            if (move == -1) {
                return 0;
            }

            int col = move - 1;
            // Tries to place the piece, if invalid, prompts again
            if (col < 0 || col >= COLS || !placePiece(board, col, currentPlayer)) {
                cout << "Invalid move, try again." << endl;
                continue;
            } else {
                validMove = true; // Move was successful
            }
        }

        // Checks for a win after a move
        if (checkWin(board, currentPlayer)) {
            displayBoard(board); // Show final board
            if (currentPlayer == 'R') {
                playerName = "Red";
            } else {
                playerName = "Yellow";
            }
            cout << playerName << " wins!" << endl;
            break; // End the game
        }

        // Checks for a tie
        if (isBoardFull(board)) {
            displayBoard(board); // Shows final board
            cout << "Game over. Tie game." << endl;
            break;
        }
        
        // Switch to the other player after each turn
        if (currentPlayer == 'R') {
            currentPlayer = 'Y';
        } else {
            currentPlayer = 'R';
        }
    }
    return 0;
}
