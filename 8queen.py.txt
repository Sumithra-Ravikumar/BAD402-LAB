def is_safe(board, row, col):
    n = len(board)


    for i in range(row):
        if board[i][col] == 1:
            return False


    i, j = row, col
    while i >= 0 and j >= 0:
        if board[i][j] == 1:
            return False
        i -= 1
        j -= 1


    i, j = row, col
    while i >= 0 and j < n:
        if board[i][j] == 1:
            return False
        i -= 1
        j += 1


    return True


def solve_queens(board, row, solutions):
    n = len(board)


    if row >= n:
        solutions.append([r[:] for r in board])
        return


    for col in range(n):
        if is_safe(board, row, col):
            board[row][col] = 1
            solve_queens(board, row + 1, solutions)
            board[row][col] = 0


def print_board(board):
    for row in board:
        print(" ".join(map(str, row)))
    print()


def print_all_solutions():
    n = 8
    board = [[0] * n for _ in range(n)]
    solutions = []


    solve_queens(board, 0, solutions)


    print(f"Total solutions: {len(solutions)}")
    for i, sol in enumerate(solutions, 1):
        print(f"Solution {i}:")
        print_board(sol)


print_all_solutions()