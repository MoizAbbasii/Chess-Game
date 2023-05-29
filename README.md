# Java Chess Game

## Main Code For Chess Game in java.
This code provides a basic implementation of a chess game. It allows players to enter moves in the format "fromSquare toSquare" (e.g., "e2 e4"). The game continues until the player enters "quit". The board is displayed after each move.

Note that this code does not include the complete rules or logic of chess. It is a simplified version to demonstrate the basic structure and functionality of a chess game. You can further enhance this code to include more advanced rules, move validation, game status tracking, and a graphical user interface if desired.

import java.util.Scanner;

public class ChessGame {
    private static final int BOARD_SIZE = 8;
    private static final String EMPTY = " ";
    private static final String[] INITIAL_BOARD = {
            "r", "n", "b", "q", "k", "b", "n", "r",
            "p", "p", "p", "p", "p", "p", "p", "p",
            EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY,
            EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY,
            EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY,
            EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY, EMPTY,
            "P", "P", "P", "P", "P", "P", "P", "P",
            "R", "N", "B", "Q", "K", "B", "N", "R"
    };

    private static String[][] board;

    public static void main(String[] args) {
        initializeBoard();
        displayBoard();

        Scanner scanner = new Scanner(System.in);
        boolean gameRunning = true;
        while (gameRunning) {
            System.out.print("Enter move (e.g., 'e2 e4'): ");
            String move = scanner.nextLine();

            if (move.equalsIgnoreCase("quit")) {
                gameRunning = false;
            } else {
                boolean validMove = processMove(move);
                if (validMove) {
                    displayBoard();
                } else {
                    System.out.println("Invalid move! Try again.");
                }
            }
        }

        System.out.println("Game over.");
    }

    private static void initializeBoard() {
        board = new String[BOARD_SIZE][BOARD_SIZE];

        for (int rank = 0; rank < BOARD_SIZE; rank++) {
            for (int file = 0; file < BOARD_SIZE; file++) {
                int index = rank * BOARD_SIZE + file;
                board[rank][file] = INITIAL_BOARD[index];
            }
        }
    }

    private static void displayBoard() {
        System.out.println();
        for (int rank = BOARD_SIZE - 1; rank >= 0; rank--) {
            System.out.print(rank + 1 + " ");
            for (int file = 0; file < BOARD_SIZE; file++) {
                System.out.print(board[rank][file] + " ");
            }
            System.out.println();
        }
        System.out.println("  a b c d e f g h");
        System.out.println();
    }

    private static boolean processMove(String move) {
        String[] moveParts = move.trim().split(" ");

        if (moveParts.length != 2) {
            return false;
        }

        String from = moveParts[0];
        String to = moveParts[1];

        if (!isValidSquare(from) || !isValidSquare(to)) {
            return false;
        }

        int fromRank = getRank(from);
        int fromFile = getFile(from);
        int toRank = getRank(to);
        int toFile = getFile(to);

        String piece = board[fromRank][fromFile];
        board[fromRank][fromFile] = EMPTY;
        board[toRank][toFile] = piece;

        return true;
    }

    private static boolean isValidSquare(String square) {
        if (square.length() != 2) {
            return false;
        }

        char fileChar = square.charAt(0);
        char rankChar = square.charAt(1);

        if (fileChar < 'a' || fileChar > 'h' || rankChar < '1' || rankChar > '8') {
            return false;
        }

        return true;
    }

    private static int getRank(String square) {
        char rankChar = square.charAt(1);
        return Character.getNumericValue(rankChar) - 1;
    }

    private static int getFile(String square) {
        char fileChar = square.charAt(0);
        return fileChar - 'a';
    }
}
