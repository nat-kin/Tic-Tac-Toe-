import os

def clear_screen():
    """Clear the terminal screen."""
    os.system('cls' if os.name == 'nt' else 'clear')

def print_board(board):
    """Print the current board with numbers."""
    clear_screen()
    print("=== TIC TAC TOE ===\n")
    print("   1   2   3  ")
    print("1  " + " | ".join(board[0:3]) + "  ")
    print("  ------------")
    print("2  " + " | ".join(board[3:6]) + "  ")
    print("  ------------")
    print("3  " + " | ".join(board[6:9]) + "  ")
    print()

def check_win(board, player):
    """Check if player has won."""
    wins = [(0,1,2), (3,4,5), (6,7,8),   # Rows
            (0,3,6), (1,4,7), (2,5,8),   # Cols
            (0,4,8), (2,4,6)]             # Diags
    return any(board[i] == player and board[j] == player and board[k] == player 
               for i, j, k in wins)

def is_full(board):
    """Check if board is full (draw)."""
    return ' ' not in board

def get_move(board, player):
    """Get valid move from player."""
    while True:
        try:
            row = int(input(f"Player {player} - Enter row (1-3): ")) - 1
            col = int(input(f"Player {player} - Enter col (1-3): ")) - 1
            if row not in range(3) or col not in range(3):
                print("❌ Row/Col must be 1, 2, or 3!")
                continue
            pos = row * 3 + col
            if board[pos] != ' ':
                print("❌ Spot already taken! Try again.")
                continue
            return pos
        except ValueError:
            print("❌ Please enter numbers 1-3!")

def play_game():
    """Play one full game."""
    board = [' '] * 9
    current_player = 'X'
    
    while True:
        print_board(board)
        pos = get_move(board, current_player)
        board[pos] = current_player
        
        if check_win(board, current_player):
            print_board(board)
            print(f"🎉 Player {current_player} WINS!")
            break
        if is_full(board):
            print_board(board)
            print("🤝 It's a DRAW!")
            break
        
        current_player = 'O' if current_player == 'X' else 'X'

def main():
    """Main loop for replays."""
    print("Welcome to Tic Tac Toe!")
    while True:
        play_game()
        replay = input("\nPlay again? (y/n): ").lower()
        if replay != 'y':
            print("Thanks for playing! 👋")
            break

if __name__ == "__main__":
    main()
