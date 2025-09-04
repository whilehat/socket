# server code
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



HOST = input("Enter server IP: ")  
PORT = 9999

client_socket = socket.socket()
client_socket.connect((HOST, PORT))
print("Connected to Player 1")

while True:
    data = client_socket.recv(4096)
    if not data:
        break
    board = pickle.loads(data)
    board.print_board()

 move = int(input(f"Player 2 (O), enter a value (1-{board.size*board.size}): "))
    board.make_move(move, "O")
    client_socket.send(pickle.dumps(board))

winner = board.check_winner()
    if winner:
        print(f"Player {'1' if winner == 'X' else '2'} Wins!")
        break
    elif all(cell != " " for row in board.grid for cell in row):
        print("It's a Draw!")
        break

client_socket.close()

# client code

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



HOST = input("Enter server IP: ")  
PORT = 9999

client_socket = socket.socket()
client_socket.connect((HOST, PORT))
print("Connected to Player 1")

while True:
    data = client_socket.recv(4096)
    if not data:
        break
    board = pickle.loads(data)
    board.print_board()

move = int(input(f"Player 2 (O), enter a value (1-{board.size*board.size}): "))
    board.make_move(move, "O")
    client_socket.send(pickle.dumps(board))

winner = board.check_winner()
    if winner:
        print(f"Player {'1' if winner == 'X' else '2'} Wins!")
        break
    elif all(cell != " " for row in board.grid for cell in row):
        print("It's a Draw!")
        break
client_socket.close()
