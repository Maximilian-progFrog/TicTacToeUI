import javax.swing.*; // Importing the Swing library for creating the graphical user interface
import java.awt.*; // Importing the AWT library for layout and color settings
import java.awt.event.ActionEvent; // For handling button click events
import java.awt.event.ActionListener; // Interface for listening to button clicks
import java.util.Random; // For generating random numbers (used for the computer's moves)

// Main class for the Tic Tac Toe game
public class TicTacToeGUI extends JFrame implements ActionListener {
    private final JButton[] buttons = new JButton[9]; // Array to store 9 buttons for the 3x3 grid
    private boolean playerTurn = true; // Boolean to track if it's the player's turn (true) or computer's turn (false)
    private final char playerSymbol = 'X'; // Symbol used by the player
    private final char computerSymbol = 'O'; // Symbol used by the computer

    // Constructor for setting up the game window
    public TicTacToeGUI() {
        // Set the title of the game window
        setTitle("Tic Tac Toe");

        // Set the size of the window (400x400 pixels)
        setSize(400, 400);

        // Make sure the program exits when the window is closed
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        // Use a grid layout to arrange buttons in a 3x3 grid
        setLayout(new GridLayout(3, 3));

        // Create 9 buttons for the game board
        for (int i = 0; i < 9; i++) {
            buttons[i] = new JButton(""); // Create a new empty button
            buttons[i].setFont(new Font("Arial", Font.BOLD, 40)); // Set font size and style
            buttons[i].setFocusPainted(false); // Remove focus border on the button
            buttons[i].addActionListener(this); // Add an event listener to handle button clicks
            add(buttons[i]); // Add the button to the window
        }

        // Make the game window visible
        setVisible(true);
    }

    // This method is called whenever a button is clicked
    @Override
    public void actionPerformed(ActionEvent e) {
        // Get the button that was clicked
        JButton clickedButton = (JButton) e.getSource();

        // Check if it's the player's turn and the button is empty
        if (playerTurn && clickedButton.getText().equals("")) {
            clickedButton.setText(String.valueOf(playerSymbol)); // Set the player's symbol ('X') on the button
            playerTurn = false; // Switch to computer's turn

            // Check if the player has won or if it's a draw
            if (checkWinner(playerSymbol)) {
                JOptionPane.showMessageDialog(this, "You win!"); // Show a popup message
                resetGame(); // Restart the game
                return;
            } else if (isDraw()) {
                JOptionPane.showMessageDialog(this, "It's a draw!"); // Show a draw message
                resetGame(); // Restart the game
                return;
            }

            // Let the computer make a move
            computerMove();
        }
    }

    // This method lets the computer make a random move
    private void computerMove() {
        Random random = new Random(); // Create a Random object to pick a random move
        int move;

        // Keep picking a random button until an empty one is found
        do {
            move = random.nextInt(9); // Generate a random number between 0 and 8
        } while (!buttons[move].getText().equals("")); // Repeat if the button is not empty

        buttons[move].setText(String.valueOf(computerSymbol)); // Set the computer's symbol ('O') on the button

        // Check if the computer has won or if it's a draw
        if (checkWinner(computerSymbol)) {
            JOptionPane.showMessageDialog(this, "Computer wins!"); // Show a popup message
            resetGame(); // Restart the game
        } else if (isDraw()) {
            JOptionPane.showMessageDialog(this, "It's a draw!"); // Show a draw message
            resetGame(); // Restart the game
        }

        playerTurn = true; // Switch back to the player's turn
    }

    // This method checks if a player (or the computer) has won
    private boolean checkWinner(char symbol) {
        // Check all possible winning combinations: rows, columns, and diagonals
        return (buttons[0].getText().equals(String.valueOf(symbol)) &&
                buttons[1].getText().equals(String.valueOf(symbol)) &&
                buttons[2].getText().equals(String.valueOf(symbol))) || // Top row
                (buttons[3].getText().equals(String.valueOf(symbol)) &&
                        buttons[4].getText().equals(String.valueOf(symbol)) &&
                        buttons[5].getText().equals(String.valueOf(symbol))) || // Middle row
                (buttons[6].getText().equals(String.valueOf(symbol)) &&
                        buttons[7].getText().equals(String.valueOf(symbol)) &&
                        buttons[8].getText().equals(String.valueOf(symbol))) || // Bottom row
                (buttons[0].getText().equals(String.valueOf(symbol)) &&
                        buttons[3].getText().equals(String.valueOf(symbol)) &&
                        buttons[6].getText().equals(String.valueOf(symbol))) || // Left column
                (buttons[1].getText().equals(String.valueOf(symbol)) &&
                        buttons[4].getText().equals(String.valueOf(symbol)) &&
                        buttons[7].getText().equals(String.valueOf(symbol))) || // Middle column
                (buttons[2].getText().equals(String.valueOf(symbol)) &&
                        buttons[5].getText().equals(String.valueOf(symbol)) &&
                        buttons[8].getText().equals(String.valueOf(symbol))) || // Right column
                (buttons[0].getText().equals(String.valueOf(symbol)) &&
                        buttons[4].getText().equals(String.valueOf(symbol)) &&
                        buttons[8].getText().equals(String.valueOf(symbol))) || // Diagonal 1
                (buttons[2].getText().equals(String.valueOf(symbol)) &&
                        buttons[4].getText().equals(String.valueOf(symbol)) &&
                        buttons[6].getText().equals(String.valueOf(symbol)));  // Diagonal 2
    }

    // This method checks if the game is a draw (no empty buttons left)
    private boolean isDraw() {
        for (JButton button : buttons) {
            if (button.getText().equals("")) {
                return false; // If any button is empty, it's not a draw
            }
        }
        return true; // All buttons are filled
    }

    // This method resets the game board for a new game
    private void resetGame() {
        for (JButton button : buttons) {
            button.setText(""); // Clear the text on all buttons
        }
        playerTurn = true; // Reset to the player's turn
    }

    // Main method to start the game
    public static void main(String[] args) {
        new TicTacToeGUI(); // Create and show the game window
    }
}
