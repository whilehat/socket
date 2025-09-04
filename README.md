import socket
import pickle

class Board:
    def __init__(self, size):
        self.size = size
        self.grid = [[" " for _ in range(size)] for _ in range(size)]

    def print_board(self):
        for i in range(self.size):
            print(" " + " | ".join(self.grid[i]))
            if i < self.size - 1:
                print("---+" * (self.size - 1) + "---")

    def check_winner(self):
        n = self.size
        b = self.grid

        for i in range(n):
            if b[i][0] != " " and all(b[i][j] == b[i][0] for j in range(n)):
                return b[i][0]
            if b[0][i] != " " and all(b[j][i] == b[0][i] for j in range(n)):
                return b[0][i]

        if b[0][0] != " " and all(b[i][i] == b[0][0] for i in range(n)):
            return b[0][0]

        if b[0][n-1] != " " and all(b[i][n-1-i] == b[0][n-1] for i in range(n)):
            return b[0][n-1]

        return None

    def make_move(self, value, symbol):
        row = (value - 1) // self.size
        col = (value - 1) % self.size

        if row < 0 or row >= self.size or col < 0 or col >= self.size:
            print("Invalid move! Out of range.")
            return False

        if self.grid[row][col] == " ":
            self.grid[row][col] = symbol
        else:
            print("Cell already taken!")
            return False

        self.print_board()
        return True



HOST = "0.0.0.0"   
PORT = 9999

size = int(input("Enter board size"))
board = Board(size)

server_socket = socket.socket()
server_socket.bind((HOST, PORT))
server_socket.listen(1)
print("Waiting for Player 2 to connect...")

conn, addr = server_socket.accept()
print(f"Player 2 connected from {addr}")

board.print_board()

for i in range(1, size*size + 1):
    if i % 2 == 1:  
        move = int(input(f"Player 1 (X), enter a value (1-{size*size}): "))
        board.make_move(move, "X")
        conn.send(pickle.dumps(board))

    else:  
        print("Waiting for Player 2 move...")
        data = conn.recv(4096)
        board = pickle.loads(data)
        board.print_board()

    winner = board.check_winner()
    if winner:
        print(f"Player {'1' if winner == 'X' else '2'} Wins!")
        break
    elif all(cell != " " for row in board.grid for cell in row):
        print("It's a Draw!")
        break

conn.close()
server_socket.close()
